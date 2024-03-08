# `GP_SERV_COMMAND_REQLOGOUTINFO`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_REQLOGOUTINFO` |
| **Client Handler**        | `RecvLogoutInfo` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x010F` |
| **Size**                  | `0x0008` |

## Description

The purpose of this packet is unknown. The below information is based on the PS2 data.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_REQLOGOUTINFO
struct GP_SERV_REQLOGOUTINFO
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    Mode; // PS2: Mode
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_REQLOGOUTINFO`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Mode`

_The logout mode._

## Additional Information

This packet does not appear to be used any longer and is assumed to be deprecated.

The handler for this packet expects a callback function to be set in order for it to be handled. In order for this callback function to be set, another function must be called which puts the client in a state that would await for this packet response. The client will send a `0x00EC` packet to the server and wait for this packet to be sent back to handle a means of logging out. The method that sends the packet to the server is not implemented or invoked anywhere else in the client, thus both packets appear to be deprecated.

