# `GP_SERV_COMMAND_ITEM_TRADE_MYLIST`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_ITEM_TRADE_MYLIST` |
| **Client Handler**        | `RecvItemTradeMyList` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0025` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the server to inform the player of a trade item update. _(This update is related to the local players items.)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_ITEM_TRADE_MYLIST
struct GP_SERV_ITEM_TRADE_MYLIST
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    ItemNum;    // PS2: ItemNum
    uint16_t    ItemNo;     // PS2: ItemNo
    uint8_t     TradeIndex; // PS2: TradeIndex
    uint8_t     ItemIndex;  // PS2: ItemIndex
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_ITEM_TRADE_MYLIST`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ItemNum`

_The item quantity._

### `ItemNo`

_The item id._

### `TradeIndex`

_The trade container index._

### `ItemIndex`

_The item index._
