# `Unknown - Packet: 0x00E6`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00E6` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the server to inform the client of information related to PvP content. _(ie. Ballista)_

## Packet Layout

The initial layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    Flags   : 6;    // PS2: (New; did not exist.)
    uint16_t    Mode    : 10;   // PS2: (New; did not exist.)
    uint8_t     Data[];         // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Flags`

_The packet flags._

The client only checks if this value is set to `1`. If not, then the handler will exit without processing the rest of the packet data. No other value is checked for/against.

### `Mode`

_The packet mode._

This value is used to determine the current packet mode, which allows the client to know what kind of information is stored in the `Data` field.

| Mode | Purpose |
| --- | --- |
| `0` | _Unknown. (Causes an unhandled fall-through in the packet handler that just returns.)_ |
| `1` | _Unknown. Updates two global state values that the client never uses elsewhere._ |
| `2` | _Used to update the Ballista scoreboard information and other Ballista values._ |
| `3` | _Used in response to the player using `/scout` to look for the nearest Rook._ |

_Values above 3 will be treated the same as `Mode` 0, the client will simply exit the handler without doing anything._

### `Data`

_The packet data._

This value will change based on the packet `Mode` being handled.

_Please see the notes below regarding how the `Data` field is populated. It will differ based on the `Mode` and its overall purpose!_

## Additional Information

The server will use this packet for multiple purposes within Ballista. The two main modes of this packet that the client uses are to update the scoreboard and other Ballista related values and another mode which is a response to a clients `/scout` query looking for the nearest Rook. Below you can find additional information in regards to each mode as well as the packet breakdowns for the individual modes as the `Data` within the packet changes per-mode.

## Mode: `1` (Unknown State Update)

This mode causes the handler to simply update two global state values, then exit. This mode has not been observed in any packet captures and the values the client store are not used anywhere else in the client. Due to this, this mode is currently considered unused and was likely left over for debugging purposes or some other cause.

```cpp
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    Flags   : 6;    // PS2: (New; did not exist.)
    uint16_t    Mode    : 10;   // PS2: (New; did not exist.)
    uint16_t    padding06;      // PS2: (New; did not exist.)
    uint32_t    unknown08;      // PS2: (New; did not exist.)
    uint32_t    unknown0C;      // PS2: (New; did not exist.)
};
```

### `padding06`

_Padding; unused._

### `unknown08`

_Unknown._

### `unknown0C`

_Unknown._

## Mode: `2` (General Scoreboard / Information Update)

This mode is the most commonly used mode for this packet as it is used to update the various Ballista scoreboard values and other general Ballista information. Any time there is an update to the scoreboards information _(ie. a team scores a point, a team wins a match, the match set changes, the player obtains a Petra, etc.)_ or the player obtains a status within the game mode _(ie. Gate Breach during Ballista, Flamme Carrier during Brenner, etc.)_ this packet will be sent to the client. If the player dies they will be sent this packet upon respawning to reset their Petra count.

This packet will also be sent any time a match starts or ends, both for the overall match or between sets. This is to reset the scoreboard as well as to initialize the scoreboards mode and flags.

```cpp
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    Flags   : 6;    // PS2: (New; did not exist.)
    uint16_t    Mode    : 10;   // PS2: (New; did not exist.)
    uint16_t    padding06;      // PS2: (New; did not exist.)
    int32_t     PetraCount;     // PS2: (New; did not exist.)
    int32_t     Score[3];       // PS2: (New; did not exist.)
    int16_t     Scoreboard[2];  // PS2: (New; did not exist.)
    uint8_t     MatchPoints[3]; // PS2: (New; did not exist.)
    uint8_t     MatchSet;       // PS2: (New; did not exist.)
    uint32_t    Flammes;        // PS2: (New; did not exist.)
    int32_t     FlammeFlg;      // PS2: (New; did not exist.)
};
```

### `padding06`

_Padding; unused._

### `PetraCount`

_The local clients current Petra count._

### `Score`

_The array of team scores._

This array holds the current score information for the active teams within the Ballista match. The order these are stored in are the always in the order of the team ids. While one of the `Scoreboard` values features a team swap flag, the order of the score information in this array does not change.

### `Scoreboard`

_The scoreboard flags and properties._

This array holds two values used to configure the scoreboard settings. This will also depend on the local players current Ballista flags and team information. _(`XiAtelBuff::BallistaFlags`, `XiAtelBuff::BallistaInfo`)_

When the client first receives this packet, it will check to see if `Scoreboard[0] >= 3`, if true then it is forced to 2, clamping its value. The `Scoreboard[1]` value is used as-is without any kind of clamping. The manner in which these values are then used will vary based on a handful of conditions and combinations of values.

The main purpose of the `Scoreboard[0]` value is to handle ensuring the local players team is always shown on the left side of the scoreboard. This value is used to flip the scoreboard as well as flip and swap to the 3rd score/match point values.

  - `0` = _Normal_
  - `1` = _Flip Teams_
  - `2` = _Team 3 Mode (This will flip the teams and use `Score[2]` and `MatchPoints[2]` values.)_

The second `Scoreboard[1]` value is used for multiple purposes depending on various conditions. This is used to do things such as:

  - Hide rendering the scoreboard.
  - Change the opposing teams icon.
  - Swap the game icon mode.

The following pseudo code shows how these values work at the start of the scoreboard renderer function:

```cpp
    auto lteamicon      = 0;
    auto rteamicon      = 0;
    auto scoreboard1    = *reinterpret_cast<uint16_t*>(&player->BallistaInfo[16]); // pkt->Scoreboard[0] (Clamped to 2, if required.)
    auto scoreboard2    = *reinterpret_cast<uint16_t*>(&player->BallistaInfo[18]); // pkt->Scoreboard[1]

    if ((scoreboard2 & 0x07) == 0)
    {
        lteamicon = scoreboard1 + 5;
        rteamicon = (scoreboard1 == 0) + 5;
        goto skip_ahead;
    }

    if ((player->BallistaFlags - 2) > 2)
        return;

    switch (scoreboard2 & ~(1 << (player->BallistaFlags - 2)))
    {
    case 2:
        rteamicon = 1;
        break;
    case 4:
        rteamicon = 2;
        break;
    default:
        rteamicon = 0;
        break;
    }

skip_ahead:

    const auto mset = player->BallistaInfo[23] + 1; // pkt->MatchSet
    if (mset / 2)
    {
        // Print the current Match Set information..
    }

    const auto lteam_mpts   = scoreboard1 >= 3 ? 0 : player->BallistaInfo[scoreboard1 + 20];
    const auto rteam_mpts   = scoreboard1 >= 3 ? 0 : player->BallistaInfo[(scoreboard1 == 0) + 20];
    const auto lteam_pts    = scoreboard1 >= 3 ? 0 : *reinterpret_cast<uint32_t*>(&player->BallistaInfo[4 * scoreboard1 + 4]);
    const auto rteam_pts    = scoreboard1 >= 3 ? 0 : *reinterpret_cast<uint32_t*>(&player->BallistaInfo[4 * (scoreboard1 == 0) + 4]);

    // Draw the left team information.. (match points, team icon, points)
    if (lteam_mpts > 1) YmMenuShape::Draw(...);
    if (lteam_mpts > 0) YmMenuShape::Draw(...);
    sprintf(buffer, "%c", lteamicon + 0x9E);
    YkDrawString(.., buffer, ..);

    // Draw the team points score..
    sprintf(buffer, " %d -%d ", lteam_pts, rteam_pts);
    YkDrawString(.., buffer, ..);

    // Draw the right team information.. (match points, team icon, points)
    if (rteam_mpts > 1) YmMenuShape::Draw(...);
    if (rteam_mpts > 0) YmMenuShape::Draw(...);
    sprintf(buffer, "%c", rteamicon + 0x9E);
    YkDrawString(.., buffer, ..);

    // ..snipped..

    // Draw the Flamme scoreboard..
    // Draw the Petra icon..
    // Draw the players Petra count..
    // Draw the players Gate Breach Status star icon..
```

### `MatchPoints`

_The array of team match points._

This array holds the current match points for the active teams within the Ballista match. The order these are stored in are the always in the order of the team ids. While one of the `Scoreboard` values features a team swap flag, the order of the score information in this array does not change.

_These values are for the yellow balls that show the match wins per team in multi-match games._

### `MatchSet`

_The current match set._

This value holds the current match set value. The client treats this value as a multiple of 2. When displaying the set count, it will use `MatchSet / 2`.

### `Flammes`

_The current Flamme status information used in Brenner._

This value is used to hold the current Flammen-Brenner scoreboard information. These are the red dots that show under the team points and icons on the scoreboard when in Brenner. They have two states being dark/dim and being lit up red showing the status of each teams Flammen-Brenner towers. This value is treated as a bit array in the following manner:

| Bit | Purpose |
| ---: | --- |
| `0 (0x00)`    | _Light Flamme Icon_ |
| `1 (0x01)`    | _(unused)_ |
| `2 (0x02)`    | _Show Flamme Icon_ |
| `3 (0x03)`    | _Light Flamme Icon_ |
| `4 (0x04)`    | _(unused)_ |
| `5 (0x05)`    | _Show Flamme Icon_ |
| `6 (0x06)`    | _Light Flamme Icon_ |
| `7 (0x07)`    | _(unused)_ |
| `8 (0x08)`    | _Show Flamme Icon_ |
| `9 (0x09)`    | _Light Flamme Icon_ |
| `10 (0x0A)`   | _(unused)_ |
| `11 (0x0B)`   | _Show Flamme Icon_ |
| `12 (0x0C)`   | _Light Flamme Icon_ |
| `13 (0x0D)`   | _(unused)_ |
| `14 (0x0E)`   | _Show Flamme Icon_ |
| `15 (0x0F)`   | _(unused)_ |
| `16 (0x10)`   | _Light Flamme Icon_ |
| `17 (0x11)`   | _(unused)_ |
| `18 (0x12)`   | _(unused)_ |
| `19 (0x13)`   | _Light Flamme Icon_ |
| `20 (0x14)`   | _(unused)_ |
| `21 (0x15)`   | _(unused)_ |
| `22 (0x16)`   | _Light Flamme Icon_ |
| `23 (0x17)`   | _(unused)_ |
| `24 (0x18)`   | _(unused)_ |
| `25 (0x19)`   | _Light Flamme Icon_ |
| `26 (0x1A)`   | _(unused)_ |
| `27 (0x1B)`   | _(unused)_ |
| `28 (0x1C)`   | _Light Flamme Icon_ |
| `29 (0x1D)`   | _(unused)_ |
| `30 (0x1E)`   | _(unused)_ |
| `31 (0x1F)`   | _(unused)_ |

  - `Light Flamme Icon` - This means the icon will be lit up red.
  - `Show Flamme Icon` - This means the icon will be visible. _(Default is dim/grey.)_

The client can show up to 5 flamme icons, however only 4 are usually shown and used in Brenner. When rendering 5, the client will over-render the icons due to not having correct spacing for the 5th icon. The client will show the icons in pairs (one on each teams side) when toggling a `Show Flamme Icon` bit. The way the client reads these bits causes some glitchy behavior if the bits are not used in order starting from 0. While you can cause the client to render icons without using the lowest bits first, it wont properly fill light them up if you skip bits.

The bits used to light up the icons are done in a specific grouping of bits to handle either team.

  - Bits 0 to 15 will handle the first team. _(As well as enabling the icons in general.)_
  - Bits 16 to 28 will handle the second team.

The client will also flip which side the icons are rendered for based on the `Scoreboard[0]` value, similar to flipping the team icons and score.

### `FlammeFlg`

_Flag set if the client is currently holding a captured Flamme._

This value is used to mark the player as currently holding a captured Flamme. This is used to tell the client to change the clients nameplate icon to the flamme rock icon.

The client will also require that the players team id is changed to either `0x06` or `0x07` in order to display the flamme icon. This team value is handled in the players name flags sent in other packets.

  - `0x000D` - _RecvCharPc_
  - `0x000E` - _RecvCharNpc_
  - `0x0037` - _RecvServerStatus_

## Mode: `3` (Scout Response)

This mode is used to respond to the client sending a `/scout` request, looking for the nearest Rook, during Ballista. The server will respond with this packet either to tell the client where the nearest Rook is located, or will tell the client there is no Rook nearby. _(When a Rook is not within packet view distance of the client, 50 yalms.)_

```cpp
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    Flags   : 6;        // PS2: (New; did not exist.)
    uint16_t    Mode    : 10;       // PS2: (New; did not exist.)
    uint16_t    padding06;          // PS2: (New; did not exist.)
    uint32_t    RookUniqueNo;       // PS2: (New; did not exist.)
    uint32_t    padding0C;          // PS2: (New; did not exist.)
    float       RookPosition[4];    // PS2: (New; did not exist.)
    float       RookDistance;       // PS2: (New; did not exist.)
};
```

### `padding06`

_Padding; unused._

### `RookUniqueNo`

_The server id of the nearest Rook, if valid._

This value will be set to 0 if there is no nearby Rook.

### `padding0C`

_Padding; unused._

### `RookPosition`

The Rooks position.

This value is the Rooks XZY coords. The 4th value of the information is a copy of the `RookDistance` value. The `RookPosition` information is used to determine the compass direction that should be displayed when telling the client where the Rook is located.

### `RookDistance`

The Rooks distance between itself and the player.

This value is the distance between the player and the Rook. However, this value is calculated differently than a normal 2D or 3D distance calculation. It also doesn't use the normal distance handling that XI uses which is normally `math.sqrt(entity->Distance)`. Instead, the client will calculate this value from the packet as:

```cpp
const auto dist = pkt->RookDistance * 0.91399997 + 0.5;
```

The way this value is calculated into the packet is:

```lua
-- Example using Ashita v4 addon coding..
local player = GetPlayerEntity();
local target = get_entity_by_server_id(pkt->RookUniqueNo);

-- Calculate the 3D distance between the player and Rook..
local dist = math.distance3d(
    player.Movement.LocalPosition.X, player.Movement.LocalPosition.Z, player.Movement.LocalPosition.Y,
    target.Movement.LocalPosition.X, target.Movement.LocalPosition.Z, target.Movement.LocalPosition.Y
    );

-- Adjust for packet..
pkt->RookDistance = (dist - 0.5) / 0.913999997;
```

## Additional Information - Gate Breach Status

During Ballista, the character will earn Gate Breach status after killing another player. This is required in order to deposit their Petras in a Rook to score for their team. Gate Breach status is not handled by this packet, but it feels important to note how it works in regards to the information above as it relates directly to it. This status is instead handled by the players render information (name flags). The client will handle and set this status via the following packets:

  - `0x000D` - _RecvCharPc_
  - `0x000E` - _RecvCharNpc_
  - `0x0037` - _RecvServerStatus_

This value is stored in the entities `entity->Render.Flags4` bits, stored at bit 9.

  - `const auto has_gatebreach = entity->Render.Flags4 & 0x100;`
