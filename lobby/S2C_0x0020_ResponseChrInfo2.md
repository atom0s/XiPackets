# `ResponseChrInfo2`

| Information               | Notes |
|---                        |---    |
| **PS2 Name**              | `Unknown` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0020` |
| **Size**                  | `(varies)` |

## Description

The server response packet sent to the client when a successful character list request has occurred.

This packet contains the accounts available characters and free content id slots. The size of this packet will vary based on the number of character entries returned. Content ids that are available to create new characters are also sent in this packet, just with most of their data not filled in. _(**Note:** Characters are considered content ids. When no actual character data is registered it is still considered a content id.)_

The smallest size this packet can be is `0x20` bytes. _(This should technically never happen as PlayOnline will block you from trying to enter the game if no available content ids exists.)_

Other examples of this packets size are:

  - With 1 character available, the size will be `0xAC`.
  - With 3 characters available, the size will be `0x1C4`.
  - With all 16 characters available, the size will be `0x8E0`.

## Packet Layout

```cpp
// PS2: lpkt_chr_info_sub2
struct lpkt_chr_info_sub2
{
    uint32_t            ffxi_id;                    // PS2: ffxi_id
    uint16_t            ffxi_id_world;              // PS2: ffxi_id_world
    uint16_t            worldid;                    // PS2: worldid
    uint16_t            status;                     // PS2: status
    uint8_t             renamef;                    // PS2: renamef
    uint8_t             ffxi_id_world_tbl;          // PS2: (New; did not exist.)
    char                character_name[16];         // PS2: character_name
    char                world_name[16];             // PS2: world_name
    TC_OPERATION_MAKE   character_info;             // PS2: character_info
};

// PS2: lpkt_chr_info2
struct packet_t
{
    //
    // Packet Header
    //

    uint32_t            packet_size;                // PS2: packet_size
    uint32_t            terminator;                 // PS2: terminator
    uint32_t            command;                    // PS2: command
    uint8_t             identifer[16];              // PS2: identifer

    //
    // Packet Data
    //

    uint32_t            characters;                 // PS2: characters
    lpkt_chr_info2      character_info[characters]; // PS2: character_info
};
```

## Packet Fields

### packet_size, terminator, command, identifer

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/packets/lobby/Header.md)_

### characters

_The number of character information blocks this packet contains._

### character_info

_The array of available character (or content id) information._

## Packet Fields (`lpkt_chr_info2`)

### ffxi_id

_The characters unique id._

_For more information on how the server ids work, please see: [**Server Ids**](/packets/lobby/Notes.md#server-ids)_

### ffxi_id_world

_The characters in-game server id._

_For more information on how the server ids work, please see: [**Server Ids**](/packets/lobby/Notes.md#server-ids)_

### worldid

_The characters server world id._

_For more information on valid server world ids, please see: [**Character Info**](/packets/lobby/CharacterInfo.md)_

### status

_The status of the character slot._

  - `0` - `Invalid` _(Invalid; the character/slot will be hidden if this value is used.)_
  - `1` - `Available` _(Used for characters that are available to be played, and slots available to create new characters in.)_
  - `2` - `Disabled (Unpaid)` _(Used for characters that exist but are not playable due to the content id not being paid for currently.)_

### renamef

_Flag that states if the character is required to be renamed before it can be played again._

  - `0` - `No Rename Required` _(All slots will use this value by default.)_
  - `1` - `Rename Required` _(The character will be prompted to rename before it can be played again. Name will show yellow in the character list as well.)_

### ffxi_id_world_tbl

_The characters in-game server id. (hi-byte)_

_For more information on how the server ids work, please see: [**Server Ids**](/packets/lobby/Notes.md#server-ids)_

### character_name

_The name of the character._

### world_name

_The name of the world the character is registered to._

### character_info

_The block of data containing the character creation information._

_This block of data uses a shared data type, for information on its layout please see: [**Character Info**](/packets/lobby/CharacterInfo.md)_

## Example Packet

_No characters available: (This should not happen.)_
```
0000h: 20 00 00 00 49 58 46 46 20 00 00 00 48 41 53 48   ...IXFF ...HASH
0010h: 48 41 53 48 48 41 53 48 48 41 53 48 00 00 00 00  HASHHASHHASH....
```

_One character registered and available:_
```
0000h: AC 00 00 00 49 58 46 46 20 00 00 00 48 41 53 48  ¬...IXFF ...HASH
0010h: 48 41 53 48 48 41 53 48 48 41 53 48 01 00 00 00  HASHHASHHASH....
0020h: 11 22 33 44 AA BB 7F 00 01 00 00 08 41 74 6F 6D  ."3Dª».....Atom
0030h: 6F 73 00 00 00 00 00 00 00 00 00 00 41 73 75 72  os..........Asur
0040h: 61 00 00 00 00 00 00 00 00 00 00 00 05 00 06 13  a...............
0050h: 05 00 02 00 00 00 00 00 0A 01 D6 11 D6 21 D6 31  ..........Ö.Ö!Ö1
0060h: D6 41 D6 51 6D 63 CB 71 48 63 01 00 02 00 00 00  ÖAÖQmcËqHc......
0070h: 00 00 06 00 FE 01 1C 02 4B 02 D2 00 65 3D 0B 00  ....þ...K.Ò.e=..
0080h: 7F 00 08 00 01 01 63 63 63 01 63 01 01 01 01 01  .....ccc.c.....
0090h: 01 01 01 01 11 22 33 44 FF FF FF FF 00 00 00 00  ....."3Dÿÿÿÿ....
00A0h: 01 00 00 00 1A 00 00 00 00 00 00 00              ............
```

_One character registered, one character unavailable due to being unpaid, one additional slot available to create a new character:_
```
0000h: C4 01 00 00 49 58 46 46 20 00 00 00 48 41 53 48  Ä...IXFF ...HASH
0010h: 48 41 53 48 48 41 53 48 48 41 53 48 03 00 00 00  HASHHASHHASH....
0020h: 11 22 33 44 AA BB 7F 00 01 00 00 08 41 74 6F 6D  ."3Dª».....Atom
0030h: 6F 73 00 00 00 00 00 00 00 00 00 00 41 73 75 72  os..........Asur
0040h: 61 00 00 00 00 00 00 00 00 00 00 00 05 00 06 13  a...............
0050h: 05 00 02 00 00 00 00 00 0A 01 D6 11 D6 21 D6 31  ..........Ö.Ö!Ö1
0060h: D6 41 D6 51 6D 63 CB 71 48 63 01 00 02 00 00 00  ÖAÖQmcËqHc......
0070h: 00 00 06 00 11 11 22 22 33 33 44 44 11 22 33 44  ......""33DD."3D
0080h: 7F 00 08 00 01 01 63 63 63 01 63 01 01 01 01 01  .....ccc.c.....
0090h: 01 01 01 01 11 22 33 44 FF FF FF FF 00 00 00 00  ....."3Dÿÿÿÿ....
00A0h: 01 00 00 00 1A 00 00 00 00 00 00 00 11 22 33 45  ............."3E
00B0h: AA BC 7F 00 02 00 00 09 41 74 6F 6D 6F 73 74 77  ª¼.....Atomostw
00C0h: 6F 00 00 00 00 00 00 00 41 73 75 72 61 00 00 00  o.......Asura...
00D0h: 00 00 00 00 00 00 00 00 01 00 06 13 05 00 02 00  ................
00E0h: 00 00 04 08 0A 01 D6 11 D6 21 D6 31 D6 41 D6 51  ......Ö.Ö!Ö1ÖAÖQ
00F0h: 6D 63 CB 71 32 63 00 00 02 00 00 00 00 00 06 00  mcËq2c..........
0100h: 11 11 22 22 33 33 44 44 11 22 33 44 7F 00 08 00  ..""33DD."3D...
0110h: 01 01 63 01 63 63 63 01 01 01 01 01 01 01 01 01  ..c.ccc.........
0120h: 11 22 33 44 FF FF FF FF 00 00 00 00 00 00 00 00  ."3Dÿÿÿÿ........
0130h: 1C 00 00 00 00 00 00 00 11 22 33 46 00 00 00 00  ........."3F....
0140h: 01 00 00 00 20 00 00 00 00 00 00 00 7F 00 00 00  .... ..........
0150h: 41 73 75 72 20 00 00 00 00 00 00 00 00 00 00 00  Asur ...........
0160h: 00 00 00 00 05 00 06 00 05 00 00 00 01 01 E2 FE  ..............âþ
0170h: 0B 05 00 10 08 20 08 30 08 40 08 50 15 60 00 70  ..... .0.@.P.`.p
0180h: E7 01 01 00 02 00 00 00 01 00 00 00 46 00 00 00  ç...........F...
0190h: 1E 00 00 00 BA 00 00 00 7E 00 00 00 01 01 01 01  ....º...~.......
01A0h: 01 01 01 01 01 01 01 01 01 01 01 01 85 6A A1 27  ............…j¡'
01B0h: 0A 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
01C0h: 00 00 00 00                                      ....
```

_**Note:** Sensitive data has been removed and replaced with temporary data._
