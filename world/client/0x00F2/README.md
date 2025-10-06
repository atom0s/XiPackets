# `GP_CLI_COMMAND_SUBMAPCHANGE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_SUBMAPCHANGE` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00F2` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when updating their sub-map region.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_SUBMAPCHANGE
struct GP_CLI_SUBMAPCHANGE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    State;          // PS2: State
    uint16_t    SubMapNumber;   // PS2: SubMapNumber
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_SUBMAPCHANGE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `State`

_The packet state._

| State | Purpose |
| --- | --- |
| `0x01` | _General_ |
| `0x02` | _Event_ |

When the client is moving around the world freely, it will make use of `State` value `0x01` any time it enters a different sub-region within the map. The only time `State` value `0x02` is used is during events that cause the player to be moved between sub-regions.

### `SubMapNumber`

_The sub-map region number._

## Additional Information

This packet is sent by the client when it moves between different sub-regions within a map. Maps can have invisible sections inside of them that are marked as sub-regions that when a client intersects with the given region, it will cause it to send this packet, informing the server that the client has entered that sub-region. _(There is no updated event to tell the server when the client has exited the sub-region. It will only send another copy of this packet when it intersects with a different sub-region than the last one it was within.)_

Sub-regions are not found in all maps. Instead, they are generally only in areas where it is possible the server needs to know the client is within a 'special' region of the area for important kind of updates. Things such as event related updates, confirming positioning for things like airships/boats, etc. It is also important to note that sub-regions do not generally overlap other sub-regions. This means that when the client enters a sub-region, if they immediately walk backward after entering it, they wont generally walk back into another one right away.

An example of where sub-regions exist would be `Port Jeuno`. Each airship docks entrance and exit are separate sub-regions allowing the server to know where the client is currently positioned when trying to interact with certain entities or the airship itself.
