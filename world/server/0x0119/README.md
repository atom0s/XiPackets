# `Unknown - Packet: 0x0119`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0119` |
| **Size**                  | `0x0104` |

## Description

This packet is sent by the server to update the clients ability (and mount) recast information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct recasttimer_t
{
    uint16_t    Timer;
    uint8_t     Calc1;
    uint8_t     TimerId;
    uint16_t    Calc2;
    uint16_t    padding00;
};

// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t        id: 9;
    uint16_t        size: 7;
    uint16_t        sync;
    recasttimer_t   Timers[31];
    uint32_t        MountRecast;
    uint32_t        MountRecastId;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Timers`

_The clients recast timer information._

### `MountRecast`

_The clients mount recast timer._

### `MountRecastId`

_The clients mount recast timer id._

This value is usually always 0.

## Structure Fields (`recasttimer_t`)

### `Timer`

_The base recast timer value._

### `Calc1`

_The recast timers extra calculated value. (1)_

### `TimerId`

_The recast timer id._

### `Calc2`

_The recast timers extra calculated value. (2)_

### `padding00`

_Padding; unused._

## Additional Information

The server sends this packet to the client each time there is a need to update the clients ability _(and mount)_ recast information. The client uses this packet to populate and/or update the current ability recast information and mount recast information. The information contained within each `Timers` entry is used to determine both the abilities recast time as well as any available charges information _(if applicable)_ about the ability.

When the client first receives this packet, it will store the `Timers` information into an unused buffer `status_data.AbilityInfo`. This buffer is only used as a temporary storage for the recast information and is basically just a snapshot of the last time the data was received from the server. Afterward, the client will then copy the information from this buffer into the main recast buffer. The client updates the recast `Time` value at this point as `time = 60 * Time;` and stores it into the main recast table.

The client will then update the mount recast information using the `MountRecast` and `MountRecastId` values. The manner in which this is done within the client is as follows:

```cpp
mount->timer    = pkt->MountRecast;                 // Stores the base mount recast time.
mount->id       = pkt->MountRecastId;               // Stores the mount recast id.
mount->recast   = (pkt->MountRecast & 0xFFFF) * 60; // Stores the calculated recast value.
```

When the client needs to lookup an abilities recast time or charges, it has two helper functions that handle the math required to properly calculate either value.

The first helper function is used to return the recast timer. In order to do this, the function needs a few pieces of information, including the recast timer id, the charges the ability can have, and the base timer value. This helper function looks like this:

```cpp
uint32_t FUNC_Recast_GetAbilityRecastTime(recasttbl_t* this, uint32_t id, uint32_t charges, uint32_t base_time)
{
    if (id > MAX_ABILITY_ID || !g_pKaDataAbility)
        return 0;

    const auto idx = FUNC_Recast_GetAbilityIndexByTimerId(this, g_pKaDataAbility[id].RecastTimerId);
    if (idx == 255)
        return 0;

    if (!charges)
        return (this->timers[idx] + 59) / 60;

    if (this->entries[idx].Calc2)
        base_time += this->entries[idx].Calc2;

    const auto val1 = ((base_time - (this->entries[idx].Calc1 & 0x3F) * 0.016666668 * base_time) / charges);

    if (((this->timers[idx] + 59) / 60 % val1) == 0)
    {
        if (this->timers[idx])
            return val1;
    }

    return result;
}
```

The second helper function is used to return the number of charges available from the recast timer information. This helper function looks like this:

```cpp
uint32_t FUNC_Recast_GetAbilityCharges(recasttbl_t* this, uint32_t id, uint32_t charges, uint32_t base_time)
{
    if (id > MAX_ABILITY_ID || !charges || !g_pKaDataAbility)
        return 0;

    const auto idx = FUNC_Recast_GetAbilityIndexByTimerId(this, g_pKaDataAbility[id].RecastTimerId);
    if (idx == 255)
        return 0;

    if (this->entries[idx].Calc2)
        base_time += this->entries[idx].Calc2;

    const auto val1 = (base_time - (this->entries[idx].Calc1 & 0x3F) * 0.016666668 * base_time);
    return (val1 - (this->timers[idx] + 59) / 60) / (val1 / charges);
}
```
