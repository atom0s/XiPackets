# `GP_CLI_COMMAND_GAMEOK`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_GAMEOK` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x000C` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the client when the client is in a state, after zoning, that is ready to begin obtaining additional packets from the server.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_GAMEOK
struct GP_CLI_GAMEOK
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    ClientState;            // PS2: ClientState
    uint32_t    DebugClientFlg  : 1;    // PS2: DebugClientFlg
    uint32_t    unused          : 31;   // PS2: unused
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_GAMEOK`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ClientState`

_The client state._

This value is always set to `0`.

### `DebugClientFlg`

_Flag set if the client is a debug client._

This flag value is always set to `0` on the normal retail client.

### `unused`

_Unused._
