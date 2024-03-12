# `Unknown - Packet: 0x011D`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x011D` |
| **Size**                  | `0x0020` |

## Description

This packet is sent by the server to inform the client of a party invite request. _(Using the newer party request system.)_

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
    uint8_t     Flags;
    uint8_t     Status;
    uint8_t     sName[16];
    uint16_t    Race;
    uint8_t     padding00[2];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The requesting players server id._

### `ActIndex`

_The requesting players target index._

This value is only set if in the same zone.

### `Flags`

_Flags used to handle the message._

This value is used as flags to deal with special message properties. However, the usage it is for has been deprecated and thus no longer has actual purpose. The client will still call dead code because of this flag but its return is not used.

The client will check this value for `& 0x01`. If this flag is set, it will request the requesting players gender from the `Flags2` value. This was used for the French/German language messages, but those are no longer used.

### `Status`

_The request status._

This value is used to determine the kind of party request packet that was sent.

| Status | Purpose |
| --- | --- |
| `0` | _The player is requesting to join the clients party._ |
| `1` | _The player has stopped requesting to join the clients party._ |

### `sName`

_The requesting players name._

### `Race`

_The requesting players race._

This value was used to properly determine the requesting players gender. However this purpose is no longer used due to the French/German languages being deprecated.

### `padding00`

_Padding; unused._
