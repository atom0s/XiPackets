# `GP_CLI_COMMAND_REQSUBMAPNUM`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_REQSUBMAPNUM` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00EB` |
| **Size**                  | `0x0004` |

## Description

This packet is sent by the client during events where it needs information about a sub-map.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_REQSUBMAPNUM
struct GP_CLI_REQSUBMAPNUM
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_REQSUBMAPNUM`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_
