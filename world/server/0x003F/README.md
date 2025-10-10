# `GP_SERV_COMMAND_SHOP_BUY`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_SHOP_BUY` |
| **Client Handlers**       | `RecvShopBuy`, `(UnnamedHandler)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x003F` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the server to inform the client of a completed purchase.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_SHOP_BUY
struct GP_SERV_SHOP_BUY
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    ShopItemIndex;  // PS2: ShopItemIndex
    uint8_t     BuyState;       // PS2: BuyState
    uint8_t     padding07;      // PS2: (New; did not exist.)
    uint32_t    Count;          // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_SHOP_BUY`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ShopItemIndex`

_The shop item index._

### `BuyState`

_Unknown._

The client does not use this value.

### `padding07`

_Padding; unused._

### `Count`

_The item count being purchased._

The client does not use this value.
