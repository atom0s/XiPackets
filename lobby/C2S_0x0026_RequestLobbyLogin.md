# `RequestLobbyLogin`

| Information               | Notes |
|---                        |---    |
| **PS2 Name**              | `RequestLobbyLogin` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0026` |
| **Size**                  | `0x0098` |

## Description

The client request packet sent to the server to log into the lobby server and validate the current session.

## Packet Layout

```cpp
// PS2: lpkt_lobby_login3
struct packet_t
{
    //
    // Packet Header
    //

    uint32_t        packet_size;                // PS2: packet_size
    uint32_t        terminator;                 // PS2: terminator
    uint32_t        command;                    // PS2: command
    uint8_t         identifer[16];              // PS2: identifer

    //
    // Packet Data
    //

    char            pol_account[16];            // PS2: pol_account
    uint8_t         client_code[8];             // PS2: client_code
    uint8_t         authCode[64];               // PS2: authCode
    char            versionCode[16];            // PS2: versionCode
    uint32_t        excode_client;              // PS2: excode_client
    uint32_t        unknown0000;                // PS2: (New; did not exist.)
    uint32_t        unknown0001;                // PS2: (New; did not exist.)
    uint32_t        unknown0002;                // PS2: (New; did not exist.)
    uint32_t        unknown0003;                // PS2: (New; did not exist.)
};
```

## Packet Fields

### packet_size, terminator, command, identifer

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/lobby/Header.md)_

### pol_account

_The clients PlayOnline account id._

_**Note:** This field is deprecated on the latest clients. Instead, the `authCode` field is used for login validation. This field is nulled when `authCode` has a value._

### client_code

_The client code value._

_This value is determined based on both the clients language value and another value that is used for rendering adjustments when FFXI is first launched. This value is initialized to `2` when the client first loads, but when initialized, it will be updated to `3` or `5` based on the two bits of information._

### authCode

_The clients newer unique session information used for authentication and validation._

_This value is generated within the polcore.dll module using several bits of information. (The clients IP address, blowfish key, login time, and a random chunk of data.) Once generated it is also encrypted then xor'd against itself. This is not intended to be decrypted/reversed as it is generated using MD5 hashing._

_**Please note:** This value is regenerated under various conditions and not static for a clients entire lifespan._

### versionCode

_The clients version string of FFXiMain.dll. (ie. 30220204\_0)_

### excode_client

_Bit flags that hold information in regards to the clients installation state. This contains things such as the clients installed expansions to tell the server what areas and content the client can access._

### unknown0000

_Unknown._

_The client always sets this value to 0._

### unknown0001

_Unknown._

_The client always sets this value to 0._

### unknown0002

_Unknown._

_The client always sets this value to 0._

### unknown0003

_Unknown._

_The client always sets this value to 0._

## Example Packet

```
0000h: 98 00 00 00 49 58 46 46 26 00 00 00 48 41 53 48  ˜...IXFF&...HASH
0010h: 48 41 53 48 48 41 53 48 48 41 53 48 00 00 00 00  HASHHASHHASH....
0020h: 00 00 00 00 00 00 00 00 00 00 00 00 05 00 00 00  ................
0030h: 00 00 00 00 41 55 54 48 43 4F 44 45 41 55 54 48  ....AUTHCODEAUTH
0040h: 43 4F 44 45 41 55 54 48 43 4F 44 45 41 55 54 48  CODEAUTHCODEAUTH
0050h: 43 4F 44 45 41 55 54 48 43 4F 44 45 41 55 54 48  CODEAUTHCODEAUTH
0060h: 43 4F 44 45 41 55 54 48 43 4F 44 45 41 55 54 48  CODEAUTHCODEAUTH
0070h: 43 4F 44 45 33 30 32 32 30 33 32 39 5F 32 00 6E  CODE30220329_2.n
0080h: 64 6F 77 FF FF 0F 00 00 00 00 00 00 00 00 00 00  dowÿÿ...........
0090h: 00 00 00 00 00 00 00 00                          ........
```

_**Note:** Sensitive data has been removed and replaced with temporary data._
