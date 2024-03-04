# `GP_SERV_COMMAND_GROUP_CHECKID`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_GROUP_CHECKID` |
| **Client Handler**        | `RecvCheckID` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00E1` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the server to inform the client of their parties `GroupID`.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_GROUP_CHECKID
struct GP_SERV_GROUP_CHECKID
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    GroupID;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_GROUP_CHECKID`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `GroupID`

_The party group id._

## Additional Information

This packet is used when the client will be requesting additional information from the server about their party. Things like requesting the party player list when selecting a level sync. The server will send this packet to tell the client its party group id, which the client will then use to send the needed requests to the cache server which handles searches.
