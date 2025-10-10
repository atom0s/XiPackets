# `GP_SERV_COMMAND_CLISTATUS2`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_CLISTATUS2` |
| **Client Handler**        | `RecvCliStatus2` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0062` |
| **Size**                  | `0x0100` |

## Description

This packet is sent by the server to update the clients skill base information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_CLISTATUS2
struct GP_SERV_CLISTATUS2
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    CommandRecast[31];  // PS2: CommandRecast
    uint16_t    skill_base[64];     // PS2: skill_base
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_CLISTATUS2`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `CommandRecast`

_The client ability recast timers._

The client no longer uses this block of data. This information is sent in different packets now.

### `skill_base`

_The clients various skill base values._

The client walks this block of data and copies each value individually into the clients player skill information table, starting at `PTR_status_data.CombatSkills`.

The first 48 entries are the skills combat skills, while the remaining 16 are the clients crafting skills.
