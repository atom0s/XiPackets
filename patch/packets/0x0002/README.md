# `Packet: 0x0002`

| Information | Notes |
| ---: | --- |
| **Function**  | `_sqPatchPkt_CnvStatusReply` |
| **Direction** | `S -> C` |
| **OpCode**    | `0x0002` |
| **Size**      | `(varies)` |

## Description

This packet is sent by the server when responding to a previous client status request. This response will contain the requested hardware / application combination patch configuration file. _(`patch.cfg`)_

## Packet Layout

The layout of this packet is the following:

```cpp
struct packet_t
{
    uint32_t    size;
    uint32_t    hash;
    uint32_t    signature;
    uint32_t    opcode;

    uint8_t     mode;
    uint8_t     data[];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `size`, `hash`, `signature`, `opcode`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Patch Server Protocol**](/patch/protocol.md)_

### `mode`

_The compression mode used for the data._

This value controls the manner in which the `data` field is populated and how it should be read.

| Mode | Notes |
| --- | --- |
| `0` | _The `data` is compressed. (Mode 1)_ |
| `1` | _The `data` is uncompressed (raw)._ |
| `2` | _The `data` is compressed. (Mode 1)_ |
| `3` | _The `data` is compressed. (Mode 2)_ |

  - **Mode 1**
    - _The file data is compressed using a LZ-like bit-packed compression scheme. This is commonly used for the `patch.cfg` file._
  - **Mode 2**
    - _The file data is compressed using ZLib. (with the header in-tact)_

The `mode` value does not appear to be strictly enforced for specific files or file types. Instead, it appears to be used at the discretion of the server based on which method compressed the file best at the time of packing. The following notes are based on what has been observed:

  - Mode `0x00` has not been observed, however the client treats it the same as using compression mode 1.
  - Mode `0x01` has been observed for general file updates.
  - Mode `0x02` has been observed for the main `patch.cfg` files.
  - Mode `0x03` has been observed for general file updates.

### `data`

_The file data._

## Example Packets

```
// English client receiving latest patch information for PlayOnline:

73 93 00 00 09 A9 B7 B4 50 4F 4C 50 02 00 00 00  |  s.......POLP....
02 E9 9A 04 00 CC A4 61 53 06 04 9C 30 74 C6 A0  |  .......aS...0t..
<snipped rest of file data..>

// Japanese client receiving latest patch information for PlayOnline:

B3 BF 00 00 97 48 DC B8 50 4F 4C 50 02 00 00 00  |  .....H..POLP....
02 EC FC 05 00 CC A4 61 53 06 04 9C 30 74 C6 A0  |  .......aS...0t..
<snipped rest of file data..>

// Japenese and English client receiving latest patch information for FFXI:

CC 12 1E 00 00 7A E7 23 50 4F 4C 50 02 00 00 00  |  .....z.#POLP....
02 B5 95 F0 00 CC A4 61 53 06 04 9C 30 74 C6 A0  |  .......aS...0t..
<snipped rest of file data..>
```