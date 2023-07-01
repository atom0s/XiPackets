# `RequestQueryWorldList`

| Information               | Notes |
|---                        |---    |
| **PS2 Name**              | `RequestQueryWorldList` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0024` |
| **Size**                  | `0x002C` |

## Description

The client request packet sent to the server to receive the list of available worlds.

## Packet Layout

```cpp
// PS2: lpkt_query_world_list
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

    uint8_t         passwd[16];                 // PS2: passwd
};
```

## Packet Fields

### packet_size, terminator, command, identifer

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/lobby/Header.md)_

### passwd

_The random generated password MD5 hashed, using the current `md5key` of the clients session._

_For more information on how the passwords work, please see: [**Passwords**](/lobby/Notes.md#passwords)_

## Example Packet

```
0000h: 2C 00 00 00 49 58 46 46 24 00 00 00 48 41 53 48  ,...IXFF$...HASH
0010h: 48 41 53 48 48 41 53 48 48 41 53 48 50 41 53 53  HASHHASHHASHPASS
0020h: 50 41 53 53 50 41 53 53 50 41 53 53              PASSPASSPASS
```

_**Note:** Sensitive data has been removed and replaced with temporary data._
