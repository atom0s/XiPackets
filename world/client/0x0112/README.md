# `Packet: 0x0112`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0112` |
| **Size**                  | `0x0006` |

## Description

This packet is sent by the client when requesting extra data about certain battlefield content.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     Kind;       // PS2: (New; did not exist.)
    uint8_t     padding05;  // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Kind`

_The packet kind._

This value is used to determine what kind of information the client is expecting to be returned from the server.

| Kind | Purpose |
| --- | --- |
| `0x00` | _The client is requesting both sidebar and map overlay information._ |
| `0x01` | _The client is requesting sidebar information_ |
| `0x02` | _The client is requesting map overlay information._ |

### `padding05`

_Padding; unused._

## Additional Information

This packet is sent by the client when requesting additional, or updated, battlefield information for certain kinds of content. The kind of content this is generally used with is for:

  - Monstrosity
  - Belligerency
  - Skirmish _(Various modes/zones.)_
  - etc.

When the request is valid and successful, the server will respond to the client with server packet `0x0072`. _(More than one response can be sent by the server depending on what kind of request was made by the client.)_
