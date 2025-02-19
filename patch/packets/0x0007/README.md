# `Packet: 0x0007`

| Information | Notes |
| ---: | --- |
| **Function**  | `_sqPatchPkt_MakeAskNewest` |
| **Direction** | `C -> S` |
| **OpCode**    | `0x0007` |
| **Size**      | `0x0058` |

## Description

This packet is sent by the client when requesting a specific hardware / application combinations newest version information.

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
    char        version[64];
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

### `version`

_The clients current version version._

This value is optional and may be empty if the clients current version is unknown. This is common when first installing `PlayOnline`, or another supported title, and the first update has not been performed yet.

## Additional Information

In order for the client to populate the data for this packet _(mainly the `version` field)_ it must decrypt the local `patch.ver` file for the current application. This file is `encrypted` using a key stored in the system registry. On Windows, these keys are stored inside of the following registry paths:

  - **JP:** `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\PlayOnline\Interface`
  - **NA:** `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\PlayOnlineUS\Interface`
  - **EU:** `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\PlayOnlineEU\Interface`

The `patch.ver` files are encrypted and simply store the local version string for the given application. These files are used for the update checks to determine if the client needs to be updated or not when new versions are pushed to the patch server. These files are encrypted to help prevent easy modification of the version string allowing clients to connect to the services with outdated files. If this file is invalid, missing or the current key present in the registry was not the one used to encrypt the file and it fails to decrypt, then the `version` field will be left empty. The server does not require this value to be set for a valid response to be sent back.

_It is normal for an application to be installed and have no `patch.ver` file present. This helps ensure that the application will require and force an initial update upon the first attempt of launching it, ensuring it is up to date. In these cases, the `version` field will be empty._

## Example Packets

```
// English client requesting latest PlayOnline version, local version is missing/unknown:

58 00 00 00 A9 1E 49 9E  50 4F 4C 50 07 00 00 00  |  X.....I.POLP....
57 32 55 00 31 30 30 30  00 00 00 00 00 00 00 00  |  W2U.1000........
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |  ................
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |  ................
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |  ................
00 00 00 00 00 00 00 00                           |  ........

// English client requesting latest PlayOnline version, local version is known:

58 00 00 00 A6 D6 73 88  50 4F 4C 50 07 00 00 00  |  X.....s.POLP....
57 32 55 00 31 30 30 30  32 30 31 31 30 38 32 39  |  W2U.100020110829
5F 45 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |  _E..............
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |  ................
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |  ................
00 00 00 00 00 00 00 00                           |  ........
```