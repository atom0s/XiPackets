# `GP_CLI_COMMAND_GROUP_COMLINK_MAKE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_GROUP_COMLINK_MAKE` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00C3` |
| **Size**                  | `0x0006` |

## Description

This packet is sent by the client when requesting to create a link pearl for an equipped linkshell.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_GROUP_COMLINK_MAKE
struct GP_CLI_GROUP_COMLINK_MAKE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     State;      // PS2: State
    uint8_t     LinkshellId;// PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_GROUP_COMLINK_MAKE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `State`

_The command state._

The client will default this value to `0`. However, this value does come from the clients input to the given command, such as `/makelinkshell 4`. If this value is anything other than `0`, then it will fail to do anything on the server side.

### `LinkshellId`

_The linkshell id._

This value is set to the linkshell id that is having a pearl created for it.

  - `1` - _Linkshell 1_
  - `2` - _Linkshell 2_
