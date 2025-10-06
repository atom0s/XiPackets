# `Packet: 0x011B`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x011B` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when changing their job mastery display. _(`/jobmasterdisp`)_

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

_The packet mode._

This value is used to indicate the mode of the command.

| Mode | Purpose |
| --- | --- |
| `0x00` | _Mode: `off`_ |
| `0x01` | _Mode: `on`_ |

### `padding00`

_Padding; unused._
