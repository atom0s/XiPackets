# `GP_SERV_COMMAND_ITEM_LIST`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_ITEM_LIST` |
| **Client Handler**        | `RecvItemList` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x001F` |
| **Size**                  | `0x0010` |

## Description

This packet is sent by the server to inform the client of an item within a container.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_ITEM_LIST
struct GP_SERV_ITEM_LIST
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    ItemNum;    // PS2: ItemNum
    uint16_t    ItemNo;     // PS2: ItemNo
    uint8_t     Category;   // PS2: Category
    uint8_t     ItemIndex;  // PS2: ItemIndex
    uint8_t     LockFlg;    // PS2: LockFlg
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_ITEM_LIST`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `ItemNum`

_The quantity of the item._

### `ItemNo`

_The item id._

### `Category`

_The container holding the item._

### `ItemIndex`

_The index inside of the container this item is located._

### `LockFlg`

_The item lock flag._
