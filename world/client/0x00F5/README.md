# `GP_CLI_COMMAND_TRACKING_START`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_TRACKING_START` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00F5` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when requesting to begin tracking an entity through the wide scan system.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_TRACKING_START
struct GP_CLI_TRACKING_START
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    ActIndex; // PS2: ActIndex
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_TRACKING_START`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ActIndex`

_The target index of the entity the client wishes to track._
