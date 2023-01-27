# `ResponseNextLogin`

| Information               | Notes |
|---                        |---    |
| **PS2 Name**              | `Unknown` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x000B` |
| **Size**                  | `0x0048` |

## Description

The server response packet sent to the client when a successful character selection request has occurred.

_This packet contains the information used to connect the client to the proper game and search servers._

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

    uint32_t        ffxi_id;                    // PS2: ffxi_id
    uint32_t        ffxi_id_world;              // PS2: ffxi_id_world
    char            character_name[16];         // PS2: character_name
    uint32_t        server_id;                  // PS2: server_id
    uint32_t        server_ip;                  // PS2: server_ip
    uint32_t        server_port;                // PS2: server_port
    uint32_t        cache_ip;                   // PS2: cache_ip
    uint32_t        cache_port;                 // PS2: cache_port
};
```

## Packet Fields

### packet_size, terminator, command, identifer

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/packets/lobby/Header.md)_

### ffxi_id

_The selected characters unique id._

_For more information on how the server ids work, please see: [**Server Ids**](/packets/lobby/Notes.md#server-ids)_

### ffxi_id_world

_The selected characters in-game server id._

_For more information on how the server ids work, please see: [**Server Ids**](/packets/lobby/Notes.md#server-ids)_

### character_name

_The name of the selected character._

### server_id

_The game server id the client will connect to._

### server_ip

_The game server IP address the client will connect to._

### server_port

_The game server port the client will connect to._

### cache_ip

_The search server IP address the client will use._

### cache_port

_The search server port the client will use._

## Example Packet

```
0000h: 48 00 00 00 49 58 46 46 0B 00 00 00 48 41 53 48  H...IXFF....HASH
0010h: 48 41 53 48 48 41 53 48 48 41 53 48 11 22 33 44  HASHHASHHASH."3D
0020h: AA BB CC DD 41 74 6F 6D 6F 73 00 00 00 00 00 00  ª»ÌÝAtomos......
0030h: 00 00 00 00 02 00 00 00 2C 21 16 0B D7 D3 00 00  ........,!..×Ó..
0040h: 2C 21 16 0B F2 D2 00 00                          ,!..òÒ..
```

_**Note:** Sensitive data has been removed and replaced with temporary data._
