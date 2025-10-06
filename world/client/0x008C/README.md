# `GP_CLI_COMMAND_PREFERENCE_READ`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_PREFERENCE_READ` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x008C` |
| **Size**                  | `0x0006` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

> [!NOTE]
> This is a special GM-related packet!

This packet is used with the GM command `//getpref`. The purpose of this packet is unknown.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_PREFERENCE_READ
struct GP_CLI_PREFERENCE_READ
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    padding00; // PS2: Dummy
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_PREFERENCE_READ`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_
