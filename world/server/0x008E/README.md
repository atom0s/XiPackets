# `Unknown - Packet: 0x008E`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x008E` |
| **Size**                  | `0x0068` |

## Description

This packet is sent by the server to populate the clients Alter Ego points information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    points;         // PS2: (New; did not exist.)
    uint8_t     padding06[2];   // PS2: (New; did not exist.)
    uint8_t     count[32];      // PS2: (New; did not exist.)
    uint16_t    next[32];       // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `points`

_The number of Alter Ego points the player has currently._

### `padding06`

_Padding; unused._

### `count`

_The array that holds the players Alter Ego points upgrades for each sub-category._

### `next`

_The array that holds the amount of needed Alter Ego points to upgrade each sub-category._

## Additional Information

The `count` and `next` array values are indexed using the same `Kind` values as the [C2S 0x00C1](/world/client/0x00C1/README.md) packet; which is used when requesting to upgrade an Alter Ego point sub-category. _(Please see that packets documentation for a valid list of indexes.)_
