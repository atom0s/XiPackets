# `GP_CLI_COMMAND_SHOP_SELL_REQ`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_SHOP_SELL_REQ` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0084` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the client when requesting to sell an item to a shop. This packet is used to appraise the item to determine the price it is worth prior to actually selling it.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_SHOP_SELL_REQ
struct GP_CLI_SHOP_SELL_REQ
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    ItemNum;    // PS2: ItemNum
    uint16_t    ItemNo;     // PS2: ItemNo
    uint8_t     ItemIndex;  // PS2: ItemIndex
    uint8_t     padding0B;  // PS2: dammy
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_SHOP_SELL_REQ`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ItemNum`

_The quantity of the item to be sold._

### `ItemNo`

_The item id being sold._

### `ItemIndex`

_The index within the clients inventory that holds the item to be sold._

### `padding0B`

_Padding; unused._
