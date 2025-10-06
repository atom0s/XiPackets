# `GP_CLI_COMMAND_GROUP_TALK`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_GROUP_TALK` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0075` |
| **Size**                  | `(varies)` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

The purpose of this packet is unknown. While this packet was present in the PS2 beta as well, it was not implemented or used.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_GROUP_TALK
struct GP_CLI_GROUP_TALK
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    padding00;  // PS2: dammy
    uint8_t     Kind;       // PS2: Kind
    uint8_t     Attr;       // PS2: Attr
    uint8_t     Str[];      // PS2: Str
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_GROUP_TALK`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `padding00`

_Padding; unused._

### `Kind`

_The packet kind._

### `Attr`

_The packet attribute._

### `Str`

_The message string._
