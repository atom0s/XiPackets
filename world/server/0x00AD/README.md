# `Unknown - Packet: 0x00AD`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00AD` |
| **Size**                  | `0x0084` |

## Description

This packet is sent by the server to populate the clients Moblin Maze Mongers information. _(Vouchers and Runes)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     Vouchers[8];    // PS2: (New; did not exist.)
    uint8_t     Runes[64];      // PS2: (New; did not exist.)
    uint8_t     unused00[56];   // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Vouchers`

_The clients unlocked MMM vouchers._

This value is used as a bit array that states which Maze Vouchers the client has unlocked. This array aligns to the item DAT file that contains the maze related items, starting at item id: `28736`

### `Runes`

_The clients unlocked MMM runes._

This value is used as a bit array that states which Maze Runes the client has unlocked. This array aligns to the item DAT file that contains the maze related items, starting at item id: `28800`

### `unused00`

_Unused._

## Additional Information

When the client opens a Maze Tabula item, it will make use of this data to determine how to populate the sub-menus that show which Vouchers and Runes that the client has available for use.

When listing the available Vouchers, it will loop through the first 64 bits of this packets data _(`vouchers`)_ to test each bit if its set. If the tested bit is set, it will align to the item DAT file that contains the maze related items, starting at item id `28736`. When selecting a voucher, the client will then list the available Maze Rune that the client has unlocked. The client will loop through 512 bits starting at the `runes` information to test for the available runes. These items are also aligned to the DAT file starting at item id `28800`.

The following item DAT file holds the maze items:

  - JP: `ROM\217\20.DAT` (File Id: `55547`)
  - NA: `ROM\217\21.DAT` (File Id: `55667`)
