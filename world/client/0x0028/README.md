# `GP_CLI_COMMAND_ITEM_DUMP`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_ITEM_DUMP` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0028` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the client when it drops an item.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_ITEM_DUMP
struct GP_CLI_ITEM_DUMP
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    ItemNum;    // PS2: ItemNum
    uint8_t     Category;   // PS2: Category
    uint8_t     ItemIndex;  // PS2: ItemIndex
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_ITEM_DUMP`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ItemNum`

_The quantity of the item to be dropped._

### `Category`

_The container holding the item._

### `ItemIndex`

_The index inside of the container this item is located._
