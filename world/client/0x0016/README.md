# `GP_CLI_COMMAND_CHARREQ`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_CHARREQ` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0016` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client if it is attempting to access an entity it does not have valid data for yet. _(Generally during events.)_

This packet is used to request that the server send an entity update _(`0x000E`)_ packet for a given entity.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_CHARREQ
struct GP_CLI_CHARREQ
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    ActIndex;   // PS2: ActIndex
    uint16_t    padding00;  // PS2: Dammy
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_CHARREQ`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ActIndex`

_The target index of the entity being requested._

### `padding00`

_Padding; unused._
