# `GP_SERV_COMMAND_BAZAAR_LIST`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_BAZAAR_LIST` |
| **Client Handler**        | `RecvList` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0105` |
| **Size**                  | `0x002C` |

## Description

This packet is sent by the server when the client is checking another players Bazaar. This packet is used to populate the checked players bazaar items.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_BAZAAR_LIST
struct GP_SERV_BAZAAR_LIST
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    Price;          // PS2: Price
    uint32_t    ItemNum;        // PS2: ItemNum
    uint16_t    TaxRate;        // PS2: TaxRate
    uint16_t    ItemNo;         // PS2: ItemNo
    uint8_t     ItemIndex;      // PS2: ItemIndex
    uint8_t     Attr[24];       // PS2: Attr
    uint8_t     padding00[3];   // PS2: (New; did not exist.))
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_BAZAAR_LIST`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Price`

_The price of the item being sold._

### `ItemNum`

_The number of items being sold._

This value holds the amount of items within a stack, if more than one item is being sold in the slot.

### `TaxRate`

_The tax rate of the current area._

This value is the percentage of tax that is being charged within the current area the player is located. The client will use this value to both populate the individual items tax rate and to set the zone-wide tax rate for the current area bazaar system.

_This value is divided by 100 to determine its real value. For example, a tax rate of 1% would mean this value will be 100._

### `ItemNo`

_The item id._

### `ItemIndex`

_The item index._

### `Attr`

_The item attributes._

This value holds the unique extended data of the item such as:

  - Augments
  - Charges
  - Cooldowns / Timers
  - Signature.
  - Trials information.
  - etc.

### `padding00`

_Padding; unused._
