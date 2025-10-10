# `GP_SERV_COMMAND_EQUIP_CLEAR`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_EQUIP_CLEAR` |
| **Client Handler**        | `RecvEquipClear` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x004F` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the server to inform the client to clear its current equipment information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_EQUIP_CLEAR
struct GP_SERV_EQUIP_CLEAR
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    padding04; // PS2: Dammy
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_EQUIP_CLEAR`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `padding04`

_Padding; unused._

## Additional Information

This packet causes the client to clear the current equipment information stored within the `zone->EquipSys.EquipTbl`. The client will then invoke a callback that will rebuild an internal sub-table of equipment information and repopulate this table. The sub-table is used for ability related functionality.
