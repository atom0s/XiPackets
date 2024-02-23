# `GP_SERV_COMMAND_ASSIST`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_ASSIST` |
| **Client Handler**        | `RecvAssist` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0058` |
| **Size**                  | `0x0010` |

## Description

This packet is sent by the server to respond to a client assist request. _(`/assist`)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_ASSIST
struct GP_SERV_ASSIST
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    UniqueNo;
    uint32_t    AssistNo;
    uint16_t    ActIndex;
    uint16_t    padding00;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_ASSIST`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The local player server id._

### `AssistNo`

_The assist target server id._

### `ActIndex`

_The actor index._

The client does not use this value.

### `padding00`

_Padding; unused._
