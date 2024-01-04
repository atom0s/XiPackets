# `GP_SERV_COMMAND_CHANNEL_ITEM`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_CHANNEL_ITEM` |
| **Client Handler**        | `RecvChannelItem` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x002B` |
| **Size**                  | `(unknown)` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

The purpose of this packet is unknown. The below information is based on the PS2 data.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_CHANNEL_ITEM
struct GP_SERV_CHANNEL_ITEM
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
	uint8_t     PropertyItemIndex;  // PS2: PropertyItemIndex
	uint8_t     ChannelIndex;       // PS2: ChannelIndex
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_TELL`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `PropertyItemIndex`

_Unknown._

### `ChannelIndex`

_Unknown._
