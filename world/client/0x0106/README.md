# `GP_CLI_COMMAND_BAZAAR_BUY`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_BAZAAR_BUY` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0106` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the client when requesting to purchase an item from a bazaar.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_BAZAAR_BUY
struct GP_CLI_BAZAAR_BUY
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     BazaarItemIndex;// PS2: BazaarItemIndex
    uint8_t     padding00[3];   // PS2: Dammy
    uint32_t    BuyNum;         // PS2: BuyNum
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_BAZAAR_BUY`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `BazaarItemIndex`

_The index within the bazaar where the item being requested for purchase is located._

### `padding00`

_Padding; unused._

### `BuyNum`

_The count of items the client is requesting to purchase._
