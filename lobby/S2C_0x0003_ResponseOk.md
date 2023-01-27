# `ResponseOk`

| Information               | Notes |
|---                        |---    |
| **PS2 Name**              | `Unknown` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0003` |
| **Size**                  | `0x0020` |

## Description

The server response packet sent to the client when a previous request was successfully completed.

## Packet Layout

```cpp
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

    uint32_t        unknown0000;                // PS2: (Unknown; not used.)
};
```

## Packet Fields

### packet_size, terminator, command, identifer

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/packets/lobby/Header.md)_

### unknown0000

_Unknown._

_This field is never referenced within the client. It was always observed to be `0x10`, but the actual value or its purpose is unknown._

## Example Packet

```
0000h: 20 00 00 00 49 58 46 46 03 00 00 00 48 41 53 48   ...IXFF....HASH
0010h: 48 41 53 48 48 41 53 48 48 41 53 48 10 00 00 00  HASHHASHHASH....
```

_**Note:** Sensitive data has been removed and replaced with temporary data._
