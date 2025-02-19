# `Packet: 0x0004`

| Information | Notes |
| ---: | --- |
| **Function**  | `_sqPatchPkt_CnvSendFile` |
| **Direction** | `S -> C` |
| **OpCode**    | `0x0004` |
| **Size**      | `(varies)` |

## Description

This packet is sent by the server when responding to a client download request.

## Packet Layout

The layout of this packet is the following:

```cpp
struct packet_t
{
    uint32_t    size;
    uint32_t    hash;
    uint32_t    signature;
    uint32_t    opcode;

    uint32_t    chunk_offset;
    uint32_t    chunk_size;
    uint32_t    name_size;
    char        name[];
    uint8_t     data[];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `size`, `hash`, `signature`, `opcode`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Patch Server Protocol**](/patch/protocol.md)_

### `chunk_offset`

_The offset within the desired file that the chunk starts at._

### `chunk_size`

_The size of data._

### `name_size`

_The length of the file `name`._

### `name`

_The name of the file the client requested._

### `data`

_The file data_

The size of this array will equal the `chunk_size` value.

## Additional Information

For more information, please refer to the [0x0003](/patch/packets/0x0003/README.md) packet documentation.

## Example Packets

```
// English client response for small file that will not result in chunking:

8A F8 00 00 F1 21 16 4A 50 4F 4C 50 04 00 00 00  |  .....!.JPOLP....
00 00 00 00 43 F8 00 00 2B 00 00 00 32 30 30 36  |  ....C...+...2006
31 32 31 32 5F 33 2F 44 69 72 65 63 74 2F 64 61  |  1212_3/Direct/da
74 61 2F 64 69 63 2F 76 75 6C 67 61 72 32 2E 64  |  ta/dic/vulgar2.d
69 63 2E 73 6C 63 00                             |  ic.slc.
<snipped rest of file data..>

// English client response for large file that results in chunking. (Chunks 0, 1, and 7 (last))

45 00 01 00 C0 52 B0 42 50 4F 4C 50 04 00 00 00  |  E....R.BPOLP....
00 00 00 00 00 00 01 00 29 00 00 00 32 30 30 36  |  ........)...2006
31 32 31 32 5F 33 2F 44 69 72 65 63 74 2F 64 61  |  1212_3/Direct/da
74 61 2F 64 69 63 2F 65 6E 74 72 79 2E 64 69 63  |  ta/dic/entry.dic
2E 73 6C 63 00 03 78 9C 00 0B 40 F4 BF 63 71 25  |  .slc..x...@..cq%
<snipped rest of file data..>

45 00 01 00 88 26 8B 28 50 4F 4C 50 04 00 00 00  |  E....&.(POLP....
00 00 01 00 00 00 01 00 29 00 00 00 32 30 30 36  |  ........)...2006
31 32 31 32 5F 33 2F 44 69 72 65 63 74 2F 64 61  |  1212_3/Direct/da
74 61 2F 64 69 63 2F 65 6E 74 72 79 2E 64 69 63  |  ta/dic/entry.dic
2E 73 6C 63 00                                   |  .slc.
<snipped rest of file data..>

A8 36 00 00 E0 09 55 E2 50 4F 4C 50 04 00 00 00  |  .6....U.POLP....
00 00 07 00 63 36 00 00 29 00 00 00 32 30 30 36  |  ....c6..)...2006
31 32 31 32 5F 33 2F 44 69 72 65 63 74 2F 64 61  |  1212_3/Direct/da
74 61 2F 64 69 63 2F 65 6E 74 72 79 2E 64 69 63  |  ta/dic/entry.dic
2E 73 6C 63 00                                   |  .slc.
<snipped rest of file data..>
```