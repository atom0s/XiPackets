# `GP_CLI_COMMAND_BAZAAR_ITEMSET`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_BAZAAR_ITEMSET` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x010A` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the client when setting an items sale price within the players personal bazaar.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_BAZAAR_ITEMSET
struct GP_CLI_BAZAAR_ITEMSET
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     ItemIndex;      // PS2: ItemIndex
    uint8_t     padding05[3];   // PS2: Dammy
    uint32_t    Price;          // PS2: Price
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_BAZAAR_ITEMSET`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ItemIndex`

_The index of the item in the clients inventory._

### `padding05`

_Padding; unused._

### `Price`

_The item sale price._

This value is set to the price the client wishes to sell the item for. If the client wishes to no longer sell the item, this value will be `0`.
