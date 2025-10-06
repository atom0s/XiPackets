# `GP_CLI_COMMAND_GROUP_SOLICIT_REQ`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_GROUP_SOLICIT_REQ` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x006E` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the client when sending party or alliance invites to other players.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_GROUP_SOLICIT_REQ
struct GP_CLI_GROUP_SOLICIT_REQ
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    UniqueNo;   // PS2: UniqueNo
    uint16_t    ActIndex;   // PS2: ActIndex
    uint8_t     Kind;       // PS2: Kind
    uint8_t     padding00;  // PS2: dammy2
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_GROUP_SOLICIT_REQ`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The server id of the player being invited._

### `ActIndex`

_The target index of the player being invited._

This value is only set if the target player is within the same zone as the local player.

### `Kind`

_The kind of party invite._

| Kind | Purpose |
| `0` | _Party Invite_ |
| `5` | _Alliance Invite_ |

### `padding00`

_Padding; unused._
