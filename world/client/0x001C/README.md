# `Packet: 0x001C`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x001C` |
| **Size**                  | `0x000C` |

## Description

The purpose of this packet is currently unknown.

The client will send this packet each time the player uses a job ability while having a pet active.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    unknown04; // PS2: (New; did not exist.)
    uint16_t    padding06; // PS2: (New; did not exist.)
    uint32_t    unknown08; // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `unknown04`

_Unknown._

This value is always set to the local players target index.

### `padding06`

_Padding; unused._

### `unknown08`

_Unknown._

This value is always set to `1`.
