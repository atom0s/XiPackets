# `GP_SERV_COMMAND_MYROOM_IS`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_MYROOM_IS` |
| **Client Handler**        | `RecvMyroomIs` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0098` |
| **Size**                  | `0x0008` |

## Description

The purpose of this packet is unknown. The below information is based on the PS2 data.

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
    uint8_t     padding05[3];   // PS2: (New; did not exist.)
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

### `padding05`

_Padding; unused._

## Additional Information

The client does not actually make use of this packet or its data in any manner. While the client does have a handler for this packet, the manner in which it works causes the packet to be ignored. It checks to see if a callback function has been set to handle the packet data, if not it simply exits. The callback function pointer used with this handler is never set, thus, the packet data is never used.
