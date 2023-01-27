# `RequestCreateChrPre`

| Information               | Notes |
|---                        |---    |
| **PS2 Name**              | `RequestCreateChrPre` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0022` |
| **Size**                  | `0x0060` |

## Description

The client request packet sent to the server when creating a new character.

_This packet is sent before the main `RequestCreateChr` packet and is used to validate information used during the character creation process. _(ie. character name, golden ticket, etc.)_

## Packet Layout

```cpp
// PS2: lpkt_makechr_pre
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
    char            character_name[16];         // PS2: character_name
    uint8_t         passwd[16];                 // PS2: passwd
    char            world_name[16];             // PS2: world_name
    char            friend_passwd[16];          // PS2: friend_passwd
};
```

## Packet Fields

### packet_size, terminator, command, identifer

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/packets/lobby/Header.md)_

### ffxi_id

_The selected characters unique id._

_For more information on how the server ids work, please see: [**Server Ids**](/packets/lobby/Notes.md#server-ids)_

### character_name

_The desired character name to be created._

### passwd

_The random generated password MD5 hashed, using the current `md5key` of the clients session._

_For more information on how the passwords work, please see: [**Passwords**](/packets/lobby/Notes.md#passwords)_

### world_name

_The name of the selected world the character is creating their character on._

### friend_passwd

_The gold world pass if it was given during creation._

## Example Packet

```
0000h: 60 00 00 00 49 58 46 46 22 00 00 00 48 41 53 48  `...IXFF"...HASH
0010h: 48 41 53 48 48 41 53 48 48 41 53 48 11 22 33 44  HASHHASHHASH."3D
0020h: 4E 41 4D 45 4E 41 4D 45 4E 41 4D 45 4E 41 4D 45  NAMENAMENAMENAME
0030h: 50 41 53 53 50 41 53 53 50 41 53 53 50 41 53 53  PASSPASSPASSPASS
0040h: 53 45 52 56 53 45 52 56 53 45 52 56 53 45 52 56  SERVSERVSERVSERV
0050h: FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF  ÿÿÿÿÿÿÿÿÿÿÿÿÿÿÿÿ
```

_**Note:** Sensitive data has been removed and replaced with temporary data._
