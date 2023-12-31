# `GP_SERV_COMMAND_GM`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_GM` |
| **Client Handler**        | `RecvGm` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0012` |
| **Size**                  | `(varies)` |

## Description

> [!NOTE]
> This is a special GM-related packet!

This packet is sent by the server to display a message to chat for Game Masters.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_GM
struct GP_SERV_GM
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Mes[]; // PS2: Mes
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_GM`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `Mes`

_The message to be printed to chat._

## Additional Information

This packet is used for GM related messaging and is not sent to normal clients. This is not used for normal GM communications with players either, those are sent as normal chat packets (`0x0017`) with a flag set to mark the packet as from a GM. The intended purpose of this packet is unknown, but speculations on what it could be for are:

  - Chat channel that only GM's can see and use.
  - Chat channel that is used for GM ticket system output.

The client simply determines the length of the message based on the packet size then prints it directly to chat using chat mode `0x0A`. This mode value is then converted into type `0xC9` internally.
