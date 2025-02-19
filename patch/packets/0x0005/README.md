# `Packet: 0x0005`

| Information | Notes |
| ---: | --- |
| **Function**  | `_sqPatchPkt_CnvError` |
| **Direction** | `S -> C` |
| **OpCode**    | `0x0005` |
| **Size**      | `0x0010` |

## Description

This packet is sent by the server when an error has occurred.

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

This packet is used by the server to inform the client when an error has occurred from a previous request. However, it is not used for every error condition. In some cases, the server may disconnect the client, by force, without any response being sent first.

This packet does not contain any information regarding what error occurred either. Instead, it is up to the client to determine what potentially errored based on the current state it is in within the patching process. _(This is generally easy for the client to do based on how the state machine is setup and being able to track what the last operation that was performed by the client was before the error happened.)_

For more information on when this packet is sent, please see: [**Patch Server Protocol**](/patch/protocol.md)

## Example Packets

```
10 00 00 00 AF 48 FF 63 50 4F 4C 50 05 00 00 00  |  .....H.cPOLP....
```