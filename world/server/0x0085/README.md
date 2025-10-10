# `GP_SERV_COMMAND_GUILD_SELLLIST`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_GUILD_SELLLIST` |
| **Client Handler**        | `gcRecvGuildSellList` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0085` |
| **Size**                  | `0x00F8` |

## Description

This packet is sent by the server to inform the client of a guild shops item list it is accepting for player sales.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: _GP_GUILD_ITEM
struct _GP_GUILD_ITEM
{
    uint16_t        ItemNo; // PS2: ItemNo
    uint8_t         Count;  // PS2: Count
    uint8_t         Max;    // PS2: Max
    int32_t         Price;  // PS2: Price
};

// PS2: _GP_GUILD_LIST
struct _GP_GUILD_LIST
{
    uint16_t        id: 9;
    uint16_t        size: 7;
    uint16_t        sync;

    _GP_GUILD_ITEM  List[30];       // PS2: List
    uint8_t         Count;          // PS2: Count
    uint8_t         Stat;           // PS2: Stat
    uint8_t         paddingF6[2];   // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`_GP_GUILD_LIST`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `List`

_The list of items accepted for sales._

### `Count`

_The number of items within `List` that are populated._

### `Stat`

_The status flags._

This value is used to adjust different parts of the clients internal guild information structure.

  - `0x40` - Initializes the guild list for usage.
    - _Sets the guilds sale list `Max` to `0`._
    - _Sets the guilds sale list `Time` to the current timestamp._
    - _Sets the guilds sale list `Stat` to `3`._
  - `0x80` - Resets the guild `Stat` value.
    - _Sets the guilds sale list `Stat` to `1`._

### `paddingF6`

_Padding; unused._

## Structure Fields (`_GP_GUILD_ITEM`)

### `ItemNo`

_The item id._

### `Count`

_The count of items available._

### `Max`

_The maximum number of items that can be available._

### `Price`

_The price of the item._
