# `GP_SERV_COMMAND_BAZAAR_SALE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_BAZAAR_SALE` |
| **Client Handler**        | `RecvSale` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x010A` |
| **Size**                  | `0x001C` |

## Description

This packet is sent by the server when a bazaar sale was successful.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_BAZAAR_SALE
struct GP_SERV_BAZAAR_SALE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    ItemNum;        // PS2: ItemNum
    uint16_t    ItemNo;         // PS2: ItemNo
    uint8_t     sName[16];      // PS2: sName
    uint8_t     padding1A[2];   // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_BAZAAR_SALE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ItemNum`

_The number of items sold._

### `ItemNo`

_The item id._

### `sName`

_The name of the buyer._

### `padding1A`

_Padding; unused._
