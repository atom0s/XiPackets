# `GP_CLI_COMMAND_MYROOM_PLACE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_MYROOM_PLACE` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00D1` |
| **Size**                  | `0x000C` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

The purpose of this packet is unknown. _(It appears to be the old furniture placement request packet or a GM command based packet.)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: _GP_MYROOM_PLACE_REQ
struct _GP_MYROOM_PLACE_REQ
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    ItemNo;     // PS2: ItemNo
    int8_t      X;          // PS2: X
    int8_t      Y;          // PS2: Y
    int8_t      Z;          // PS2: Z
    int8_t      V;          // PS2: V
    uint8_t     TableNo;    // PS2: TableNo
    uint8_t     padding0B;  // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`_GP_MYROOM_PLACE_REQ`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ItemNo`

_Unknown._

### `X`

_Unknown._

### `Y`

_Unknown._

### `Z`

_Unknown._

### `V`

_Unknown._

### `TableNo`

_Unknown._

### `padding0B`

_Unknown._
