# `GP_SERV_COMMAND_FAQ_GMPARAM`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_FAQ_GMPARAM` |
| **Client Handler**        | `FDtRecvGmParam` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00B5` |
| **Size**                  | `0x0020` |

## Description

This packet is sent by the server in response to the client opening the `Help Desk` menu.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_FAQ_GMPARAM
struct GP_SERV_FAQ_GMPARAM
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    RescueCount;    // PS2: RescueCount
    uint32_t    params[4];      // PS2: params
    uint16_t    Id;             // PS2: Id
    uint16_t    Option;         // PS2: Option
    uint16_t    Status;         // PS2: Status
    uint16_t    RescueTime;     // PS2: RescueTime
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_FAQ_GMPARAM`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `RescueCount`

_Unknown._

The client does not use this value.

_This value has been observed to be `0` and non-zero values._

### `params`

_Unknown._

The client does not use this value.

The first value of this array used to be the number of GM calls in the queue. The client would use this value when sending an actual GM report; however this is no longer used. The client will now always send 1 instead.

### `Id`

_The response id._

This value is reflected from what the client sent in its own `0x0D4` - `FDtXiRequestGmParam` request packet. Each time the client opens the menu, it will increment the local request id and send it to the server.

### `Option`

_Unknown._

The client does not use this value.

_This value has been observed to be `0`. This value may reflect the option(s) selected within the `Help Desk` menu._

### `Status`

_Unknown._

The client does not use this value.

_This value has been observed to be `0` or `1`._

### `RescueTime`

_Unknown._

The client does not use this value.

_This value has been observed to be `1800`._

## Additional Information

This packet is sent as a response to the client opening the `Help Desk -> Help Desk` menu. The client sends a `FDtXiRequestGmParam` (`0xD4`) request packet when opening this menu, which this is the response to that request. The client does not actually use anything from this packet except the `Id` to check if its non-zero. If non-zero, the client copies the entire packet _(including the header)_ into a global buffer `PTR_FDtGmParam`. The only value that is used in this buffer again is the `Id` value.

Each time the client opens the `Help Desk` menu, it will increment the `Id` and send a new request. _(In the event that the `Id` rolls over, the client will reset it to `1`.)_
