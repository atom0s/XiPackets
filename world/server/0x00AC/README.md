# `GP_SERV_COMMAND_COMMAND_DATA`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_COMMAND_DATA` |
| **Client Handler**        | `RecvCommandData` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00AC` |
| **Size**                  | `0x00E4` |

## Description

This packet is sent by the server to populate the clients command information. _(Weapon Skills, Abilities, Pet Abilities, Traits)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_COMMAND_DATA
struct GP_SERV_COMMAND_DATA
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     CommandDataTbl[224]; // PS2: CommandDataTbl
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_COMMAND_DATA`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `CommandDataTbl`

_The clients available command information._

This value is used to populate the clients local `PTR_pGlobalNowZone->FeatSys.CommandDataTbl` buffer, which is used to hold the clients unlocked and available weapon skills, job abilities, pet abilities and job traits. The `CommandDataTbl` information is broken into 4 blocks to hold these values, which is as follows:

  - `Weapon Skills` _(64 bytes)_
  - `Job Abilities` _(64 bytes)_
  - `Pet Abilities` _(64 bytes)_
  - `Job Traits` _(32 bytes)_

## Additional Information

The client directly copies the information in `CommandDataTbl` into its own local buffer `PTR_pGlobalNowZone->FeatSys.CommandDataTbl`. The client uses this block of information to determine if the player has access to abilities and traits related to their job _(and sub-job)_. This block of data is treated as bits for each type of entry within the block, following the order of the above list. It aligns directly with the ability names DAT file:

  - JP: `ROM\181\68.DAT` (File Id: `55581`)
  - NA: `ROM\181\72.DAT` (File Id: `55701`)

This file holds all of the above information _(and additionally, BLU spells)_ in the same size blocks, but laid out as bits aligned to each string index. For example, the first 8 bits within the `Weapon Skills` part of this block aligns to the following in the DAT file:

| Index | Weapon Skill |
| --- | --- |
| `0` | _(none)_ |
| `1` | _Combo_ |
| `2` | _Shoulder Tackle_ |
| `3` | _One Inch Punch_ |
| `4` | _Backhand Blow_ |
| `5` | _Raging Fists_ |
| `6` | _Spinning Attack_ |
| `7` | _Howling Fist_ |
| `...` | `...` |

As mentioned above, the sections in this file aligns directly (in bits) to the `CommandDataTbl`. The sections of the file are as follows:

| Section | Start Index | End Index |
| --- | --- | --- |
| `Weapon Skills`   | `0`       | `511` |
| `Job Abilities`   | `512`     | `1023` |
| `Pet Abilities`   | `1024`    | `1535` |
| `Job Traits`      | `1536`    | `1791` |

_After the `Job Traits` entries, the Blue Magic spells begins at index `1792`._
