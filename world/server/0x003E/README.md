# `GP_SERV_COMMAND_SHOP_OPEN`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_SHOP_OPEN` |
| **Client Handlers**       | `YkWndShopMain::RecvShopOpen`, `RecvShopOpen` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x003E` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the server to inform the client to prepare to open a shop window.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_SHOP_OPEN
struct GP_SERV_SHOP_OPEN
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    ShopListNum; // PS2: ShopListNum
    uint16_t    padding00;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_SHOP_OPEN`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ShopListNum`

_The number of items within the shop._

### `padding00`

_Padding; unused._
