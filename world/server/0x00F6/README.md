# `GP_SERV_COMMAND_TRACKING_STATE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_TRACKING_STATE` |
| **Client Handlers**       | `RecvState`, `YkWndTrack::RecvListEnd` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00F6` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the server to inform the client of the start or end of a wide scan list request.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_TRACKING_POS_STATE
enum GP_TRACKING_STATE
{
    GP_TRACKING_STATE_NONE,
    GP_TRACKING_STATE_LIST_START,
    GP_TRACKING_STATE_LIST_END,
    GP_TRACKING_STATE_ERR_ETC = 0x0A,
    GP_TRACKING_STATE_END
};

// PS2: GP_SERV_TRACKING_STATE
struct GP_SERV_TRACKING_STATE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    GP_TRACKING_STATE State; // PS2: State
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_TRACKING_STATE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `State`

_The current response state to the clients wide scan request._

| State | Name | Purpose |
| --- | --- | --- |
| `0x00` | `GP_TRACKING_STATE_NONE`         | _None._ |
| `0x01` | `GP_TRACKING_STATE_LIST_START`   | _Begin wide scan list._ |
| `0x02` | `GP_TRACKING_STATE_LIST_END`     | _End wide scan list._ |
| `0x0A` | `GP_TRACKING_STATE_ERR_ETC`      | _Error._ |
| `0x03` | `GP_TRACKING_STATE_END`          | _None._ |

## Additional Information

When the client requests the current wide scan list, the server will respond with this packet first, setting the `State` to `GP_TRACKING_STATE_LIST_START`, to inform the client that the server will begin sending wide scan entries using packet `0x00F4`. When the server has finished sending the wide scan entries, it will respond with this packet again, this time with the `State` set to `GP_TRACKING_STATE_LIST_END` to inform the client the list has been fully populated and is ready for use.
