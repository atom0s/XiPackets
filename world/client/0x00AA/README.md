# `GP_CLI_COMMAND_GUILD_BUY`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_GUILD_BUY` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00AA` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when buying items from a guild shop.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (Unnamed packet.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    ItemNo;
    uint8_t     PropertyItemIndex;
    uint8_t     ItemNum;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ItemNo`

_The item id being purchased._

### `PropertyItemIndex`

_The index within the clients inventory to place the item._

This value is always set to `0`.

### `ItemNum`

_The quantity of items being purchased._
