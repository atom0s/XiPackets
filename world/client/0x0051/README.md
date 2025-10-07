# `Packet: 0x0051`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0051` |
| **Size**                  | `0x0048` |

## Description

This packet is sent by the client when equipping an equipset.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct equipsetitem_t
{
    uint8_t     ItemIndex;
    uint8_t     EquipKind;
    uint8_t     Category;
    uint8_t     padding03;
};

// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t        id: 9;
    uint16_t        size: 7;
    uint16_t        sync;

    uint8_t         Count;          // PS2: (New; did not exist.)
    uint8_t         padding05[3];   // PS2: (New; did not exist.)
    equipsetitem_t  Equipment[16];  // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Count`

_The number of slots populated in the `Equipment` array._

### `padding05`

_Padding; unused._

### `Equipment`

_The array of items the client is attempting to equip._

## Structure Fields (`equipsetitem_t`)

### `ItemIndex`

_The index of the item within the container._

### `EquipKind`

_The equipment slot enumeration id._

### `Category`

_The container holding the item being equipped._

### `padding03`

_Padding; unused._
