<div align="center">
    <img width="200" src="https://github.com/atom0s/XiPackets/raw/main/repo/icon.png" alt="XiPackets">
    </br>
</div>

# Patch Server Reversing

This document holds various information related to the reverse engineering of the patch server communications within the client files.

The main functionality that the client makes use of to communicate with the `patch` server is stored within the `PlayOnline` module `app.dll`. Additionally, there is also a copy of most of the same functions _(with some changes)_ stored within the main `PlayOnline` module, `polcore.dll`. _It is assumed the cloned copies of the functions within `polcore.dll` are used to allow external applications to also make use of the patch system by simply including in `polcore.dll` and making use of the needed functions._

## Hardware Tags

The patch system allows for multiple platforms (and languages) to coexist for the same application, such as the `PlayOnline` client for PC, PS2, and Xbox. In order to properly handle patching, a hardware tag is used to differentiate between the various platforms. The following table contains the known platform hardware tags that have been observed:

| Tag | Platform |
| :---: | --- |
| `DWU` | _Unknown (Assumed to be debug client related.)_ |
| `DWJ` | _Unknown (Assumed to be debug client related.)_ |
| `W20` | _Windows (Japanese Client)_ |
| `W2U` | _Windows (English Client)_ |
| `PS2` | _PlayStation 2_ |

_**Note:** The hardware tag is not always enforced in the expected manner. For example, it is possible for the English client to make use of the expected `W2U` tag but later use the `W20` tag when version checking `Final Fantasy XI`._

## Application Product Ids

When communicating with the `patch` server, the client will include the id of the given application _(also known as a product)_. This allows the server to know which product the client is requesting information for. _(Product ids are formatted as `%04d` when used.)_

| Product Id | Flag | Tag Name | Unicode Name | Ascii Name | Unk. Name |
| :---: | :---: | --- | --- | --- | --- |
| `0`       | `0` | `UNDEFINED`             | `UNDEFINED CONTENTS`              | `UNDEFINED CONTENTS`                  | `UNSUPPORTED PROFILE` |
| `1`       | `1` | `FINALFANTASY_XI`       | `FINAL FANTASY XI`                | `FINAL FANTASY XI`                    | `FINAL FANTASY XI` |
| `2`       | `1` | `TETRAMASTER`           | `Tetra Master`                    | `Tetra Master`                        | `Tetra Master` |
| `3`       | `1` | `MAHJONG`               | `JongHoLow`                       | `JongHoLow`                           | `JongHoLow` |
| `4`       | `1` | `FRONTMISSION_ONLINE`   | `Front Mission Online`            | `Front Mission Online`                | `Front Mission Online` |
| `5`       | `0` | `MAIL_FORWARD`          | `Mail Forward`                    | `Mail Forward`                        | `null` |
| `6`       | `0` | `CHARNAMEADDR`          | `Character Name Address`          | `Character Name Address`              | `null` |
| `7`       | `0` | `MAIL_FILTER`           | `Mail Filter`                     | `Mail Filter`                         | `null` |
| `8`       | `0` | `FINALFANTASY_XIV`      | `FINAL FANTASY XIV`               | `FINAL FANTASY XIV`                   | `FINAL FANTASY XIV` |
| `9`       | `0` | `(none)`                | `(none)`                          | `(none)`                              | `null` |
| `10`      | `1` | `DC_FFVII`              | `DC -FFVII-`                      | `DC -FFVII-`                          | `DC -FFVII-` |
| `11`      | `1` | `FANTASY_EARTH`         | `FANTASY EARTH`                   | `FANTASY EARTH`                       | `FANTASY EARTH` |
| `12`      | `1` | `TEMP_OS_012`           | `TEMP_OS_012`                     | `TEMP_OS_012`                         | `TEMP_OS_012` |
| `13`      | `0` | `EVER_QUEST_II`         | `EVER QUEST II`                   | `EVER QUEST II`                       | `null` |
| `14`      | `0` | `POL_FL_APP`            | `PlayOnline Friend List Applicat` | `PlayOnline Friend List Application`  | `null` |
| `15`      | `1` | `FINALFANTASY_XI_TEST`  | `FINAL FANTASY XI Test Server`    | `FINAL FANTASY XI Test Server`        | `FINAL FANTASY XI Test Server` |
| `999`     | `1` | `SAMPLE`                | `Sample`                          | `Sample`                              | `null` |
| `1000`    | `0` | `NAVIGATOR`             | `Handle`                          | `Handle`                              | `Handle Profile` |
| `1001`    | `0` | `CHAT`                  | `Chat`                            | `Chat`                                | `null` |
| `1002`    | `0` | `CHAT`                  | `Chat`                            | `Chat`                                | `null` |
| `1003`    | `0` | `CHAT`                  | `Chat`                            | `Chat`                                | `null` |
| `1004`    | `0` | `KBSEARCH`              | `KB Search`                       | `KB Search`                           | `null` |
| `1005`    | `0` | `KBSEARCH_IDX`          | `KB Search Index`                 | `KB Search Index`                     | `null` |

## Function Signatures - `app.dll`

The following tables hold patterns that can be used to find the various functions related to the `patch` server communications within the `app.dll` module.

### Main Patch Loops / State Machines

| Function Name | Pattern |
| --- | --- |
| <a href="#_sqPatchMain_ProcSub">`_sqPatchMain_ProcSub`</a>        | `55 8B EC 81 EC 3C 08 00 00 56 57 8B 45 08 89 85` |
| <a href="#_sqntTcpClient">`_sqntTcpClient`</a>                    | `55 8B EC B8 ?? ?? ?? ?? E8 ?? ?? ?? ?? 8B 45 08` |
| <a href="#PatchFileSys_ThreadMain">`PatchFileSys_ThreadMain`</a>  | `55 8B EC 81 EC 0C 06 00 00 8B 45 0C 89` |

### Patch Server Packet Methods

| OpCode | Direction | Function Name | Pattern |
| :---: | :---: | --- | --- |
| `---`     | `---`     | <a href="#_sqPatchPkt_GetDigest">`_sqPatchPkt_GetDigest`</a>                      | `55 8B EC 83 EC 68 8D 45 98 50 E8 ?? ?? ?? ?? 83` |
| `0x0001`  | `C -> S`  | <a href="#_sqPatchPkt_MakeStatusRequest">`_sqPatchPkt_MakeStatusRequest`</a>      | `55 8B EC 51 8B 45 08 89 45 FC 8B 4D FC C7 01 18 00 00 00` |
| `0x0002`  | `S -> C`  | <a href="#_sqPatchPkt_CnvStatusReply">`_sqPatchPkt_CnvStatusReply`</a>            | `89 41 0C B8 01 00 00 00 8B E5 5D C3` |
| `0x0003`  | `C -> S`  | <a href="#_sqPatchPkt_MakeDownLoadRequest">`_sqPatchPkt_MakeDownLoadRequest`</a>  | `55 8B EC 83 EC 10 8B 45 08 89 45 FC 8B 4D 0C 51 E8` |
| `0x0004`  | `S -> C`  | <a href="#_sqPatchPkt_CnvSendFile">`_sqPatchPkt_CnvSendFile`</a>                  | `89 41 0C 8B 4D F8 8B 55 F8 8B 42 10 89` |
| `0x0005`  | `S -> C`  | <a href="#_sqPatchPkt_CnvError">`_sqPatchPkt_CnvError`</a>                        | `89 41 0C 8B 4D F8 8B 51 04 3B 55 FC 74 ?? 33 C0 EB` |
| `0x0006`  | `C -> S`  | <a href="#_sqPatchPkt_MakeShutDown">`_sqPatchPkt_MakeShutDown`</a>                | `55 8B EC 51 8B 45 08 89 45 FC 8B 4D FC C7 01 10 00 00 00` |
| `0x0007`  | `C -> S`  | <a href="#_sqPatchPkt_MakeAskNewest">`_sqPatchPkt_MakeAskNewest`</a>              | `55 8B EC 51 8B 45 08 89 45 FC 8B 4D FC C7 01 58 00 00 00` |
| `0x0008`  | `S -> C`  | <a href="#_sqPatchPkt_CnvReplyNewest">`_sqPatchPkt_CnvReplyNewest`</a>            | `89 41 0C 8B 4D F8 8B 55 F8 8B 42 58 89 41 58` |

> [!NOTE]
> Some of the above patterns are mid-function due to the functions sharing a common starting block of instructions!

### Additional Methods of Interest

| Function Name | Pattern |
| --- | --- |
| <a href="#CalcVerNumFromStr">`CalcVerNumFromStr`</a>                                                  | `55 8B EC 83 EC 10 56 C7 45 F8 00 00 00 00 C7 45 FC 00 00 00 00` |
| <a href="#PatchMain_MakeFileList">`PatchMain_MakeFileList`</a>                                        | `55 8B EC 83 EC 0C 8B 45 08 89 45 F4 8B 4D F4 C7` |
| <a href="#__sqUDirectPatchDecompressor_decompress">`__sqUDirectPatchDecompressor_decompress`</a>      | `55 8B EC 81 EC A8 00 00 00 89 8D 58 FF FF FF 8D 4D CC E8` |
| <a href="#__sqUIndirectPatchDecompressor_decompress">`__sqUIndirectPatchDecompressor_decompress`</a>  | `55 8B EC 81 EC D4 00 00 00 89 8D 2C FF FF FF 8D 4D BC E8` |
| <a href="#zlib_inflate_init">`zlib_inflate_init`</a>                                                  | `FF 74 24 0C FF 74 24 0C 6A 0F FF 74 24 10 E8` |
| <a href="#zlib_inflate">`zlib_inflate`</a>                                                            | `53 55 56 8B 74 24 10 33 DB 3B F3 57 0F` |
| <a href="#zlib_inflate_end">`zlib_inflate_end`</a>                                                    | `56 8B 74 24 08 85 F6 74 ?? 8B 46 1C 85 C0` |

# Function Information

## <a name="_sqPatchMain_ProcSub"></a> Function: `_sqPatchMain_ProcSub`

Function used to process the main patch system operations.

This is the main function that handles the majority of the patch system operations. It is designed as a state machine style function where it will be constantly called every frame while the patch process is happening. The state object will track a current counter to determine which step in the process is being handled currently. This function is a large switch case for each of the valid steps that can take place during the patching/updating process. Most of the operations within this function are calls to queue additional commands to the patch file system which are then processed within the `PatchFileSys_ThreadMain` thread callback.

## <a name="_sqntTcpClient"></a> Function: `_sqntTcpClient`

Function used to process the patch file system packet flow.

This function handles the communications between the client and `patch` server. The socket handling makes use of non-blocking sockets to allow for async reading/writing.

## <a name="PatchFileSys_ThreadMain"></a> Function: `PatchFileSys_ThreadMain`

Function used to process the patch file system commands.

This function is a thread callback that is used process queued file commands from the `_sqPatchMain_ProcSub` function. This includes commands such as reading, writing, decompressing, removing, renaming, etc. The following table contains the current list of commands that are processed in this function.

| Id | Command |
| :---: | --- |
| `0x00` | _Reset (Error)_ |
| `0x01` | _Open File (Read/Write)_ |
| `0x02` | _Read File_ |
| `0x03` | _Close File_ |
| `0x04` | _Write File_ |
| `0x05` | _Delete File_ |
| `0x06` | _Move/Rename File_ |
| `0x07` | _File Length (1) (Deprecated)_ |
| `0x08` | _File Length (2) (Time/Size)_ |
| `0x09` | _File Length (3) (Deprecated)_ |
| `0x0A` | _Decompress File (Direct)_ |
| `0x0B` | _Decompress File (Indirect)_ |
| `0x0C` | _Move/Rename File (After Reboot)_ |
| `0x0D` | _Get Free Disk Space_ |
| `0x0E` | _File Length (4)_ |
| `0x0F` | _File Checksum_ |

_**Note:** The file length handling is done using the `_stat32` CRT function in most of these calls. If the file is already open, it is possible that the `GetFileInformationByHandle` API call will be used instead._

## <a name="_sqPatchPkt_GetDigest"></a> Function: `_sqPatchPkt_GetDigest`

Function used to calculate a `patch` server packet hash. _(Uses `MD5` algorithm for hashing.)_

## <a name="_sqPatchPkt_MakeStatusRequest"></a> Function: `_sqPatchPkt_MakeStatusRequest`

Function used to build a `StatusRequest` request packet to be sent to the server.

## <a name="_sqPatchPkt_CnvStatusReply"></a> Function: `_sqPatchPkt_CnvStatusReply`

Function used to process a `StatusReply` response packet from the server.

## <a name="_sqPatchPkt_MakeDownLoadRequest"></a> Function: `_sqPatchPkt_MakeDownLoadRequest`

Function used to build a `DownloadRequest` request packet to be sent to the server.

## <a name="_sqPatchPkt_CnvSendFile"></a> Function: `_sqPatchPkt_CnvSendFile`

Function used to process a `SendFile` response packet from the server.

## <a name="_sqPatchPkt_CnvError"></a> Function: `_sqPatchPkt_CnvError`

Function used to process an `Error` response packet from the server.

## <a name="_sqPatchPkt_MakeShutDown"></a> Function: `_sqPatchPkt_MakeShutDown`

Function used to build a `Shutdown` request packet to be sent to the server.

## <a name="_sqPatchPkt_MakeAskNewest"></a> Function: `_sqPatchPkt_MakeAskNewest`

Function used to build an `AskNewest` request packet to be sent to the server.

## <a name="_sqPatchPkt_CnvReplyNewest"></a> Function: `_sqPatchPkt_CnvReplyNewest`

Function used to process an `AskNewest` response packet from the server.

## <a name="CalcVerNumFromStr"></a> Function: `CalcVerNumFromStr`

Function used to convert a version string into a numeric value, used for version comparisons.

## <a name="PatchMain_MakeFileList"></a> Function: `PatchMain_MakeFileList`

Function used to parse the `patch.cfg` file blocks; preparing the list of files that are handled through the patch system.

## <a name="__sqUDirectPatchDecompressor_decompress"></a> <a name="__sqUIndirectPatchDecompressor_decompress"></a> Function: `__sqUDirectPatchDecompressor_decompress`, `__sqUIndirectPatchDecompressor_decompress`

Functions used to handle custom bit based decompression.

These function calls are part of two separate classes _(`__sqUDirectPatchDecompressor` and `__sqUIndirectPatchDecompressor`)_ that handle the decompression of some downloaded files, mainly `patch.cfg`. _(This compression method uses run-length encoding, similar to LZSS/LZH.)_

## <a name="zlib_inflate_init"></a> <a name="zlib_inflate"></a> <a name="zlib_inflate_end"></a> Function: `zlib_inflate_init`, `zlib_inflate`, `zlib_inflate_end`

Functions used to handle zlib decompression.

These function calls are implemented via the zlib library. _(Specifically using version `1.1.4` of zlib.)_

---

_This information is based on the `PlayOnline` client version `1.18.15e`. (`20110829_E`)_