# `GP_CLI_COMMAND_SHOP_REQ`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_SHOP_REQ` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0082` |
| **Size**                  | `0x0008` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

> [!NOTE]
> This is a special GM-related packet!

This packet was used with the GM command `//slist`. The purpose of this packet is unknown.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_SHOP_REQ
struct GP_CLI_SHOP_REQ
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    ShopNo;             // PS2: ShopNo
    uint8_t     ShopItemOffsetIndex;// PS2: ShopItemOffsetIndex
    uint8_t     padding00;          // PS2: padding00
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_SHOP_REQ`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ShopNo`

_Unknown._

The client does not use or set this value.

### `ShopItemOffsetIndex`

_The shop item index._

### `padding00`

_Padding; unused._
