# `Packet: 0x0006`

| Information | Notes |
| ---: | --- |
| **Function**  | `_sqPatchPkt_MakeShutDown` |
| **Direction** | `C -> S` |
| **OpCode**    | `0x0006` |
| **Size**      | `0x0010` |

## Description

The purpose of this packet is unknown.

> [!NOTE]
> This packet is deprecated and no longer used.

## Packet Layout

The layout of this packet is the following:

```cpp
struct packet_t
{
    uint32_t    size;
    uint32_t    hash;
    uint32_t    signature;
    uint32_t    opcode;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `size`, `hash`, `signature`, `opcode`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Patch Server Protocol**](/patch/protocol.md)_

## Additional Information

The purpose of this packet is not unknown and is no longer used, however the function to generate it still exists. This packet does not contain any additional fields outside of the normal header. It is assumed this packet was originally used by the client to inform the server of when it was disconnecting. Sending this packet to the server now will cause the server to disconnect the client without an error.

## Example Packets

_This packet has not been observed in a live environment. Its handler is never called or referenced._