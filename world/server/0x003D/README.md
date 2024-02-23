# `GP_SERV_COMMAND_SHOP_SELL`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_SHOP_SELL` |
| **Client Handler**        | `RecvShopSell` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x003D` |
| **Size**                  | `0x0010` |

## Description

This packet has two handlers and usages:

  - This packet is sent by the server to inform the client of the value of an item the client wishes to sell. _(Item appraisal.)_
  - This packet is sent by the server to inform the client of a completed sale.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_SHOP_SELL
struct GP_SERV_SHOP_SELL
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    Price;              // PS2: Price
    uint8_t     PropertyItemIndex;  // PS2: PropertyItemIndex
    uint8_t     Type;               // PS2: (New; did not exist.)
    uint16_t    padding00;          // PS2: (New; did not exist.)
    uint32_t    Count;              // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_SHOP_SELL`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Price`

_The price of the item._

### `PropertyItemIndex`

_The index of the item._

### `Type`

_The type of packet being sent._

  - `0` - _The packet is for an item appraisal._
  - `1` - _The packet is for an item sale._

### `padding00`

_Padding; unused._

### `Count`

_The number of items being sold._
