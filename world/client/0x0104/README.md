# `GP_CLI_COMMAND_BAZAAR_EXIT`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_BAZAAR_EXIT` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0104` |
| **Size**                  | `0x0004` |

## Description

This packet is sent by the client when exiting a bazaar.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_BAZAAR_EXIT
struct GP_CLI_BAZAAR_EXIT
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_BAZAAR_EXIT`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_
