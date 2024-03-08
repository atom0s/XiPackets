# `GP_SERV_COMMAND_REQSUBMAPNUM`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_REQSUBMAPNUM` |
| **Client Handler**        | `RecvSubMapNum` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x010E` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the server to update the clients sub map number.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_REQSUBMAPNUM
struct GP_SERV_REQSUBMAPNUM
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    MapNum; // PS2: MapNum
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_REQSUBMAPNUM`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `MapNum`

_The sub map number._

## Additional Information

This packet is sent by the server in response to the client requesting a sub-map number. When the client is in certain events, it may request a sub-map number from the server in order to continue the event successfully. This is done using the event opcode `0x00A6`. When this opcode is used, the client will send a request to the server using the packet id `0x00EB` _(`GP_CLI_SUBMAPCHANGE`)_. The server will respond with the needed sub-map number for the client to successfully continue the event they are waiting within.

When the client receives this packet, it will store the given sub-map number into the local players information and store it into the event VMs parameters. The event will then be continued and usually, this opcode is followed by another expected opcode `0x0075` which is used to change the clients sub-region on a map. It will do this using the value that was stored into the event VMs parameters from the previous `0x00A6` opcode. _(Depending on how the `0x0075` opcode is called, it may result in the client telling the server of its sub-map change using packet `0x00F2`.)_

_This packet has been observed to be used during the ending rank mission for Windurst (9-2) Moon Reading. During the final cutscene, the player will be teleported from `Heavens Tower` to `Windurst Walls` just outside of the tower entrance. When this happens, the event will make use of the opcode `0x00A6` requesting the sub-map information. For this mission, the return value used for `MapNum` in this packet has been 0._
