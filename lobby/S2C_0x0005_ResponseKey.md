# `ResponseKey`

| Information               | Notes |
|---                        |---    |
| **PS2 Name**              | `Unknown` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0005` |
| **Size**                  | `0x0028` |

## Description

The server response packet sent to the client after logging into the lobby server.

## Packet Layout

```cpp
// PS2: lpkt_key
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

    uint32_t        key;                        // PS2: key
    uint32_t        excode_server;              // PS2: excode_server
    uint32_t        excode_server2;             // PS2: (New; did not exist.)
};
```

## Packet Fields

### packet_size, terminator, command, identifer

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/packets/lobby/Header.md)_

### key

_The MD5 key the client will begin using for any hashing performed._

### excode_server

_The extended code bit flags. Holds the available expansion packs that the server has available._

### excode_server2

_The extended code bit flags (2). Holds the available additional services that are registered on the current account. (ie. security token, satchels, wardrobes, etc.)_

## Example Packet

```
0000h: 28 00 00 00 49 58 46 46 05 00 00 00 48 41 53 48  (...IXFF....HASH
0010h: 48 41 53 48 48 41 53 48 48 41 53 48 BB 87 75 CF  HASHHASHHASH»‡uÏ
0020h: FF 0F 00 00 01 00 00 00                          ÿ.......
```

_**Note:** Sensitive data has been removed and replaced with temporary data._
