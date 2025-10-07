# `GP_CLI_COMMAND_TRACKING_END`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_TRACKING_END` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00F6` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when requesting to end tracking an entity through the wide scan system.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_TRACKING_END
struct GP_CLI_TRACKING_END
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    padding04; // PS2: Dammy
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_TRACKING_END`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `padding00`

_Padding; unused._

The client always sets this value to `0`.
