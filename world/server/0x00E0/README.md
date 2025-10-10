# `GP_SERV_COMMAND_GROUP_COMLINK`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_GROUP_COMLINK` |
| **Client Handler**        | `RecvComlink` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00E0` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the server to update the clients equipped linkshell information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_GROUP_COMLINK
struct GP_SERV_GROUP_COMLINK
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     LinkshellNum;   // PS2: (New; did not exist.)
    uint8_t     ItemIndex;      // PS2: (New; did not exist.)
    uint8_t     Category;       // PS2: (New; did not exist.)
    uint8_t     padding07;      // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_GROUP_COMLINK`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `LinkshellNum`

_The linkshell slot number._

  - `0` - _The packet will affect the main linkshell._
  - `1` - _The packet will affect the secondary linkshell._

### `ItemIndex`

_The linkshell item index._

### `Category`

_The container that the linkshell item is located in._

### `padding07`

_Padding; unused._
