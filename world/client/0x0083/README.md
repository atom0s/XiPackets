# `GP_CLI_COMMAND_SHOP_BUY`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_SHOP_BUY` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0083` |
| **Size**                  | `0x0010` |

## Description

This packet is sent by the client when purchasing items from a shop.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_SHOP_BUY
struct GP_CLI_SHOP_BUY
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    ItemNum;            // PS2: ItemNum
    uint16_t    ShopNo;             // PS2: ShopNo
    uint16_t    ShopItemIndex;      // PS2: ShopItemIndex
    uint8_t     PropertyItemIndex;  // PS2: PropertyItemIndex
    uint8_t     padding0D[3];       // PS2: dammy
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_SHOP_BUY`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ItemNum`

_The quantity of the item to be purchased._

### `ShopNo`

_Unused._

The client does not set or use this value.

### `ShopItemIndex`

_The index within the shops item list of the item being purchased._

### `PropertyItemIndex`

_The index within the clients inventory to store the item._

This value is always set to `0`. The server is responsible for determining where to place the item.

### `padding0D`

_Padding; unused._
