# `GP_SERV_COMMAND_ITEM_PRESENT`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_ITEM_PRESENT` |
| **Client Handler**        | `RecvItemPresent` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0024` |
| **Size**                  | `(Unknown)` |

## Description

Unknown. This packet is assumed to be deprecated.

> [!WARNING]
> This packets information is dumped from the PS2 beta but was also unused then. Thus the usage of this packet is unknown.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_ITEM_PRESENT
struct GP_SERV_ITEM_PRESENT
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    ItemNum;            // PS2: ItemNum
    uint16_t    ItemNo;             // PS2: ItemNo
    uint8_t     ReqID;              // PS2: ReqID
    uint8_t     ReqIndex;           // PS2: ReqIndex
    uint8_t     ItemData[];         // PS2: ItemData
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_ITEM_PRESENT`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ItemNum`

_The item quantity._

### `ItemNo`

_The item id._

### `ReqID`

_Unknown._

### `ReqIndex`

_Unknown._

### `ItemData`

_Unknown._
