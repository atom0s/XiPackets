# `GP_CLI_COMMAND_REQLOGOUTINFO`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_REQLOGOUTINFO` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00EC` |
| **Size**                  | `0x0004` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

The purpose of this packet is unknown.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_REQLOGOUTINFO
struct GP_CLI_REQLOGOUTINFO
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_REQLOGOUTINFO`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_
