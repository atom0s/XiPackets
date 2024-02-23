# `GP_SERV_COMMAND_MYROOM_ENTER`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_MYROOM_ENTER` |
| **Client Handler**        | `RecvMyroomEnter` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0096` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the server when the client is entering the mog house of another player.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: _GP_MYROOM_GATE
struct _GP_MYROOM_GATE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Result;         // PS2: Result
    uint8_t     padding00[3];   // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`_GP_MYROOM_GATE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Result`

_The packet result._

### `padding00`

_Padding; unused._

## Additional Information

This packet is sent to the client when they have successfully requested to enter into another players mog house. The client does not actually make use of this packet or its data in any manner. While the client does have a handler for this packet, the manner in which it works causes the packet to be ignored. It checks to see if a callback function has been set to handle the packet data, if not it simply exits. The callback function pointer used with this handler is never set, thus, the packet data is never used.

If the client fails to successfully enter another players mog house, then this packet will not be sent. _(In this case, the server will actually not inform the client of the failure at all. It will still play the end of the NPCs event welcoming them into the players house, but nothing will happen.)_
