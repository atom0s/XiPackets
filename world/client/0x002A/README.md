# `GP_CLI_COMMAND_ITEM_ATTR`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_ITEM_ATTR` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x002A` |
| **Size**                  | `0x0006` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

## Packet Layout

The layout of this packet is the following:

```cpp
struct GP_CLI_ITEM_ATTR
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Category;   // PS2: Category
    uint8_t     ItemIndex;  // PS2: ItemIndex
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_ITEM_ATTR`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Category`

_The container holding the item._

### `ItemIndex`

_The index inside of the container this item is located._
