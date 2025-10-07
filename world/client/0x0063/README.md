# `GP_CLI_COMMAND_DIG`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_DIG` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0063` |
| **Size**                  | `0x0010` |

## Description

This packet is sent by the client when it has completed the chocobo digging animation; informing the server it is ready for the results.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_DIG
struct GP_CLI_DIG
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    UniqueNo;   // PS2: UniqueNo
    uint32_t    para;       // PS2: para
    uint16_t    ActIndex;   // PS2: ActIndex
    uint8_t     mode;       // PS2: mode
    uint8_t     padding0F;  // PS2: dammy
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_DIG`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The clients server id._

### `para`

_The packet parameter._

The client always sets this value to `0x00` for this packet.

### `ActIndex`

_The clients target index._

### `mode`

_The packet mode._

The client always sets this value to `0x11` for this packet. _(This value matches the beginning action id sent by the client when requesting to dig.)_

### `padding0F`

_Padding; unused._
