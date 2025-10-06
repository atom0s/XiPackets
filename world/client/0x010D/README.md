# `Packet: 0x010D`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x010D` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when requesting to remove an active Records of Eminence objective.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    ObjectiveId;
    uint16_t    padding00;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ObjectiveId`

_The Records of Eminence object id._

This value holds the normalized Records of Eminence objective id the client is requesting to remove. This value is normalized by subtracting `57344` from the id before writing it to the packet. _(The client also masks the id to 12 bits, leaving the remaining 4 bits unused.)_

### `padding00`

_Padding; unused._
