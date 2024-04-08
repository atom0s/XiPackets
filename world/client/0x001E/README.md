# `GP_CLI_COMMAND_GM`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_GM` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x001E` |
| **Size**                  | `(varies)` |

## Description

> [!NOTE]
> This is a special GM-related packet!

This packet is sent by the client when using GM commands.

_**Note:** The system to actually make use of this packet is no longer implemented in the retail client. However, the functions that send this packet still exist._

## Packet Layout

The layout of this packet is the following:

```cpp
struct GP_CLI_GM
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Command[]; // PS2: Command
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_GM`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Command`

_The GM command string to execute._
