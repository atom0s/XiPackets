# `GP_CLI_COMMAND_MAP_GROUP`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_MAP_GROUP` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00D2` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when requesting party member map marker information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: _GP_MAP_GROUP_REQ
struct _GP_MAP_GROUP_REQ
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    ZoneNo; // PS2: ZoneNo
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`_GP_MAP_GROUP_REQ`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ZoneNo`

_The local clients current zone id._
