# `Unknown - Packet: 0x0048`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0048` |
| **Size**                  | `0x0080` |

## Description

This packet is sent by the server when the client is interacting with a linkshell concierge NPC.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     data[124];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `data`

_The response data for the previous client request._

This field will hold different data based on the type of request that was made by the client. _(TODO: Additional reversing will be done later.)_
