# `Unknown - Packet: 0x0077`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0077` |
| **Size**                  | `0x0088` |

## Description

This packet is sent by the server to inform the client of a list of entity server ids that it is allowed to see, even if the entity is marked as hidden.

_Note: This packet may have additional usages in the future. It is a general purpose style packet._

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Flags;
    uint8_t     padding00[3];
    uint8_t     Data[128];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `Flags`

_The packet flags._

This value is used to determine how the data within this packet should be handled.

At this time, the client only handles a single flag value; `1`.

### `padding00`

_Padding; unused._

### `Data`

_The packet data._

This value is treated differently based on the `Flags` value that is present.

## Additional Information: `Flags`

Currently, the client only handles a single flag value within this packets handler; `0x01`. When this flag is present, then the `Data` array is instead treated as an array of `uint32_t` values _(32 total)_. These values are used as a list of entities that the client is allowed to see that would otherwise be hidden. This is used for special content and events where a select group of hidden entities within a zone need to be made visible to the client to complete the content.

For example, the quest `Full Speed Ahead!` will use this packet when the client zones into `Batallia Downs`, marking the various plates of raptor food as visible to the client. These entities are always within the zone, but are marked hidden from players unless specifically given permission to see them. If an entity within the list that is sent has been killed, despawned, used, etc. then the server will send updates to this list blanking the given entry by zeroing it, thus hiding the entity from the client again.
