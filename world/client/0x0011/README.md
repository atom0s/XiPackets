# `Packet: 0x0011`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0011` |
| **Size**                  | `0x0006` |

## Description

This packet is sent by the client when it has successfully entered the requested zone, confirming it has entered the new area.

## Packet Layout

The layout of this packet is the following:

```cpp
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     unknown00;
    uint8_t     unknown01;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `unknown00`

_Unknown._

This value is always set to `2`.

### `unknown01`

_Unknown._

This value is always set to `0`.
