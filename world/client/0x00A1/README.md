# `GP_CLI_COMMAND_SWITCH_VOTE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_SWITCH_VOTE` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00A1` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the client when voting in a proposal. _(via `/vote`)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_SWITCH_VOTE
struct GP_CLI_SWITCH_VOTE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Index;   // PS2: Index
    uint8_t     Name[];  // PS2: Name
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_SWITCH_VOTE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Index`

_The index of the option the client is voting for in the proposal._

### `Name`

_The name of the player who made the proposal the client is voting in._

This value will be the name of the player that made the proposal the client is voting it. The client can give this name when using the `/vote` command. If they do not give a name, then the client will automatically use the name of the player who made the last seen proposal.
