# `Unknown - Packet: 0x0040`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0040` |
| **Size**                  | `(unknown)` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

This packet has not been observed and is assumed to be deprecated. _(Please see additional information below.)_

## Packet Layout

The current layout of this packet is unknown. However, the client does not expect any data from the server for this packet to be handled properly.

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

## Additional Information

This packet has not been observed through normal packet logging means and is being considered deprecated. The handler for this packet in the client is setup to close the clients currently opened shop window. Due to this, it is assumed that the name for this packet would be `RecvShopClose` to follow the similar shop packet namings.

The client will still handle this packet if its sent, which will close any currently opened shop window.
