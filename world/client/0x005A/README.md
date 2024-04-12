# `GP_CLI_COMMAND_REQCONQUEST`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_REQCONQUEST` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x005A` |
| **Size**                  | `0x0004` |

## Description

This packet is sent by the client when requesting the current Conquest map information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (Unnamed packet.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_
