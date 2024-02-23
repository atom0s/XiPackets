# `GP_SERV_COMMAND_NARAKU`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_NARAKU` |
| **Client Handler**        | `RecvNaraku` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0006` |
| **Size**                  | `(unknown)` |

## Description

This packet is sent by the server to request a clients previous collision information. _(GM related.)_

## Packet Layout

The current layout of this packet is unknown.

_Due to the nature of what this packet is for, there are currently no known packet logs that have captured this packet._

```cpp
// PS2: GP_SERV_NARAKU
struct GP_SERV_NARAKU
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_PACKETCONTROL`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

## Additional Information

When the client receives this packet, it will be put into a state where it will begin reporting collision position data back to the server. The client sends this data back to the server using the `GP_CLI_GMCOMMAND` (`0x001F`) packet. The client builds blocks of position entries in the format of:

```cpp
const auto vec4 = reinterpret_cast<D3DXVECTOR4*>(PTR_CollisionCache);
sprintf(buffer, "@%d,%d,%d,%d", vec4.x * 10000.0f, vec4.y * 10000.0f, vec4.z * 10000.0f, vec4.w * 10000.0f);
```

The client will do this formatting in a loop, walking the `PTR_CollisionCache` each step and appending the data together into a larger buffer. Once the buffer is 150 bytes long, the current block is sent to the server and the buffer is cleared and the next chunk will be walked and sent.

The client will keep sending this information until it reaches the current maximum cached collision entry. This is done in an async-like manner, without the client ever knowing it is happening in the background.
