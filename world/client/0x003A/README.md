# `GP_CLI_COMMAND_ITEM_STACK`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_ITEM_STACK` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x003A` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when sorting an inventory container.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_ITEM_STACK
struct GP_CLI_ITEM_STACK
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    Category; // PS2: Category
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_ITEM_STACK`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Category`

_The container holding the item._
