# `GP_CLI_COMMAND_MYROOM_DANCER`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_MYROOM_DANCER` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0101` |
| **Size**                  | `0x0004` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

> [!NOTE]
> This is a special GM-related packet!

This packet is used with the GM command `//dancer`. The purpose of this packet is unknown.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_MYROOM_DANCER
struct GP_CLI_MYROOM_DANCER
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_MYROOM_DANCER`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_
