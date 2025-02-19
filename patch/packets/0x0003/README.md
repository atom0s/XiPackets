# `Packet: 0x0003`

| Information | Notes |
| ---: | --- |
| **Function**  | `_sqPatchPkt_MakeDownLoadRequest` |
| **Direction** | `C -> S` |
| **OpCode**    | `0x0003` |
| **Size**      | `(varies)` |

## Description

This packet is sent by the client when requesting to download part of a file on the `patch` server.

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
    uint32_t    hardware_id;
    uint32_t    application_id;
    uint32_t    name_size;
    char        name[];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `size`, `hash`, `signature`, `opcode`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Patch Server Protocol**](/patch/protocol.md)_

### `chunk_offset`

_The offset within the desired file that the chunk should be read from._

### `chunk_size`

_The size of data the client wishes to receive from the file._

### `hardware_id`

_The id of the hardware that the client is currently on._

### `application_id`

_The id of the application that the client is requesting the information for._

### `name_size`

_The length of the file `name`._

### `name`

_The name of the file the client requesting a chunk of._

This value is the name of the file on the remote server that the client wishes to download part of. It is a null-terminated string with the length defined by `name_size`.

## Additional Information

The client uses this packet to make requests to download files _(or chunks of files)_ from the `patch` server. The clients local buffer size has a maximum limit of 65,536 bytes. If the file that the client wishes to download is larger than this buffer, then the file will be requested in chunks rather than requesting the full file. This chunking is performed by walking the file using the `chunk_offset` and `chunk_size` values to let the server know which part of the file the client is requesting next.

The client will determine if chunking is required and how to walk the file with these requests based on the information that was read from the `patch.cfg` file. For example, here is a block from the `patch.cfg` file for the current `PlayOnline` version.

```
file data/dic/entry.dic {
20030909_A 467376 -265113 -283405202 20030909_A/Direct/data/dic/entry.dic.slc 462402
20061212_3 475128 -286689 -1313077611 20061212_3/Direct/data/dic/entry.dic.slc 472675 20061212_3/Indirect/data/dic/entry.dic.olc 493370
}
```

This block shows the information needed to download and update the `data/dic/entry.dic` file used by `PlayOnline`. The client will iterate each entry (line) in this block to determine which version is the latest and which to compare to the local clients file, if one exists. When an update is required, the client will then read the rest of the 'winning' lines information to determine how to request the file from the remote server.

For example, if this file was going to be updated, the last entry in the block is the latest version. The other information in this line will then be read to determine the needed information to request and download the file. The main pieces of interest are the remote file path and name `20061212_3/Direct/data/dic/entry.dic.slc` and the sizes of the file, both compressed on the server and decompressed on the client after download. Those sizes in this case would be:

  - **Local:** 475128 _(This file size is the uncompressed file size.)_
  - **Remote:** 472675 _(This file size is the size of the file on the remote server, usually compressed.)_

It is possible that the remote file will not be compressed. This is usually in cases where running the file through the compressor would result in a larger file instead of smaller due to the kind of information stored in the file. When this happens, the remote size and local sizes will differ by a single byte due to the compression mode value being stored at the start of the file. _(See below for more information.)_

In this example case, the file size on the server is larger than the clients local 65,536 byte buffer, meaning it will require being downloaded in chunks. When this happens, the client will request 65,536 byte chunks at a time until getting to the final chunk, requesting only the remaining bytes left to finish downloading the full file. This would look something like:

```cpp
_sqPatchPkt_MakeDownLoadRequest(pkt, "20061212_3/Direct/data/dic/entry.dic.slc", 0x00000000, 0x00010000);
_sqPatchPkt_MakeDownLoadRequest(pkt, "20061212_3/Direct/data/dic/entry.dic.slc", 0x00010000, 0x00010000);
_sqPatchPkt_MakeDownLoadRequest(pkt, "20061212_3/Direct/data/dic/entry.dic.slc", 0x00020000, 0x00010000);
_sqPatchPkt_MakeDownLoadRequest(pkt, "20061212_3/Direct/data/dic/entry.dic.slc", 0x00030000, 0x00010000);
_sqPatchPkt_MakeDownLoadRequest(pkt, "20061212_3/Direct/data/dic/entry.dic.slc", 0x00040000, 0x00010000);
_sqPatchPkt_MakeDownLoadRequest(pkt, "20061212_3/Direct/data/dic/entry.dic.slc", 0x00050000, 0x00010000);
_sqPatchPkt_MakeDownLoadRequest(pkt, "20061212_3/Direct/data/dic/entry.dic.slc", 0x00060000, 0x00010000);
_sqPatchPkt_MakeDownLoadRequest(pkt, "20061212_3/Direct/data/dic/entry.dic.slc", 0x00070000, 0x00003663);
```

> [!NOTE]
> The client will do additional steps between each request, this is simply to show the chunk offset and size._

The manner in which this works can be thought of like the `fopen`, `fseek` and `fread` file operation calls in C++. The client will tell the server which file it wishes to open _(via `fopen`)_, where inside of the file it should be begin reading from _(via `fseek`)_ and the size of data that should be read from the file _(via `fread`)_ to create the desired chunk.

The `chunk_size` limit set by the client of 65,536 bytes is not the actual limit imposed by the server. Instead, the server will allow clients to request larger chunks and appears to be a very large value, potentially the max value of an `int32_t`. However, the server does have other limitations in place that makes larger chunks unreliable. The server appears to have a response operation timeout limit of about 15 seconds. If the server takes longer than 15 seconds to complete a response to the client, it will simply close the connection.

Files downloaded from the server using this request can be compressed using the same compression methods that were discussed in the [0x0002](/patch/packets/0x0002/README.md) packet documentation. Instead of containing the `mode` within the packet, the server instead stores the mode within the file data. The first byte in all files that are downloaded from the server determines the compression mode that is being used, even if the file is not compressed. The client will strip this byte from the buffer and process the remaining data as needed when saving the downloaded files.

## Example Packets

```
// English client requesting small files that will not result in chunking:

4F 00 00 00 5D D5 C3 86 50 4F 4C 50 03 00 00 00  |  O...]...POLP....
00 00 00 00 F1 95 00 00 57 32 55 00 31 30 30 30  |  ........W2U.1000
2B 00 00 00 32 30 30 36 31 32 31 32 5F 33 2F 44  |  +...20061212_3/D
69 72 65 63 74 2F 64 61 74 61 2F 64 69 63 2F 65  |  irect/data/dic/e
6E 74 72 79 6E 77 2E 64 69 63 2E 73 6C 63 00     |  ntrynw.dic.slc.

4F 00 00 00 E6 DA 39 0C 50 4F 4C 50 03 00 00 00  |  O.....9.POLP....
00 00 00 00 43 F8 00 00 57 32 55 00 31 30 30 30  |  ....C...W2U.1000
2B 00 00 00 32 30 30 36 31 32 31 32 5F 33 2F 44  |  +...20061212_3/D
69 72 65 63 74 2F 64 61 74 61 2F 64 69 63 2F 76  |  irect/data/dic/v
75 6C 67 61 72 32 2E 64 69 63 2E 73 6C 63 00     |  ulgar2.dic.slc.

// English client requesting large file, with chunking:

4D 00 00 00 9E 22 A8 6B 50 4F 4C 50 03 00 00 00  |  M....".kPOLP....
00 00 00 00 00 00 01 00 57 32 55 00 31 30 30 30  |  ........W2U.1000
29 00 00 00 32 30 30 36 31 32 31 32 5F 33 2F 44  |  )...20061212_3/D
69 72 65 63 74 2F 64 61 74 61 2F 64 69 63 2F 65  |  irect/data/dic/e
6E 74 72 79 2E 64 69 63 2E 73 6C 63 00           |  ntry.dic.slc.

4D 00 00 00 3D 26 D1 92 50 4F 4C 50 03 00 00 00  |  M...=&..POLP....
00 00 01 00 00 00 01 00 57 32 55 00 31 30 30 30  |  ........W2U.1000
29 00 00 00 32 30 30 36 31 32 31 32 5F 33 2F 44  |  )...20061212_3/D
69 72 65 63 74 2F 64 61 74 61 2F 64 69 63 2F 65  |  irect/data/dic/e
6E 74 72 79 2E 64 69 63 2E 73 6C 63 00           |  ntry.dic.slc.

4D 00 00 00 CF BA A5 D5 50 4F 4C 50 03 00 00 00  |  M.......POLP....
00 00 02 00 00 00 01 00 57 32 55 00 31 30 30 30  |  ........W2U.1000
29 00 00 00 32 30 30 36 31 32 31 32 5F 33 2F 44  |  )...20061212_3/D
69 72 65 63 74 2F 64 61 74 61 2F 64 69 63 2F 65  |  irect/data/dic/e
6E 74 72 79 2E 64 69 63 2E 73 6C 63 00           |  ntry.dic.slc.
```