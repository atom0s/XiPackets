# `ResponseWorldList`

| Information               | Notes |
|---                        |---    |
| **PS2 Name**              | `Unknown` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0023` |
| **Size**                  | `(varies)` |

## Description

The server response packet sent to the client when a successful world list request has occurred.

This packet contains the available servers the player can select when creating a new character. The size of this packet will vary based on the number of server entries returned.

## Packet Layout

```cpp
// PS2: lpkt_world_name
struct lpkt_world_name
{
    uint32_t        no;                         // PS2: no
    char            name[16];                   // PS2: name
};

// PS2: lpkt_world_list
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

    uint32_t        sumofworld;                 // PS2: sumofworld
    lpkt_world_name world_name[sumofworld];     // PS2: world_name
};
```

## Packet Fields

### packet_size, terminator, command, identifer

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/packets/lobby/Header.md)_

### sumofworld

_The number of server information blocks this packet contains._

### world_name

_The array of available servers._

## Packet Fields (`lpkt_world_name`)

### no

_The server worldid number._

### name

_The server name._

## Example Packet

```
0000h: 60 01 00 00 49 58 46 46 23 00 00 00 48 41 53 48  `...IXFF#...HASH
0010h: 48 41 53 48 48 41 53 48 48 41 53 48 10 00 00 00  HASHHASHHASH....
0020h: 64 00 00 00 42 61 68 61 6D 75 74 00 00 00 00 00  d...Bahamut.....
0030h: 00 00 00 00 65 00 00 00 53 68 69 76 61 00 00 00  ....e...Shiva...
0040h: 00 00 00 00 00 00 00 00 68 00 00 00 50 68 6F 65  ........h...Phoe
0050h: 6E 69 78 00 00 00 00 00 00 00 00 00 69 00 00 00  nix.........i...
0060h: 43 61 72 62 75 6E 63 6C 65 00 00 00 00 00 00 00  Carbuncle.......
0070h: 6A 00 00 00 46 65 6E 72 69 72 00 00 00 00 00 00  j...Fenrir......
0080h: 00 00 00 00 6B 00 00 00 53 79 6C 70 68 00 00 00  ....k...Sylph...
0090h: 00 00 00 00 00 00 00 00 6C 00 00 00 56 61 6C 65  ........l...Vale
00A0h: 66 6F 72 00 00 00 00 00 00 00 00 00 6E 00 00 00  for.........n...
00B0h: 4C 65 76 69 61 74 68 61 6E 00 00 00 00 00 00 00  Leviathan.......
00C0h: 6F 00 00 00 4F 64 69 6E 00 00 00 00 00 00 00 00  o...Odin........
00D0h: 00 00 00 00 73 00 00 00 51 75 65 74 7A 61 6C 63  ....s...Quetzalc
00E0h: 6F 61 74 6C 00 00 00 00 74 00 00 00 53 69 72 65  oatl....t...Sire
00F0h: 6E 00 00 00 00 00 00 00 00 00 00 00 77 00 00 00  n...........w...
0100h: 52 61 67 6E 61 72 6F 6B 00 00 00 00 00 00 00 00  Ragnarok........
0110h: 7A 00 00 00 43 65 72 62 65 72 75 73 00 00 00 00  z...Cerberus....
0120h: 00 00 00 00 7C 00 00 00 42 69 73 6D 61 72 63 6B  ....|...Bismarck
0130h: 00 00 00 00 00 00 00 00 7E 00 00 00 4C 61 6B 73  ........~...Laks
0140h: 68 6D 69 00 00 00 00 00 00 00 00 00 7F 00 00 00  hmi............
0150h: 41 73 75 72 61 00 00 00 00 00 00 00 00 00 00 00  Asura...........
```

_**Note:** Sensitive data has been removed and replaced with temporary data._
