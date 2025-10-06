# `GP_CLI_COMMAND_SHOP_SELL_SET`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_SHOP_SELL_SET` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0085` |
| **Size**                  | `0x0006` |

## Description

This packet is sent by the client when confirming the sale of an item to a shop.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_SHOP_SELL_SET
struct GP_CLI_SHOP_SELL_SET
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    SellFlag; // PS2: SellFlag
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_SHOP_SELL_SET`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `SellFlag`

_Flag set to confirm the sale of an item to the shop._

This value is always set to `1`.

## Additional Information

When the client is selling items to a shop, it will first send an `0x0084` packet to obtain the sale price that the item is worth. It will then send this packet to confirm the sale of the previously selected item. The client will always send an `0x0084` packet first when selling an item to ensure the proper item is set as the selling item. Even if the client has already price checked an item in the same menu, it will send both packets every time.
