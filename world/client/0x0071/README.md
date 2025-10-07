# `GP_CLI_COMMAND_GROUP_STRIKE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_GROUP_STRIKE` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0071` |
| **Size**                  | `0x001C` |

## Description

This packet is sent by the client when kicking a member from a party, alliance or linkshell.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_GROUP_STRIKE
struct GP_CLI_GROUP_STRIKE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    UniqueNo;   // PS2: UniqueNo
    uint16_t    ActIndex;   // PS2: ActIndex
    uint8_t     Kind;       // PS2: Kind
    uint8_t     padding0B;  // PS2: dammy2
    uint8_t     sName[15];  // PS2: sName
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_GROUP_STRIKE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The server id of the player being kicked._

This value is always set to `0`.

### `ActIndex`

_The target index of the player being kicked._

This value is always set to `0`.

### `Kind`

_The kind of kick being performed._

| Kind | Purpose |
| `0` | _Kick From Party_ |
| `1` | _Kick From Linkshell (1)_ |
| `2` | _Kick From Linkshell (2)_ |
| `5` | _Kick From Alliance_ |

### `padding0B`

_Padding; unused._

### `sName`

_The name of the player being kicked._
