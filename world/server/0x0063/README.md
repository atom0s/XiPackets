# `Packet: 0x0063`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0063` |
| **Size**                  | `(varies)` |

> [!NOTE]
> The name of the command and client handler for this packet are unknown.

## Description

This packet is sent by the server to inform the client of multiple different kinds of information.

## Packet Layout

The initial layout is as follows:

```cpp
struct packet_t
{
    uint16_t    id     : 9;
    uint16_t    size   : 7;
    uint16_t    sync;

    uint16_t    type;       // PS2: (New; did not exist.)
    uint16_t    unknown06;  // PS2: (New; did not exist.)
    uint8_t     data[];     // PS2: (New; did not exist.)
};
```

The layout of this packet will vary based on the value sent for the `type` field.

### `type`

_The packets sub-type enumeration value which determines the content that will be contained inside of `data`._

```cpp
enum class pkt_type
{
    merits          = 0x02,
    monstrosity1    = 0x03,
    monstrosity2    = 0x04,
    job_points      = 0x05,
    homepoints      = 0x06,
    unity           = 0x07,
    status_icons    = 0x09,
    unknown_0A      = 0x0A,
};
```

### `unknown06`

_This value is unused._

The client does not directly use this value. However, it has been observed that this value holds the size of the `data` field.

### `data`

_The actual data of the packet. Its contents will vary based on the `type` field value._

---

## Type: `0x02` - Merit / Limit Point Information

The packet `data` field contains information about the players merit and limit points.

> [!NOTE]
> The client does not use all the information sent with this packet type!

```cpp
PTR_status_data.LimitPoints     = *(static_cast<uint32_t*>(pkt) + 2); // 0x08
PTR_status_data.MeritPointsMax  = *(static_cast<uint32_t*>(pkt) + 3); // 0x0C
PTR_status_data.DataLoadedFlags |= 2;
```

## Type: `0x03` - Monstrosity Information (1)

The packet `data` field contains information about the players monstrosity data. (1)

The client copies the entire block of data sent with the packet into a local buffer used elsewhere to populate the clients monstrosity information.

```cpp
qmemcpy(&PTR_MonstrosityData1, pkt + 8, 0xD4);
```

The layout of the `PTR_MonstrosityData1` is as follows:

```cpp
struct monstrosity1_t
{
    uint32_t    data;
    uint32_t    flags;
    uint16_t    unknown00; // Unused.
    uint16_t    infamy;
    uint32_t    prestige;
    uint32_t    unknown01; // Unused.
    uint8_t     instincts[64];
    uint8_t     levels[128];
};
```

### `data`

_Multi-purpose value that holds more than one piece of information._

The client uses this value to hold the monstrosity item and race information about the player:

  - The item id is pulled from this value using: `static_cast<uint16_t>(PTR_MonstrosityData1.data & 0x01FF) - 0x1000`
  - The race id is pulled from this value using: `static_cast<uint32_t>(PTR_MonstrosityData1.data >> 0x0012) & 0x0F`

### `flags`

_The flags related to the current monstrosity information._

The client uses this value to determine the current monstrosity tier and rank.

  - The is rank pulled from this value using: `(PTR_MonstrosityData1.flags >> 0x02) & 0x03`
  - The is tier pulled from this value using: `(PTR_MonstrosityData1.flags & 0x03)`

When the rank is being read, it's value is translated into a string that can be one of the following values:

  - `Gla` _(Gladiator)_
  - `CG` _(Champion Gladiator)_
  - `HCG` _(High Champion Gladiator)_

### `unknown00`

_This value is unused._

### `infamy`

_The current amount of infamy._

There is an unused function that is left in the compiled client binary that may help determine the ranges used with this value.

```cpp
bool FUNC_UnusedMonstrosityHelper()
{
    if ((PTR_MonstrosityData1.flags & 3) == 0)
        return 1;

    if ((PTR_MonstrosityData1.flags & 3) == 1)
    {
        if (PTR_MonstrosityData1.infamy >= 5000)
            return 1;
    }
    else if ((PTR_MonstrosityData1.flags & 3) == 2 && PTR_MonstrosityData1.infamy >= 10000)
        return 1;

    return 0;
}
```

### `prestige`

_The current amount of prestige._

This value is used to display the players current amount of prestige within the `Main Menu -> Status -> Profile` menu. The client reads and displays this value as:

```cpp
sprintf(buffer, "%d", PTR_MonstrosityData1.prestige & 0xFFFF);
```

### `unknown01`

_This value is unused._

### `instincts`

_The players instinct information. (Bitfield.)_

### `levels`

_The players monstrosity levels._

## Type: `0x04` - Monstrosity Information (2)

The packet `data` field contains information about the players monstrosity data. (2)

The client copies the entire block of data sent with the packet into a local buffer used elsewhere to populate the clients monstrosity information.

```cpp
qmemcpy(&PTR_MonstrosityData2, pkt + 8, 0xAC);
PTR_MonstrosityData2_Loaded = 1;
```

The layout of the `PTR_MonstrosityData2` is as follows:

```cpp
struct monstrosity2_t
{
    uint8_t     unknown00[128];
    uint32_t    unknown01[3];
    uint32_t    unknown02[8];
};
```

### `unknown00`

_Unknown._

This value appears to be used as a secondary `level` array.

### `unknown01`

_Unknown._

This value appears to be used as a secondary `instincts` array.

### `unknown02`

_Unknown._

This value appears to be used for `variant` related information.

## Type: `0x05` - Job Point Information

The packet `data` field contains information about the players job points.

The client copies the entire block of data sent with the packet into a local buffer to be used elsewhere then, if set, will call the `JobPointsFunc` callback to reinitialize the points menu.

```cpp
qmemcpy(&PTR_status_data.JobPoints, pkt + 8, sizeof(PTR_status_data.JobPoints));
PTR_status_data.DataLoadedFlags |= 4;

if (zone->PointsSys.JobPointsFunc)
{
    for (auto i = 0; i <= 22; ++i)
        zone->PointsSys.JobPointsFunc(i);
}
```

The `JobPoints` structure is defined in the client as:

```cpp
struct jobpointentry_t
{
    uint16_t        capacity_points;
    uint16_t        points;
    uint16_t        points_spent;
};

struct jobpointsinfo_t
{
    uint8_t         flags;
    uint8_t         unknown00[3];
    jobpointentry_t jobs[24];
};
```

The `flags` value determines if the player has job points unlocked.

This is checked via: `jpoints->flags & 1`

## Type: `0x06` - Homepoint Bit Masks

The packet `data` field contains information about the players homepoints.

The client copies the entire block of data sent with the packet into a local buffer to be used elsewhere.

```cpp
qmemcpy(PTR_status_data.HomepointMasks, pkt + 8, sizeof(PTR_status_data.HomepointMasks));
PTR_status_data.DataLoadedFlags |= 8;
```

The `HomepointMasks` structure is defined in the client as:

```cpp
struct teleportmasks_t
{
    uint32_t home_point[4];
    uint32_t survival_guide[4];
    uint32_t waypoint[4];
    uint32_t telepoint;
    uint32_t atmos;
    uint32_t eschan_portal;
    uint32_t unknown00;     // Unsure if Eschal Portal uses this as well.
};
```

Internally, the client handles the various teleport methods in the following enumeration:

```cpp
enum teleport_type
{
    home_point          = 0x0, // Home Point
    telepoint           = 0x1, // Telepoint
    survival_guide      = 0x2, // Survival Guide
    waypoint            = 0x3, // Waypoint
    beginner            = 0x4, // Beginner
    zone_change         = 0x5, // ZoneChange
    atmos               = 0x6, // Atmos
    up                  = 0x7, // Up
    down                = 0x8, // Down
    atmos_s             = 0x9, // Atmos(S)
    eschan_portal       = 0xA, // Eschan Portal
    ethereal_ingress    = 0xB, // Ethereal Ingress
};
```

When the client is rendering a map in a state that will show the various types of teleport icons with their names, it will use the following helper functions to determine if the teleport is unlocked. It uses the above `teleport_type` enumeration to determine which helper function to use and which string will be displayed next to the icon.

```cpp
bool __cdecl FUNC_HasHomePoint(int bit)
{
    const uint8_t* mask = FUNC_gcTeleportsDataGet();
    return mask && ((1 << (bit % 32)) & *(uint32_t*)&mask[4 * (bit / 32)]) != 0;
}
bool __cdecl FUNC_HasTelepoint(int bit)
{
    const uint8_t* mask = FUNC_gcTeleportsDataGet();
    return mask && ((1 << (bit % 32)) & *(uint32_t*)&mask[4 * (bit / 32) + 48]) != 0;
}
bool __cdecl FUNC_HasSurvivalGuide(int bit)
{
    const uint8_t* mask = FUNC_gcTeleportsDataGet();
    return mask && ((1 << (bit % 32)) & *(uint32_t*)&mask[4 * (bit / 32) + 16]) != 0;
}
bool __cdecl FUNC_HasWaypoint(int bit)
{
    const uint8_t* mask = FUNC_gcTeleportsDataGet();
    return mask && ((1 << (bit % 32)) & *(uint32_t*)&mask[4 * (bit / 32) + 32]) != 0;
}
bool __cdecl FUNC_HasAtmosS(int bit)
{
    const uint8_t* mask = FUNC_gcTeleportsDataGet();
    return mask && ((1 << (bit % 32)) & *(uint32_t*)&mask[4 * (bit / 32) + 52]) != 0;
}
bool __cdecl FUNC_HasEschanPortal(int bit)
{
    const uint8_t* mask = FUNC_gcTeleportsDataGet();
    return mask && ((1 << (bit % 32)) & *(uint32_t*)&mask[4 * (bit / 32) + 56]) != 0;
}
```

## Type: `0x07` - Unity Information

The packet `data` field contains information about the players unity information.

The client copies the entire block of data sent with the packet into a local buffer to be used elsewhere. The data sent is chunked into separate packets that fill in multiple blocks.

```cpp
const auto val1 = pkt[8] != 0;
const auto val2 = pkt[9] * 32;

qmemcpy((char *)&PTR_UnityData + 4096 * val1 + 4 * val2, pkt + 12, 0x80);

if (val2 + 32 < 1024)
    return;

PTR_UnityFlag1[val1] = 1;
PTR_UnityFlag2[val2] = 1;
```

> [!NOTE]
> TODO: PTR_UnityData is currently not reversed due to lack of retail access/captures.

## Type: `0x09` - Status Icons

The packet `data` field contains information about the players status icons.

The client copies the entire block of data sent with the packet into a local buffer to be used elsewhere.

```cpp
qmemcpy(PTR_status_data.StatusIcons, pkt + 8, 0xC0);
PTR_status_data.DataLoadedFlags |= 0x10;
```

The layout of the `PTR_status_data.StatusIcons` is as follows:

```cpp
struct statusicons_t
{
    int16_t     icon[32];
    uint32_t    timestamp[32];
};
```

> [!IMPORTANT]
> The timestamp value here overflows! This is intentional and is handled by the client correctly.

Timestamp values that are intended to be infinite or hidden are set to `0x7FFFFFFF`.

## Type: `0x0A` - Unknown

The packet `data` field contains unknown information.

_It does not appear this data is used by the client any longer. The function to return and use this information is not referenced any longer._
