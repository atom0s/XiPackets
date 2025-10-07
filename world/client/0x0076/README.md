# `GP_CLI_COMMAND_GROUP_LIST_REQ`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_GROUP_LIST_REQ` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0076` |
| **Size**                  | `0x0006` |

## Description

This packet is sent by the client when it has attempted to access invalid party member information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_GROUP_LIST_REQ
struct GP_CLI_GROUP_LIST_REQ
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     Kind; // PS2: Kind
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_GROUP_LIST_REQ`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Kind`

_The packet kind._

## Additional Information

This packet is sent when the client has attempted to make use of an invalid / missing party members information. The client has an iterator function to walk the party member list _(`FUNC_gcGroupLookNext`)_ to return the next expected member in the party _(with sorting)_ that is valid. At the end of this function, it checks if the members name is valid/set. If it is not set, then this packet will be sent to request that the server send the client an updated snapshot of the current party information.

This is the only usage of this packet, and the `Kind` value will always be set to `0`.
