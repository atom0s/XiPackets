# `GP_CLI_COMMAND_GROUP_KICK`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_GROUP_KICK` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0072` |
| **Size**                  | `0x001C` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

_The purpose of this packet is unknown. While this packet is defined from the PS2 beta and has a function that will send it, it is never called. It was also not called in the PS2 beta. It is assumed to work similar to the `0x0071` packet._

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_GROUP_KICK
struct GP_CLI_GROUP_KICK
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

## Structure Fields (`GP_CLI_GROUP_KICK`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The server id of the player being kicked._

This value is not set by the client.

### `ActIndex`

_The target index of the player being kicked._

This value is not set by the client.

### `Kind`

_The kind of kick being performed._

The purpose of this value is unknown.

### `padding0B`

_Padding; unused._

### `sName`

_The name of the player being kicked._
