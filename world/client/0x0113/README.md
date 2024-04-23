# `Packet: 0x0113`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0113` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the client when requesting to sit in a chair. _(`/sitchair`)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    Mode;
    uint32_t    ChairId;
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

This value is used to indicate the mode of the command. _(Toggle, On, Off)_

| Mode | Purpose |
| --- | --- |
| `0x00` | _Mode: (Toggle)_ |
| `0x01` | _Mode: `on`_ |
| `0x02` | _Mode: `off`_ |

### `ChairId`

_The id of the chair the client wishes to sit on._

This value is default to `0` if no argument was given.
