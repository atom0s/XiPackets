# `GP_SERV_COMMAND_ITEM_NUM`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_ITEM_NUM` |
| **Client Handler**        | `RecvItemNum` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x001E` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the server to inform the client that an item change has happened within a container.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_ITEM_NUM
struct GP_SERV_ITEM_NUM
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    ItemNum;    // PS2: ItemNum
    uint8_t     Category;   // PS2: Category
    uint8_t     ItemIndex;  // PS2: ItemIndex
    uint8_t     LockFlg;    // PS2: LockFlg
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_ITEM_NUM`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `ItemNum`

_The updated quantity of items to be set._

### `Category`

_The container holding the item being updated._

### `ItemIndex`

_The index inside of the container of the item being updated._

### `LockFlg`

_The item lock flag._
