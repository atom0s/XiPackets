# `GP_CLI_COMMAND_LOGOUT`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_LOGOUT` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x000B` |
| **Size**                  | `0x001C` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

This packet was originally used during the PS2 beta when logging out and with a command that no longer exists. _(`/zonechg`)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_LOGOUT
struct GP_CLI_LOGOUT
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    ZoneNo;     // PS2: ZoneNo
    uint32_t    MapNo;      // PS2: MapNo
    int32_t     x;          // PS2: x
    int32_t     y;          // PS2: y
    int32_t     z;          // PS2: z
    uint16_t    State;      // PS2: State
    uint8_t     padding00;  // PS2: padding00
    uint8_t     dir;        // PS2: dir
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_LOGOUT`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ZoneNo`

_The zone number the client wishes to change to._

### `MapNo`

_The map number the client wishes to change to._

### `x`

_The requested X position the client wishes to be placed when changing zones._

### `y`

_The requested Y position the client wishes to be placed when changing zones._

### `z`

_The requested Z position the client wishes to be placed when changing zones._

### `State`

_The logout state._

The following enumeration is believed to be used with this state value.

| Id | Value |
| --- | --- |
| `0`   | `GP_GAME_LOGOUT_STATE_NONE` |
| `1`   | `GP_GAME_LOGOUT_STATE_LOGOUT` |
| `2`   | `GP_GAME_LOGOUT_STATE_ZONECHANGE` |
| `3`   | `GP_GAME_LOGOUT_STATE_MYROOM` |
| `4`   | `GP_GAME_LOGOUT_STATE_CANCELL` |
| `5`   | `GP_GAME_LOGOUT_STATE_POLEXIT` |
| `6`   | `GP_GAME_LOGOUT_STATE_JOBEXIT` |
| `7`   | `GP_GAME_LOGOUT_STATE_POLEXIT_MYROOM` |
| `8`   | `GP_GAME_LOGOUT_STATE_TIMEOUT` |
| `9`   | `GP_GAME_LOGOUT_STATE_GMLOGOUT` |
| `10`  | `GP_GAME_LOGOUT_STATE_END` |

### `padding00`

_Padding; unused._

### `dir`

_The requested direction the client wishes to be when changing zones._

## Additional Information

This packet was originally used to tell the server that the client wishes to log out in some manner. There was multiple states in which the client could log out, or be logged out, that are now no longer used. Normal logout requests by the client would set the `State` value to `GP_GAME_LOGOUT_STATE_LOGOUT`.

This packet was also used with a now removed command `/zonechg` which was likely used with GM accounts. This command would allow the client to force-zone themselves to another area and give the initial position the client should be loaded into at upon zoning. This packet is now deprecated and no longer used by the client, however the function that sends this packet still exists and the layout of the packet has not changed since the PS2 beta. When this command was used, the `State` value of this packet was set to `GP_GAME_LOGOUT_STATE_ZONECHANGE`.
