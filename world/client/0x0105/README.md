# `GP_CLI_COMMAND_BAZAAR_LIST`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_BAZAAR_LIST` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0105` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the client when requesting to view a bazaar.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_BAZAAR_LIST
struct GP_CLI_BAZAAR_LIST
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    UniqueNo;   // PS2: UniqueNo
    uint16_t    ActIndex;   // PS2: ActIndex
    uint16_t    padding0A;  // PS2: Dammy
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_BAZAAR_LIST`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The server id of the player whos bazaar is being requested._

### `ActIndex`

_The target index of the player whos bazaar is being requested._

### `padding0A`

_Padding; unused._
