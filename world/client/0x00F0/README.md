# `GP_CLI_COMMAND_RESCUE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_RESCUE` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00F0` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when requesting to be unstuck via the GM Help Desk system.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_RESCUE
struct GP_CLI_RESCUE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    State; // PS2: State
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_RESCUE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `State`

_The client state._

This value is always set to `0`.

## Additional Information

This packet is sent by the client when requesting to be unstuck via the GM Help Desk system. The client will only send this packet when selecting the following menu option: `Help Desk -> I can't move my character or log out of the game. -> My character is stuck. -> I am stuck and need to get out. My movement is not being restricted by a GM.`
