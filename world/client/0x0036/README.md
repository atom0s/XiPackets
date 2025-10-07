# `GP_CLI_COMMAND_ITEM_TRANSFER`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_ITEM_TRANSFER` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0036` |
| **Size**                  | `0x0040` |

## Description

This packet is sent by the client when completing a trade with an NPC.

```cpp
// PS2: GP_CLI_ITEM_TRANSFER
struct GP_CLI_ITEM_TRANSFER
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    UniqueNo;                   // PS2: UniqueNo
    uint32_t    ItemNumTbl[10];             // PS2: ItemNumTbl
    uint8_t     PropertyItemIndexTbl[10];   // PS2: PropertyItemIndexTbl
    uint16_t    ActIndex;                   // PS2: ActIndex
    uint8_t     ItemNum;                    // PS2: ItemNum
    uint8_t     padding3D[3];               // PS2: dammy2
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_ITEM_TRANSFER`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The server id of the NPC being traded with._

### `ItemNumTbl`

_An array holding the count of items being traded for each slot._

### `PropertyItemIndexTbl`

_An array holding the item index of items being traded for each slot._

When the client includes gil in their trade, it will be put into index `0` of this array. The index value used is also `0` for gil. The `ItemNum[0]` value is set to the amount of gil being traded as well.

### `ActIndex`

_The target index of the NPC being traded with._

### `ItemNum`

_The number of slots populated in the trade._

### `padding3D`

_Padding; unused._
