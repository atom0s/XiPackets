# `GP_SERV_COMMAND_GRAP_LIST`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_GRAP_LIST` |
| **Client Handler**        | `RecvGrapList` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0051` |
| **Size**                  | `0x0018` |

## Description

This packet is sent by the server to update the clients entity model visual appearence.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_GRAP_LIST
struct GP_SERV_GRAP_LIST
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    GrapIDTbl[9];   // PS2: GrapIDTbl
    uint16_t    padding16;      // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_GRAP_LIST`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `GrapIDTbl`

_The clients equipment model visual ids._

  - `GrapIDTbl[0]` - _The clients race and hair ids._
  - `GrapIDTbl[1]` - _The clients head equipment visual model id._
  - `GrapIDTbl[2]` - _The clients body equipment visual model id._
  - `GrapIDTbl[3]` - _The clients hands equipment visual model id._
  - `GrapIDTbl[4]` - _The clients legs equipment visual model id._
  - `GrapIDTbl[5]` - _The clients feet equipment visual model id._
  - `GrapIDTbl[6]` - _The clients main equipment visual model id._
  - `GrapIDTbl[7]` - _The clients sub equipment visual model id._
  - `GrapIDTbl[8]` - _The clients ranged equipment visual model id._

### `padding16`

_Padding; unused._
