# `Packet: 0x0118`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0118` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when changing their Unity chat setting. _(Active or Inactive)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     Mode;
    uint8_t     padding00[3];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Mode`

_The chat mode._

| Mode | Purpose |
| --- | --- |
| `0x00` | _Inactive_ |
| `0x01` | _Active_ |

### `padding00`

_Padding; unused._
