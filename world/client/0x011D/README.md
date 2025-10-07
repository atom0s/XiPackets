# `Packet: 0x011D`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x011D` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the client when using the jump command. _(`/jump`)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    UniqueNo;       // PS2: (New; did not exist.)
    uint16_t    ActIndex;       // PS2: (New; did not exist.)
    uint8_t     padding0A[2];   // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The local clients server id._

### `ActIndex`

_The local clients target index._

### `padding0A`

_Padding; unused._
