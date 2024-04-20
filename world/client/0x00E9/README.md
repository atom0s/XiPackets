# `GP_CLI_COMMAND_GLOBALUNIQUENO_REQ`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_GLOBALUNIQUENO_REQ` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00E9` |
| **Size**                  | `0x000C` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

The purpose of this packet is unknown.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_GLOBALUNIQUENO_REQ
struct GP_CLI_GLOBALUNIQUENO_REQ
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    UniqueNo;   // PS2: UniqueNo
    uint16_t    ActIndex;   // PS2: ActIndex
    uint16_t    padding00;  // PS2: dammy
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_GLOBALUNIQUENO_REQ`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The entity server id._

### `ActIndex`

_The entity target index._

### `padding00`

_Padding; unused._

## Additional Information

This packet is not used by the client but the function to send it still exists. It is assumed, based on the packets command name, that this was likely used with a GM command.
