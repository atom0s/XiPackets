# `GP_CLI_COMMAND_DICE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_DICE` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00A2` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when using the `/random` command.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_DICE
struct GP_CLI_DICE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    unknown00; // PS2: Dammy
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_DICE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `unknown00`

_Unknown._

This value is set by the client as the numerical parameter given to the `/random` command. However, it does not appear to have any actual affect. _(It is assumed this was intended to be a range limiter to allow players to roll between 0 and a given number at some point.)_
