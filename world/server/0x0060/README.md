# `GP_SERV_COMMAND_MUSICVOLUME`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_MUSICVOLUME` |
| **Client Handler**        | `RecvMusicVolume` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0060` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the server to update the clients music volume.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_MUSICVOLUME
struct GP_SERV_MUSICVOLUME
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    time;
    uint16_t    volume;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_MUSICVOLUME`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### time

_The amount of time to take to reach the given `volume` level._

The client uses linear interpolation to adjust the volume over time based on this value until it reaches the desired level.

### volume

_The volume level._

This value ranges from 0 to 127, with 127 being the loudest.
