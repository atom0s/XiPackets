# `RequestSelectChr`

| Information               | Notes |
|---                        |---    |
| **PS2 Name**              | `RequestSelectChr` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0007` |
| **Size**                  | `0x0058` |

## Description

The client request packet sent to the server when selecting a character to play.

## Packet Layout

```cpp
// PS2: lpkt_select_chr
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
    uint8_t         passwd[16];                 // PS2: passwd
    uint32_t        unknown0000;                // PS2: (New; did not exist.)
    uint8_t         authcode_checksum[16];      // PS2: (New; did not exist.)
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

### passwd

_The random generated password MD5 hashed, using the current `md5key` of the clients session._

_For more information on how the passwords work, please see: [**Passwords**](/packets/lobby/Notes.md#passwords)_

### unknown0000

_This value is potentially not the desired value being sent to the server that SE originally intended._

_When FFXI first starts, it creates an internal checksum of both `FFXi.dll` and `FFXiMain.dll` in an array. This packet appears like it was supposed to send the hash of either of the DLLs, but instead sends the index value from the table that was built. Thus, it is always sent as a value of `3`. Based on how the code works, this looks like a possible mistake/error in the code._

### authcode_checksum

_This value is the current MD5 hash of a custom built block of data containing the clients current authentication code, random generated password, and generated hash values of `FFXi.dll` and `FFXiMain.dll`. However, due to potential bugs in the code, this value may be generated using the wrong set of data. It is still simply used as a unique auth checksum._

## Example Packet

```
0000h: 58 00 00 00 49 58 46 46 07 00 00 00 48 41 53 48  X...IXFF....HASH
0010h: 48 41 53 48 48 41 53 48 48 41 53 48 11 22 33 44  HASHHASHHASH."3D
0020h: AA BB CC DD 41 74 6F 6D 6F 73 00 00 00 00 00 00  ª»ÌÝAtomos......
0030h: 00 00 00 00 50 41 53 53 50 41 53 53 50 41 53 53  ....PASSPASSPASS
0040h: 50 41 53 53 03 00 00 00 48 41 53 48 48 41 53 48  PASS....HASHHASH
0050h: 48 41 53 48 48 41 53 48                          HASHHASH
```

_**Note:** Sensitive data has been removed and replaced with temporary data._
