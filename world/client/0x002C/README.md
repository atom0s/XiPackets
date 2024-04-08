# `Packet: 0x002C`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x002C` |
| **Size**                  | `0x0048` |

## Description

This packet is sent by the client when using the `/itemsearch` command.

## Packet Layout

The layout of this packet is the following:

```cpp
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Language;
    uint8_t     padding00[3];
    uint8_t     Name[64];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Language`

_The clients language id._

  - `0` - _Japanese_
  - `1` - _English_
  - `2` - _French (Deprecated)_
  - `3` - _German (Deprecated)_

### `padding00`

_Padding; unused._

### `Name`

_The name of the item to be searched for._
