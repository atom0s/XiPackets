# `Unknown - Packet: 0x011C`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x011C` |
| **Size**                  | `(unknown)` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

This packet is sent by the server in response to the client locking their style. _(This packet was intended to work similar to packet `0x0117` to inform the client when a piece of gear could not be style locked.)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Count;
    uint8_t     padding00[3];
    uint16_t    ItemNo[];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Count`

_The number of entries within the `ItemNo` array._

### `padding00`

_Padding; unused._

### `ItemNo`

_The array of item ids that failed to be style locked._

This value holds an array of every item id that failed to style lock upon the players request.

The client will loop through this array _(using `Count` as the total amount to check)_ and print an error message saying the item failed to style lock.

## Additional Information

This packet is believed to be deprecated as the lock style system makes use of other packets now. While the client can still handle this packet properly, it has not been observed on current retail.
