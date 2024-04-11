# `GP_CLI_COMMAND_TROPHY_ABSENCE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_TROPHY_ABSENCE` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0042` |
| **Size**                  | `0x0006` |

## Description

This packet is sent by the client when passing on treasure pool items.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_TROPHY_ABSENCE
struct GP_CLI_TROPHY_ABSENCE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     TrophyItemIndex;    // PS2: TrophyItemIndex
    uint8_t     padding00;          // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_TROPHY_ABSENCE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `TrophyItemIndex`

_The treasure pool window index of the item being passed on._

### `padding00`

_Padding; unused._
