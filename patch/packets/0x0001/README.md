# `Packet: 0x0001`

| Information | Notes |
| ---: | --- |
| **Function**  | `_sqPatchPkt_MakeStatusRequest` |
| **Direction** | `C -> S` |
| **OpCode**    | `0x0001` |
| **Size**      | `0x0018` |

## Description

This packet is sent by the client when requesting a specific hardware / application combinations latest patch configuration file. _(`patch.cfg`)_

## Packet Layout

The layout of this packet is the following:

```cpp
struct packet_t
{
    uint32_t    size;
    uint32_t    hash;
    uint32_t    signature;
    uint32_t    opcode;

    uint32_t    hardware_id;
    uint32_t    application_id;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `size`, `hash`, `signature`, `opcode`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Patch Server Protocol**](/patch/protocol.md)_

### `hardware_id`

_The id of the hardware that the client is currently on._

### `application_id`

_The id of the application that the client is requesting the information for._

## Additional Information

The client will send its current hardware and application ids when making this request, however the hardware id is not alway enforced. In some cases, the application that the client is requesting information for shares data between multiple languages and platforms, removing the need for the hardware id to be unique. In cases like this, the client may make use of a different hardware code than would be otherwise expected.

## Example Packets

```
// English client requesting PlayOnline patch information:

18 00 00 00 AC A7 27 BC 50 4F 4C 50 01 00 00 00  |  ......'.POLP....
57 32 55 00 31 30 30 30                          |  W2U.1000

// English client requesting Final Fantasy XI patch information:

18 00 00 00 A7 18 C3 7B 50 4F 4C 50 01 00 00 00  |  ........POLP....
57 32 55 00 30 30 30 31                          |  W2U.0001

// Japanese client requesting PlayOnline patch information:

18 00 00 00 6A D7 29 2F 50 4F 4C 50 01 00 00 00  |  ....j.)/POLP....
57 32 30 00 31 30 30 30                          |  W20.1000

// Japanese client requesting Final Fantasy XI patch information:

18 00 00 00 BF 12 A6 01 50 4F 4C 50 01 00 00 00  |  ........POLP....
57 32 30 00 30 30 30 31                          |  W20.0001
```