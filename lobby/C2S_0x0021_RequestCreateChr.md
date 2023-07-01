# `RequestCreateChr`

| Information               | Notes |
|---                        |---    |
| **PS2 Name**              | `RequestCreateChr` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0021` |
| **Size**                  | `0x0090` |

## Description

The client request packet sent to the server when creating a new character.

_This packet is sent after the initial `RequestCreateChrPre` has been successfully validated._

## Packet Layout

```cpp
// PS2: lpkt_makechr2
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
    uint8_t         passwd[16];                 // PS2: passwd
    uint8_t         character_info[96];         // PS2: character_info
};
```

## Packet Fields

### packet_size, terminator, command, identifer

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/lobby/Header.md)_

### ffxi_id

_The selected characters unique id._

_For more information on how the server ids work, please see: [**Server Ids**](/lobby/Notes.md#server-ids)_

### passwd

_The random generated password MD5 hashed, using the current `md5key` of the clients session._

_For more information on how the passwords work, please see: [**Passwords**](/lobby/Notes.md#passwords)_

### character_info

_The block of data containing the character creation information._

_This block of data uses a shared data type, for information on its layout please see: [**Character Info**](/lobby/CharacterInfo.md)_

_**Note:** Only certain bits of the `character_info` field are populated and used with character creation. Not all data is used/honored or looked at on the server end._

## Example Packet

```
0000h: 90 00 00 00 49 58 46 46 21 00 00 00 48 41 53 48  ....IXFF!...HASH
0010h: 48 41 53 48 48 41 53 48 48 41 53 48 11 22 33 44  HASHHASHHASH."3D
0020h: 50 41 53 53 50 41 53 53 50 41 53 53 50 41 53 53  PASSPASSPASSPASS
0030h: 46 46 46 46 46 46 46 46 46 46 46 46 46 46 46 46  FFFFFFFFFFFFFFFF
0040h: 46 46 46 46 46 46 46 46 46 46 46 46 46 46 46 46  FFFFFFFFFFFFFFFF
0050h: 46 46 46 46 46 46 46 46 46 46 46 46 46 46 46 46  FFFFFFFFFFFFFFFF
0060h: 46 46 46 46 46 46 46 46 46 46 46 46 46 46 46 46  FFFFFFFFFFFFFFFF
0070h: 46 46 46 46 46 46 46 46 46 46 46 46 46 46 46 46  FFFFFFFFFFFFFFFF
0080h: 46 46 46 46 46 46 46 46 46 46 46 46 46 46 46 46  FFFFFFFFFFFFFFFF
```

_**Note:** Sensitive data has been removed and replaced with temporary data._
