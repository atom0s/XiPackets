# `Packet: 0x002B`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x002B` |
| **Size**                  | `0x0048` |

## Description

This packet is sent by the client when using the `/translate` command.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     FromIndex;
    uint8_t     ToIndex;
    uint16_t    padding00;
    uint8_t     Name[64];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `FromIndex`

_The language index being translated from._

  - `0` - _Japanese_
  - `1` - _English_
  - `2` - _German (Deprecated)_
  - `3` - _French (Deprecated)_

### `ToIndex`

_The language index being translated to._

  - `0` - _Japanese_
  - `1` - _English_
  - `2` - _German (Deprecated)_
  - `3` - _French (Deprecated)_

### `padding00`

_Padding; unused._

This value is always set to 0.

### `Name`

_The item name to be translated._

## Additional Information

The `/translate` command is used to request translations of an items name from one language to another which will also be added to the clients current auto-translate dictionary. The format of this command dictates the direction in which the translation will happen in regards to the language of the item being sent and what the client expects back as the response.

For example, the following would request translating `Fire Crystal` from English to Japanese:

  - `/translate "Fire Crystal" ej`
