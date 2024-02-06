# `GP_SERV_COMMAND_PENDINGNUM`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_PENDINGNUM` |
| **Client Handler**        | `RecvPendingNum` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x005C` |
| **Size**                  | `0x0024` |

## Description

This packet is sent by the server to update the clients event work parameters.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_PENDINGNUM
struct GP_SERV_PENDINGNUM
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    int32_t     num[8]; // PS2: num
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_PENDINGNUM`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `num`

The event parameter values to be updated.

The client copies this data into its local `PTR_Work_Zone` buffer, starting at index `2`. _(This buffer is used by the event system to hold information that events can store and read from as needed.)_