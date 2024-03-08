# `GP_SERV_COMMAND_BAZAAR_SELL`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_BAZAAR_SELL` |
| **Client Handlers**       | `RecvSell`, `RecvBazarSell` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0109` |
| **Size**                  | `0x0024` |

## Description

This packet is sent by the server when a bazaar purchase was successful.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_BAZAAR_SELL
struct GP_SERV_BAZAAR_SELL
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    UniqueNo;       // PS2: UniqueNo
    uint32_t    ItemNum;        // PS2: ItemNum
    uint16_t    ActIndex;       // PS2: ActIndex
    uint16_t    BazaarActIndex; // PS2: BazaarActIndex
    uint8_t     sName[16];      // PS2: sName
    uint8_t     ItemIndex;      // PS2: ItemIndex
    uint8_t     padding00[3];   // PS2: padding00
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_BAZAAR_SELL`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The buying players server id._

### `ItemNum`

_The number of items being purchased._

### `ActIndex`

_The buying players target index._

### `BazaarActIndex`

_The target index of the bazaar owner._

### `sName`

_The buying players name._

### `ItemIndex`

_The purchased item index._

### `padding00`

_Padding; unused._
