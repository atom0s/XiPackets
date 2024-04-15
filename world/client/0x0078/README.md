# `GP_CLI_COMMAND_GROUP_CHECKID`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_GROUP_CHECKID` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0078` |
| **Size**                  | `0x0004` |

## Description

This packet is sent by the client when requesting the clients server-side party id.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_GROUP_CHECKID
struct GP_CLI_GROUP_CHECKID
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_GROUP_CHECKID`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

## Additional Information

This packet is used by the client to request the actual group id that the client is part of. The client is generally not aware of the actual server-side group id that they are part of, as it is not needed for the client to function. The only time this id is actually needed is when the client is interacting with certain menus that display party member information. These menus make requests to the cache server which requires the group id as part of the information queries that the client will request.

When the client opens one of these menus, it will send this packet and set a callback to handle the returned group id value. That callback will then make a new request, this time to the cache server, that makes use of the returned group id along with any other conditional based information to form the query of information the client is requesting.
