# `GP_SERV_COMMAND_CHANNEL_STATE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_CHANNEL_STATE` |
| **Client Handler**        | `RecvChannelState` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x002C` |
| **Size**                  | `(unknown)` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

The purpose of this packet is unknown. The below information is based on the PS2 data.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_CHANNEL_STATE
struct GP_SERV_CHANNEL_STATE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    UniqueNo;       // PS2: UniqueNo
    uint32_t    ZoneNo;         // PS2: ZoneNo
    uint32_t    IP;             // PS2: IP
    uint16_t    ActIndex;       // PS2: ActIndex
    uint16_t    Port;           // PS2: Port
    uint8_t     ChannelIndex;   // PS2: ChannelIndex
    uint8_t     HP;             // PS2: HP
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_CHANNEL_STATE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `UniqueNo`

_Unknown._

### `ZoneNo`

_Unknown._

### `IP`

_Unknown._

### `ActIndex`

_Unknown._

### `Port`

_Unknown._

### `ChannelIndex`

_Unknown._

### `HP`

_Unknown._
