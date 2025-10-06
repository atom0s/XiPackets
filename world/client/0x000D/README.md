# `GP_CLI_COMMAND_NETEND`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_NETEND` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x000D` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when it is being logged out of a zone.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_NETEND
struct GP_CLI_NETEND
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    State;      // PS2: State
    uint16_t    padding00;  // PS2: Dammy
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_NETEND`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `State`

_The client state._

This value is always set to 0.

### `padding00`

_Padding; unused._

## Additional Information

The client sends this packet when it receives an `0x000B` packet from the server, depending on that packets `LogoutState`. If the `LogoutState` is `GP_GAME_LOGOUT_STATE_CANCEL` (`4`), then this packet will not be sent.
