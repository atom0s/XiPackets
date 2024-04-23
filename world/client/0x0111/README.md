# `Packet: 0x0111`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0111` |
| **Size**                  | `0x0008` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

This packet was sent by the client when requesting to lock their style. This packet is no longer used as the style lock system has undergone several changes since this packet was first added. Other packets are now used to handle this feature.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    Status;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Status`

_The style lock status._

This value was used to determine if the client wishes to lock or unlock their style.

  - `0` - _Unlock_
  - `1` - _Lock_
