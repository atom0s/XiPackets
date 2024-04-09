# `GP_CLI_COMMAND_ITEM_MAKE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_ITEM_MAKE` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0038` |
| **Size**                  | `0x000C` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

> [!NOTE]
> This is a special GM-related packet!

This packet was used with the GM command `//itemmake`.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_ITEM_MAKE
struct GP_CLI_ITEM_MAKE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    ItemNum;    // PS2: ItemNum
    uint16_t    ItemNo;     // PS2: ItemNo
    uint16_t    padding00;  // PS2: padding00
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_ITEM_MAKE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ItemNum`

_The count of items._

### `ItemNo`

_The item id._

### `padding00`

_Padding; unused._
