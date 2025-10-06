# `GP_CLI_COMMAND_GMCOMMAND`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_GMCOMMAND` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x001F` |
| **Size**                  | `(varies)` |

## Description

> [!NOTE]
> This is a special GM-related packet!

This packet is sent by the client in response to a remote GM command execution request.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_GMCOMMAND
struct GP_CLI_GMCOMMAND
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    GMUniqueNo; // PS2: GMUniqueNo
    uint8_t     Command[];  // PS2: Command
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_GMCOMMAND`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `GMUniqueNo`

_The server id of the GM._

### `Command`

_The GM command string._

## Additional Information

The client has several commands built into it to return information to a GM when requested. A GM client can cause a normal players client to execute a given command locally, then respond with information from the executed command using this packet.

For example, there is a remote GM command called `eventinfo`. If a GM sends this command to a client, it will send this packet in response with the `Command` information populated with information related to the clients current event state.
