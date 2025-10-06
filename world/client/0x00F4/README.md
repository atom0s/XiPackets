# `GP_CLI_COMMAND_TRACKING_LIST`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_TRACKING_LIST` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00F4` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when requesting the current wide scan list for the entities around the player.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_TRACKING_LIST
struct GP_CLI_TRACKING_LIST
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    SendFlg; // PS2: SendFlg
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_TRACKING_LIST`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `SendFlg`

_Unknown._

The purpose of this flag is unknown. The client always sets this value to `1`.
