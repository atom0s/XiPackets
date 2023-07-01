# `RequestRenameChr`

| Information               | Notes |
|---                        |---    |
| **PS2 Name**              | `RequestRenameChr` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0028` |
| **Size**                  | `0x0044` |

## Description

The client request packet sent to the server when renaming a character.

## Packet Layout

```cpp
// PS2: lpkt_renamechr
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
    char            new_name[16];               // PS2: new_name
    uint8_t         passwd[16];                 // PS2: passwd
};
```

## Packet Fields

### packet_size, terminator, command, identifer

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/lobby/Header.md)_

### ffxi_id

_The selected characters unique id._

_For more information on how the server ids work, please see: [**Server Ids**](/lobby/Notes.md#server-ids)_

### ffxi_id_world

_The selected characters in-game server id._

_For more information on how the server ids work, please see: [**Server Ids**](/lobby/Notes.md#server-ids)_

### new_name

_The new name of the selected character._

### passwd

_The random generated password MD5 hashed, using the current `md5key` of the clients session._

_For more information on how the passwords work, please see: [**Passwords**](/lobby/Notes.md#passwords)_

## Example Packet

```
0000h: 44 00 00 00 49 58 46 46 28 00 00 00 48 41 53 48  D...IXFF(...HASH
0010h: 48 41 53 48 48 41 53 48 48 41 53 48 11 22 33 44  HASHHASHHASH."3D
0020h: AA BB CC DD 41 74 6F 6D 6F 73 00 00 00 00 00 00  ª»ÌÝAtomos......
0030h: 00 00 00 00 50 41 53 53 50 41 53 53 50 41 53 53  ....PASSPASSPASS
0040h: 50 41 53 53                                      PASS
```

_**Note:** Sensitive data has been removed and replaced with temporary data._
