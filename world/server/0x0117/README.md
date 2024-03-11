# `Unknown - Packet: 0x0117`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0117` |
| **Size**                  | `0x0088` |

## Description

This packet is sent by the server after the client has equipped an equpset. _(ie. `/equipset 1`)_ It is used to validate if any pieces of gear within the set failed to equip.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct equipsetitem_t
{
    uint8_t     ItemIndex;
    uint8_t     EquipKind;
    uint8_t     Category;
    uint8_t     padding00;
};

// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t        id: 9;
    uint16_t        size: 7;
    uint16_t        sync;
    uint8_t         Count;
    uint8_t         padding00[3];
    equipsetitem_t  ItemsChanged[16];
    equipsetitem_t  ItemsEquipped[16];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Count`

_The number of entries within the `ItemsChanged` array._

### `padding00`

_Padding; unused._

### `ItemsChanged`

_The array of items that were equipped (changed) using the equipset._

### `ItemsEquipped`

_The array of the clients current equipped items._

## Structure Fields (`equipsetitem_t`)

### `ItemIndex`

_The index of the item within the container._

### `EquipKind`

_The equipment slot enumeration id._

### `Category`

_The container holding the item being equipped._

### `padding00`

_Padding; unused._

## Additional Information

This packet is sent by the server each time the client swaps gear using the built-in equipset system. The client uses this packets information to determine if any piece of equipment that is defined in the equipset has failed to be equipped. The client will loop over the `ItemsChanged` array and compare it to the values within the `ItemsEquipped` array. If any slot failed to change, it will be detected as a mismatch between these two arrays and the client will print an error message stating the item failed to equip.
