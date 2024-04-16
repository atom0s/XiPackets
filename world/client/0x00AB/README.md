# `GP_CLI_COMMAND_GUILD_BUYLIST`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_GUILD_BUYLIST` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00AB` |
| **Size**                  | `0x0004` |

## Description

This packet is sent by the client when requesting the current guild stock. _(When opening the `Buy` window.)_

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

## Additional Information

The client uses this packet when it needs to request the current guild vendors item stock. This can happen if the client gets into a state where the current local guild stock information becomes invalidated or is too old and needs to be refreshed.
