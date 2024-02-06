# `GP_SERV_COMMAND_MISSION`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_MISSION` |
| **Client Handler**        | `RecvMissionItem` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0056` |
| **Size**                  | `0x0028` |

## Description

This packet is sent by the server to populate the clients mission and quest information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_MISSION
struct GP_SERV_MISSION
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    data[8];    // PS2: data
    uint16_t    Port;       // PS2: Port
    uint16_t    padding00;  // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_MISSION`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `data`

_The mission and quest data._

### `Port`

_The packet type._

### `padding00`

_Padding; unused._

## Additional Information

The server sends multiple copies of this packet to the client to fully populate its mission and quest information. The manner in which this packet is handled is based on the packets `Port` value which determines what information is being populated. Due to how the handler works, the `Port` value is used as both a hard-coded value the client will check for, but also an offset into different fields of the clients `MISSION_DATA` structure which holds all of the clients mission and quest data.

The `MISSION_DATA` structure is defined as follows:

```cpp
// PS2: MISSION_DATA
struct MISSION_DATA
{
    uint32_t    QuestOffer[88];     // PS2: QuestOffer
    uint32_t    QuestComplete[88];  // PS2: QuestComplete
    uint32_t    MissionComplete[32];// PS2: MissionComplete
    uint32_t    Expansion_RotZ;     // PS2: ScCounter
    uint16_t    NationMission;      // PS2: QuestStart
    uint8_t     Nation;             // PS2: Nation
    uint8_t     RdyFlag;            // PS2: RdyFlag
    uint32_t    Expansion_CoP;      // PS2: (New; did not exist.)
    uint32_t    Expansion_CoP2;     // PS2: (New; did not exist.)
    uint32_t    Expansion_Addons;   // PS2: (New; did not exist.)
    uint32_t    Expansion_SoA;      // PS2: (New; did not exist.)
    uint32_t    Expansion_RoV;      // PS2: (New; did not exist.)
    uint32_t    Expansion_TVR;      // PS2: (New; did not exist.)
    uint32_t    TalesBeginning;     // PS2: (New; did not exist.)
    uint32_t    RecBitFlag;         // PS2: RecBitFlag
    uint32_t    CallPtr;            // PS2: CallPtr
};
```

## Structure Fields (`MISSION_DATA`)

### `QuestOffer`

_The current activated quest bits._

**Note:** See the table below for which content is stored in this container.

### `QuestComplete`

_The completed quest bits._

**Note:** See the table below for which content is stored in this container.

### `MissionComplete`

_The completed mission bits._

**Note:** See the table below for which content is stored in this container.

### `Expansion_RotZ`

_Current mission information for the expansion: `Rise of the Zilart`_

The client only uses the lower `uint16_t` part of this value currently.

  - When the client has no active or completed RotZ missions, this value is set to `0xFFFF`.
  - When the client uses the new `Tales'Beginning` system, this value is set to `0xFFFF`.
    - _The `TalesBeginning` flags will have `0x0001` set._

### `NationMission`

_Current mission information for clients current home nation._

The client only uses the lower `uint16_t` part of this value currently.

  - When the client has no active mission, this value is set to `0xFFFF`.

### `Nation`

_The clients current home nation id._

  - `0` - San d'Oria
  - `1` - Bastok
  - `2` - Windurst

This value is used to determine which nation mission menu will display its mission information. Menus that are not of the clients current home nation will display the `Not allowed under current allegiance.` message instead.

### `RdyFlag`

_The structure ready flag._

This flag is set when all data has been received and the full `MISSION_DATA` structure is considered populated. The client does this by checking the `RecBitFlag` value for the fully populated value of: `0x83FFFFFF`

### `Expansion_CoP`

_Mission information for the expansion: `Chains of Promathia`_

This value holds the mission information related to `Chains of Promathia`. The client uses this value to show both the current mission as well as the completed missions for this expansion.

  - When the client has no active or completed CoP missions, this value is set to `0`.
  - When the client uses the new `Tales'Beginning` system, this value is set to `0`.
    - _The `TalesBeginning` flags will have `0x0008` set._

### `Expansion_CoP2`

_Mission information for the expansion: `Chains of Promathia`_

This value holds extended mission information related to `Chains of Promathia`. The client uses this value when displaying the CoP mission information for certain specific mission branches.

The client has specific usages for this value when handling certain CoP mission values:

| Expansion_CoP | Mission | Purpose |
| --- | --- | --- |
| `325` | `The Road Forks`  | _Used to display sub-mission completions._ |
| `530` | `Three Paths`     | _Used to display sub-mission completions._ |

### `Expansion_Addons`

_Mission information for the addon scenario expansions._

This value holds the three addon scenario expansions mission information. The client uses these values to show both the current mission and the completed missions.

| Expansion | Value Usage | Value Range |
| --- | --- | --- |
| `A Crystalline Prophecy`  | `val & 0x0F`  | `0x00` to `0x0B` |
| `A Moogle Kupo d'Etat`    | `val << 0x04` | `0x00` to `0x0E` |
| `A Shantotto Ascension`   | `val << 0x08` | `0x00` to `0x0E` |

  - If an addon scenario has no mission information, then its value will be `0`. _(Adjusted accordingly.)_
  - If an addon scenario uses the new `Tales'Beginning` system, then its value will be `0`. _(Adjusted accordingly.)_
    - _The `TalesBeginning` flags will have its corresponding flag set for the given addon scenario._
      - _`A Crystalline Prophecy` will have flag `0x02` set._
      - _`A Moogle Kupo d'Etat` does not use this system._
      - _`A Shantotto Ascension` will have flag `0x04` set._

### `Expansion_SoA`

_Mission information for the expansion: `Seekers of Adoulin`_

This value holds the mission information related to `Seekers of Adoulin`. The client uses this value to show both the current mission as well as the completed missions for this expansion.

  - When the client has no active or completed SoA missions, this value is set to `0`.
  - When the client uses the new `Tales'Beginning` system, this value is set to `0`.
    - _The `TalesBeginning` flags will have `0x0020` set._
  - When the client has completed the original SoA mission line prior to the Epilogue, this value will be set to `999`.

### `Expansion_RoV`

_Mission information for the expansion: `Rhapsodies of Vana'diel`_

This value holds the mission information related to `Rhapsodies of Vana'diel`. The client uses this value to show both the current mission as well as the completed missions for this expansion.

  - When the client has no active or completed RoV missions, this value is set to `0`.
  - When the client uses the new `Tales'Beginning` system, this value is set to `0`.
    - _The `TalesBeginning` flags will have `0x0040` set._
  - When the client has completed the original RoV mission line prior to the Epilogue, this value will be set to `999`.

### `Expansion_TVR`

_Mission information for the expansion: `The Voracious Resurgence`_

This value holds the mission information related to `The Voracious Resurgence`. The client uses this value to show both the current mission as well as the completed missions for this expansion.

  - When the client has no active or completed TVR missions, this value is set to `0`.
  - When the client has completed the original TVR mission line prior to the Epilogue, this value will be set to `999`.

The client uses this value by doing the following masking: `tvr = PTR_MissionData.Expansion_TVR & 0xEFFF`

When the client has completed TVR missions but does not have one currently active, then this value will have an upper-bounds flag set. The client checks for this flag via the following: `(PTR_MissionData.Expansion_TVR & 0x80000000) != 0`

### `TalesBeginning`

_Mission information for the clients `Tales'Beginning` status for the available expansions._

This value holds the bitflags used to determine if a supported expansion has been suspended using the new `Tales Beginning` system. When a client first is prompted to begin one of the supported expansion storylines, they will now be given an option to suspend starting for later. When this happens, their mission log will be set to a newly added temporary mission that explains how to continue and start the missions later. This value supports the following flags:

| Flag | Expansion | Mission Log |
| --- | --- | --- |
| `0x01` | `Rise of the Zilart`         | `To Commence RotZ` |
| `0x02` | `A Crystalline Prophecy`     | `To commence ACP` |
| `0x04` | `A Shantotto Ascension`      | `To Begin ASA...` |
| `0x08` | `Chains of Promathia`        | `To Begin CoP...` |
| `0x10` | `(unused)`                   | `(unused)` |
| `0x20` | `Seekers of Adoulin`         | `To begin SoA...` |
| `0x40` | `Rhapsodies of Vana'diel`    | `To Start RoV...` |

### `RecBitFlag`

_The received data bit flags._

This value is used to determine what data has been populated so far into the clients `MISSION_DATA` structure. This value is also checked before making use of any of the data in the clients additional mission and quest handler functions. If the value is not `0x83FFFFFF`, then those functions will exit early and use default values for the requested data from the structure instead.

The client will populate this value using the packets `Port` value to determine how to properly add in the current packets bits.

  - When the `Port` value is `0xFFFE`, the client will perform: `PTR_MissionData.RecBitFlag |= 0x80000000;`
  - When the `Port` value is `0xFFFF`, the client will perform: `PTR_MissionData.RecBitFlag |= 0x80000000;`

For the non-static checked ranged based `Port` values, the client will handle this differently.

| Port | RecBitFlag Usage |
| --- | --- |
| `0x0030` | `val = (1 << (((pkt->Port - 0x20) >> 3) + 22));` |
| `0x0038` | `val = (1 << (((pkt->Port - 0x20) >> 3) + 22));` |
| `0x0050` | `val = (1 << ((pkt->Port - 0x50) >> 3));` |
| `0x0058` | `val = (1 << ((pkt->Port - 0x50) >> 3));` |
| `0x0060` | `val = (1 << ((pkt->Port - 0x50) >> 3));` |
| `0x0068` | `val = (1 << ((pkt->Port - 0x50) >> 3));` |
| `0x0070` | `val = (1 << ((pkt->Port - 0x50) >> 3));` |
| `0x0078` | `val = (1 << ((pkt->Port - 0x50) >> 3));` |
| `0x0080` | `val = (1 << ((pkt->Port - 0x50) >> 3));` |
| `0x0088` | `val = (1 << ((pkt->Port - 0x50) >> 3));` |
| `0x0090` | `val = (1 << (((pkt->Port - 0x90) >> 3) + 11));` |
| `0x0098` | `val = (1 << (((pkt->Port - 0x90) >> 3) + 11));` |
| `0x00A0` | `val = (1 << (((pkt->Port - 0x90) >> 3) + 11));` |
| `0x00A8` | `val = (1 << (((pkt->Port - 0x90) >> 3) + 11));` |
| `0x00B0` | `val = (1 << (((pkt->Port - 0x90) >> 3) + 11));` |
| `0x00B8` | `val = (1 << (((pkt->Port - 0x90) >> 3) + 11));` |
| `0x00C0` | `val = (1 << (((pkt->Port - 0x90) >> 3) + 11));` |
| `0x00C8` | `val = (1 << (((pkt->Port - 0x90) >> 3) + 11));` |
| `0x00D0` | `val = (1 << (((pkt->Port - 0xD0) >> 3) + 22));` |
| `0x00D8` | `val = (1 << (((pkt->Port - 0xD0) >> 3) + 22));` |
| `0x00E0` | `val = (1 << ((pkt->Port - 0xA0) >> 3));` |
| `0x00E8` | `val = (1 << (((pkt->Port - 0xA8) >> 3) + 11));` |
| `0x00F0` | `val = (1 << ((pkt->Port - 0xA8) >> 3));` |
| `0x00F8` | `val = (1 << (((pkt->Port - 0xB0) >> 3) + 11));` |
| `0x0100` | `val = (1 << ((pkt->Port - 0xB0) >> 3));` |
| `0x0108` | `val = (1 << (((pkt->Port - 0xB8) >> 3) + 11));` |

_Afterward, the `val` produced here will be handled via: `PTR_MissionData.RecBitFlag |= val;`_

### `CallPtr`

_The callback function pointer._

This value is no longer used. The function used to set this value exists but is not referenced in the client.

## Additional Information: `Port` Container Usage

When the `Port` value is not one of the static values _(`0xFFFE` or `0xFFFF`)_ then the client will handle the information based on a range of values the `Port` falls within. The below table shows the current used `Port` values from retail and which table they relate to when being used:

| Port | Container | Offset | Purpose |
| --- | --- | --- | --- |
| `0x0030` | `MissionComplete`  | `(pkt->Port - 0x20) * 0x04` | _Completed Missions: Campaign_ |
| `0x0038` | `MissionComplete`  | `(pkt->Port - 0x20) * 0x04` | _Completed Missions: Campaign_ |
| `0x0050` | `QuestOffer`       | `(pkt->Port - 0x50) * 0x04` | _Current Quests: San d'Oria_ |
| `0x0058` | `QuestOffer`       | `(pkt->Port - 0x50) * 0x04` | _Current Quests: Bastok_ |
| `0x0060` | `QuestOffer`       | `(pkt->Port - 0x50) * 0x04` | _Current Quests: Windurst_ |
| `0x0068` | `QuestOffer`       | `(pkt->Port - 0x50) * 0x04` | _Current Quests: Jeuno_ |
| `0x0070` | `QuestOffer`       | `(pkt->Port - 0x50) * 0x04` | _Current Quests: Other Areas_ |
| `0x0078` | `QuestOffer`       | `(pkt->Port - 0x50) * 0x04` | _Current Quests: Outlands_ |
| `0x0080` | `QuestOffer`       | `(pkt->Port - 0x50) * 0x04` | _Current Quests: Aht Urhgan_ |
| `0x0088` | `QuestOffer`       | `(pkt->Port - 0x50) * 0x04` | _Current Quests: Crystal War_ |
| `0x0090` | `QuestComplete`    | `(pkt->Port - 0x90) * 0x04` | _Completed Quests: San d'Oria_ |
| `0x0098` | `QuestComplete`    | `(pkt->Port - 0x90) * 0x04` | _Completed Quests: Bastok_ |
| `0x00A0` | `QuestComplete`    | `(pkt->Port - 0x90) * 0x04` | _Completed Quests: Windurst_ |
| `0x00A8` | `QuestComplete`    | `(pkt->Port - 0x90) * 0x04` | _Completed Quests: Jeuno_ |
| `0x00B0` | `QuestComplete`    | `(pkt->Port - 0x90) * 0x04` | _Completed Quests: Other Areas_ |
| `0x00B8` | `QuestComplete`    | `(pkt->Port - 0x90) * 0x04` | _Completed Quests: Outlands_ |
| `0x00C0` | `QuestComplete`    | `(pkt->Port - 0x90) * 0x04` | _Completed Quests: Aht Urhgan_ |
| `0x00C8` | `QuestComplete`    | `(pkt->Port - 0x90) * 0x04` | _Completed Quests: Crystal War_ |
| `0x00D0` | `MissionComplete`  | `(pkt->Port - 0xD0) * 0x04` | _Completed Missions: San d'Oria, Bastok, Windurst_ |
| `0x00D8` | `MissionComplete`  | `(pkt->Port - 0xD0) * 0x04` | _Completed Missions: Treasures, Wings of the Goddess_ |
| `0x00E0` | `QuestOffer`       | `(pkt->Port - 0xA0) * 0x04` | _Current Quests: Abyssea_ |
| `0x00E8` | `QuestComplete`    | `(pkt->Port - 0xA8) * 0x04` | _Completed Quests: Abyssea_ |
| `0x00F0` | `QuestOffer`       | `(pkt->Port - 0xA8) * 0x04` | _Current Quests: Adoulin_ |
| `0x00F8` | `QuestComplete`    | `(pkt->Port - 0xB0) * 0x04` | _Completed Quests: Adoulin_ |
| `0x0100` | `QuestOffer`       | `(pkt->Port - 0xB0) * 0x04` | _Current Quests: Coalition_ |
| `0x0108` | `QuestComplete`    | `(pkt->Port - 0xB8) * 0x04` | _Completed Quests: Coalition_ |

## Additional Information: `Data` Usage

The manner in which this packets `data` is handled will depend on the `Port` value.

### Port: `0xFFFE` - `The Voracious Resurgence Information`

When this port value is being handled, the client only uses the value from `data[0]`.

```cpp
PTR_MissionData.Expansion_TVR = pkt->data[0];
```

### Port: `0xFFFF` - `Main Mission Information`

When this port value is being handled, the client makes use of all of the `data` fields.

```cpp
PTR_MissionData.Nation          = pkt->data[0];
PTR_MissionData.NationMission   = pkt->data[1];
PTR_MissionData.Expansion_RotZ  = pkt->data[2];
PTR_MissionData.Expansion_CoP   = pkt->data[3];
PTR_MissionData.Expansion_CoP2  = pkt->data[4]; // Note: This is not a direct copy for this value, much more happens to it!
PTR_MissionData.Expansion_Addons= LOWORD(pkt->data[5]);
PTR_MissionData.TalesBeginning  = HIWORD(pkt->data[5]);
PTR_MissionData.Expansion_SoA   = pkt->data[6];
PTR_MissionData.Expansion_RoV   = pkt->data[7];
```

### Port: `Other` - `All Other Port Values`

When the client is handling any other port value that was listed in the tables above, then the client makes use of the entire `data` block as a single value for that given purpose. It'll use the offset based information and table listed above and then copy the `data` into the given table at that offset as-is. All 8 `uint32_t` values will be copied.
