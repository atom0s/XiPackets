# `GP_SERV_COMMAND_MYROOM_DIARY`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_MYROOM_DIARY` |
| **Client Handler**        | `RecvMyroomDiary` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x009D` |
| **Size**                  | `0x0034` |

## Description

The purpose of this packet is unknown. The below information is based on the PS2 data.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: _GP_MYROOM_DIARY_DATA
struct _GP_MYROOM_DIARY_DATA
{
    uint32_t    KillCount;  // PS2: KillCount
    uint32_t    DeadCount;  // PS2: DeadCount
    uint16_t    Rare[10];   // PS2: Rare
    uint16_t    Event[10];  // PS2: Event
};

// PS2: _GP_MYROOM_DIARY
struct _GP_MYROOM_DIARY
{
    uint16_t                id: 9;
    uint16_t                size: 7;
    uint16_t                sync;
    _GP_MYROOM_DIARY_DATA   data; // PS2: data
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`_GP_MYROOM_DIARY`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `data`

_The diary data._

## Structure Fields (`_GP_MYROOM_DIARY_DATA`)

### `KillCount`

_Unknown._

### `DeadCount`

_Unknown._

### `Rare`

_Unknown._

### `Event`

_Unknown._

## Additional Information

The naming of this packet and the usage of other `Myroom` packets implies this may have been used allow players to view another players lifetime statistics. _(Kill count, death count, other interaction stats, etc.)_

The client does not actually make use of this packet or its data in any manner. While the client does have a handler for this packet, the manner in which it works causes the packet to be ignored. It checks to see if a callback function has been set to handle the packet data, if not it simply exits. The callback function pointer used with this handler is never set, thus, the packet data is never used.
