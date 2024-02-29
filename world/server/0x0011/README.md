# `GP_SERV_COMMAND_CHAR_DEL`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_CHAR_DEL` |
| **Client Handler**        | `RecvCharDel` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0011` |
| **Size**                  | `(unknown)` |

## Description

The purpose of this packet is unknown.

## Packet Layout

The current layout of this packet is unknown.

```cpp
// PS2: GP_SERV_CHAR_DEL
struct GP_SERV_CHAR_DEL
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_CHAR_DEL`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

## Additional Information

The purpose of this packet is unknown and has not been observed in any packet logs. The original PS2 beta information shows this packet may have been used as a means to despawn a player entity. However, both the PS2 and current retail PC handlers for this packet are empty. The packet information is not used at all.
