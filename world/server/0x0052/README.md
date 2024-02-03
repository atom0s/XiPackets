# `GP_SERV_COMMAND_EVENTUCOFF`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_EVENTUCOFF` |
| **Client Handler**        | `RecvUcOff` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0052` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the server to adjust the clients current event user control state.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_EVENTUCOFF
struct GP_SERV_EVENTUCOFF
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    Mode; // PS2: Mode
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_EVENTUCOFF`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `Mode`

_The user control mode value._

## Additional Information: `Mode`

The `Mode` value holds the kind of user control release this packet is for. It can also hold event related information depending on the mode being used.

### Mode: `0`

This mode adjusts the clients standard control. When used, the following will happen:

```cpp
if (((PTR_CliEventMode | PTR_CliEventModeLocal) & 0x2000) != 0)
{
    PTR_CliEventUcFlag      = 0;
    PTR_CliEventCancelFlag  = 1;

    FUNC_EventTargetCancel();
}
```


### Mode: `1`

This mode adjusts the clients event receive pending flag. When used, the following will happen:

```cpp
PTR_RecPendingFlag = 0;
```

During events, the client can send the server an update state packet _(either `0x5B` or `0x5C`)_. While waiting for a response from the server, it will set `PTR_RecPendingFlag` to `1` to mark itself as waiting (pending) a response.

### Mode: `2`

This mode is used to cancel the current event the client is within. When this mode is used, an extra value is packed into the `Mode` value and is expected to match the clients current event id in order to properly cancel out. When used, the following will happen:

```cpp
if (PTR_EventExecFlag && pkt->Mode >> 8 == reinterpret_cast<uint16_t>(PTR_CliEventPara))
{
    PTR_CliEventPara2           = pkt->Mode >> 8;
    PTR_ServerReqEventCancel    = 1;
}
```

### Mode: `3`

This mode is used to cancel the clients current numerical or string input during an event. _(ie. Door passwords.)_ When used, the following will happen:

```cpp
PTR_RecPassWdWindowFlag = 0;
```

### Mode: `4`

This mode is used to release the client from an event lock related to fishing. When used, the following will happen:

```cpp
PTR_ClientEventUcFlag2 = 0;
```
