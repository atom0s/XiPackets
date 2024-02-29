# `GP_SERV_COMMAND_ITEM_TRADE_LIST`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_ITEM_TRADE_LIST` |
| **Client Handlers**       | `CTkTrade::TkRecvItemTradeList`, `RecvItemTradeList` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0023` |
| **Size**                  | `0x0028` |

## Description

This packet is sent by the server to inform the player of a trade item update. _(This update is related to the other parties items.)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_ITEM_TRADE_LIST
struct GP_SERV_ITEM_TRADE_LIST
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    ItemNum;            // PS2: ItemNum
    uint16_t    TradeCounter;       // PS2: TradeCounter
    uint16_t    ItemNo;             // PS2: ItemNo
    uint8_t     ItemFreeSpaceNum;   // PS2: ItemFreeSpaceNum
    uint8_t     TradeIndex;         // PS2: TradeIndex
    uint8_t     Attr[24];           // PS2: Attr
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_ITEM_TRADE_LIST`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ItemNum`

_The traded item quantity._

### `TradeCounter`

_The trade counter._

### `ItemNo`

_The traded item id._

### `ItemFreeSpaceNum`

_Unknown._

This value is not used outside of this packet handler. It's set into the main trade container as follows but it is not used elsewhere in the client:

```cpp
zone->ItemSys.Trade.ItemFreeNum = pkt->ItemFreeSpaceNum;
```

### `TradeIndex`

_The trade container index._

### `Attr`

_The traded item attributes._

This value holds the unique extended data of the item such as:

  - Augments
  - Charges
  - Cooldowns / Timers
  - Trials information.
  - etc.
