# `GP_SERV_COMMAND_MYROOM_PLACE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_MYROOM_PLACE` |
| **Client Handler**        | `RecvMyroomJob` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x009E` |
| **Size**                  | `0x0017` |

## Description

The purpose of this packet is unknown. The below information is based on the PS2 data.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: _GP_MYROOM_JOB
struct _GP_MYROOM_JOB
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    int8_t      job_lev[16];    // PS2: job_lev
    int8_t      curjob[2];      // PS2: curjob
    int8_t      second;         // PS2: second
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`_GP_MYROOM_JOB`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

## Additional Information

The naming of this packet and the usage of other `Myroom` packets implies this may have been used allow players to switch their jobs while in another players mog house, or to be able to view the other players unlocked jobs and job levels.

The client does not actually make use of this packet or its data in any manner. While the client does have a handler for this packet, the manner in which it works causes the packet to be ignored. It checks to see if a callback function has been set to handle the packet data, if not it simply exits. The callback function pointer used with this handler is never set, thus, the packet data is never used.
