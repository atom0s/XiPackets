# `GP_SERV_COMMAND_MUSIC`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_MUSIC` |
| **Client Handler**        | `RecvMusic` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x005F` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the server to update a music file to be played.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_MUSIC
struct GP_SERV_MUSIC
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    Slot;       // PS2: Slot
    uint16_t    MusicNum;   // PS2: MusicNum
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_MUSIC`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Slot`

_The music slot to be updated._

This value can be 0 to 7 and represents which music kind will be changed.

| Slot | Purpose |
| --- | --- |
| `0` | _Zone (Day)_ |
| `1` | _Zone (Night)_ |
| `2` | _Combat (Solo)_ |
| `3` | _Combat (Party)_ |
| `4` | _Mount_ |
| `5` | _Dead_ |
| `6` | _Mog House_ |
| `7` | _Fishing_ |

### `MusicNum`

_The music id._
