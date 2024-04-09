# `GP_CLI_COMMAND_ITEM_TRADE_LIST`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_ITEM_TRADE_LIST` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0034` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the client when setting an item inside of the trade window.

```cpp
// PS2: GP_CLI_ITEM_TRADE_LIST
struct GP_CLI_ITEM_TRADE_LIST
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    ItemNum;    // PS2: ItemNum
    uint16_t    ItemNo;     // PS2: ItemNo
    uint8_t     ItemIndex;  // PS2: ItemIndex
    uint8_t     TradeIndex; // PS2: TradeIndex
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_ITEM_TRADE_LIST`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ItemNum`

_The quantity of the item to be traded._

### `ItemNo`

_The id of the item being traded._

### `ItemIndex`

_The index within the players inventory the item is located._

### `TradeIndex`

_The trade window slot index._
