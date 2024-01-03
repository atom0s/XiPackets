# `GP_SERV_COMMAND_ITEM_ATTR`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_ITEM_ATTR` |
| **Client Handler**        | `RecvItemAttr` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0020` |
| **Size**                  | `0x002C` |

## Description

This packet is sent by the server to populate an items full information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_ITEM_ATTR
struct GP_SERV_ITEM_ATTR
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    ItemNum;    // PS2: ItemNum
    uint32_t    Price;      // PS2: Price
    uint16_t    ItemNo;     // PS2: ItemNo
    uint8_t     Category;   // PS2: Category
    uint8_t     ItemIndex;  // PS2: ItemIndex
    uint8_t     LockFlg;    // PS2: LockFlg
    uint8_t     Attr[];     // PS2: Attr
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_ITEM_ATTR`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `ItemNum`

_The quantity of the item._

### `Price`

_The bazaar price, if set, of the item._

### `ItemNo`

_The item id._

### `Category`

_The container holding the item._

### `ItemIndex`

_The index inside of the container this item is located._

### `LockFlg`

_The item lock flag._

### `Attr`

_The item attributes._

This value holds the unique extended data of the item such as:

  - Augments
  - Charges
  - Cooldowns / Timers
  - Trials information.
  - etc.
