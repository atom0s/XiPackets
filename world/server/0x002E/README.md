# `GP_SERV_COMMAND_OPENMOGMENU`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_OPENMOGMENU` |
| **Client Handler**        | `RecvOpenMogMenu` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x002E` |
| **Size**                  | `0x0004` |

## Description

This packet is sent by the server to inform the client to open the mog house menu.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_OPENMOGMENU
struct GP_SERV_OPENMOGMENU
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_OPENMOGMENU`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

## Additional Information

This packet has no actual data, it simply informs the client that the mog house menu should be opened. This is done by setting a global variable in the client `OpenMogMenuFlag` which will then be checked on the next frame.
