# `Packet: 0x0008`

| Information | Notes |
| ---: | --- |
| **Function**  | `_sqPatchPkt_CnvReplyNewest` |
| **Direction** | `S -> C` |
| **OpCode**    | `0x0008` |
| **Size**      | `(varies)` |

## Description

This packet is sent by the server when responding to a clients newest version information request.

## Packet Layout

The layout of this packet is the following:

```cpp
struct packet_t
{
    uint32_t    size;
    uint32_t    hash;
    uint32_t    signature;
    uint32_t    opcode;

    uint32_t    timestamp;
    uint32_t    unknown;
    uint8_t     status[64];
    uint32_t    version_size;
    char        version[];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `size`, `hash`, `signature`, `opcode`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Patch Server Protocol**](/patch/protocol.md)_

### `timestamp`

_The Unix timestamp of the version._

### `reset`

_Unknown._

The purpose of this value is currently unknown. It has been observed to always be set to `1`. It is known internally to the client as a reset flag.

### `status`

_The version status._

This value contains a block of null-separated strings. See below for more information.

### `version_size`

_The version string size._

### `version`

_The version string._

## Additional Information

The `status` field holds a block of null-separated strings that are returned from the server regarding the status of the response. These strings are used to inform the client how the request was processed and how it should continue further with the patching process, if required.

The first string in this block determines the validation of the clients local version string that was sent with the original request. This string can be one of the following values:

| Value | Notes |
| --- | --- |
| `empty`       | _The request version value was empty._ |
| `registered`  | _The request version was set and the server has considered it valid._ |
| `unknown`     | _The request version was set but the server does not consider it valid._ |

If the client did not send a version string in the original request, then this string will be set to `empty`. If the version string was set, then this string will be set to either `registered` or `unknown` based on the server side validation. This validation does not seem to function properly in the sense that it actually cares about the version is and if its a valid/known version to the server. Instead, it will only check the first byte of the version. If the first byte is 0x32, 0x33, 0x34, 0x35, 0x36, 0x37, 0x38 or 0x39 then the server considers the version valid and will return `registered`. Otherwise, it will return `unknown` for this string.

The second string is the IP address of the patch server that the client should connect to for any further requests. This includes any further requests such as `0x0001` or `0x0003` requests when downloading files from the server.

The remaining string(s) are read but passed to a null stub function. Their purpose is unknown at this time.

## Example Packets

```
// English client response to requesting PlayOnlines version information:
// (Client sent valid local version string.)

67 00 00 00 C3 6A 95 F5  50 4F 4C 50 08 00 00 00  |  g....j..POLP....
97 64 4E 4E 01 00 00 00  72 65 67 69 73 74 65 72  |  .dNN....register
65 64 00 32 30 32 2E 36  37 2E 36 32 2E 31 30 32  |  ed.202.67.62.102
00 30 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |  .0..............
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |  ................
00 00 00 00 00 00 00 00  0B 00 00 00 32 30 31 31  |  ............2011
30 38 32 39 5F 45 00                              |  0829_E.

// English client response to requesting PlayOnlines version information:
// (Client sent empty local version string.)

67 00 00 00 71 E4 0A 99  50 4F 4C 50 08 00 00 00  |  g...q...POLP....
CA 62 4E 4E 01 00 00 00  65 6D 70 74 79 00 32 30  |  .bNN....empty.20
32 2E 36 37 2E 36 32 2E  37 30 00 30 00 00 00 00  |  2.67.62.70.0....
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |  ................
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |  ................
00 00 00 00 00 00 00 00  0B 00 00 00 32 30 31 31  |  ............2011
30 38 32 39 5F 45 00                              |  0829_E.

// English client response to requesting Final Fantasy XI version information:
// (Client sent out of date version: 30220329_1)

67 00 00 00 FE 97 2C 99  50 4F 4C 50 08 00 00 00  |  g.....,.POLP....
04 36 5D 62 01 00 00 00  72 65 67 69 73 74 65 72  |  .6]b....register
65 64 00 32 30 32 2E 36  37 2E 36 32 2E 37 30 00  |  ed.202.67.62.70.
30 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |  0...............
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |  ................
00 00 00 00 00 00 00 00  0B 00 00 00 33 30 32 32  |  ............3022
30 33 32 39 5F 32 00                              |  0329_2.
```