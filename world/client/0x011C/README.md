# `Packet: 0x011C`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x011C` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the client when using the party request command. _(`/partyrequestcmd`)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    UniqueNo;
    uint16_t    ActIndex;
    uint8_t     Kind;
    uint8_t     padding00;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The server id of the player whos party the client is requesting to join._

### `ActIndex`

_The target index of the player whos party the client is requesting to join._

### `Kind`

_The packet kind._

| Kind | Purpose |
| --- | --- |
| `0x00` | _Add - Request to join the target players party._ |
| `0x01` | _Remove - Remove request to join the target players party._ |

### `padding00`

_Padding; unused._
