# `GP_CLI_COMMAND_BUFFCANCEL`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_BUFFCANCEL` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00F1` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when cancelling a buff.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_BUFFCANCEL
struct GP_CLI_BUFFCANCEL
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    BuffNo;     // PS2: BuffNo
    uint16_t    padding00;  // PS2: Dammy
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_BUFFCANCEL`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `BuffNo`

_The id of the buff being cancelled._

### `padding00`

_Padding; unused._
