# `GP_SERV_COMMAND_MAP_GROUP`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_MAP_GROUP` |
| **Client Handler**        | `RecvMapGroup` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00A0` |
| **Size**                  | `0x0018` |

## Description

This packet is sent by the server to inform the client of its party members locations when viewing the map.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: _GP_MAP_GROUP
struct _GP_MAP_GROUP
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    UniqueID;
    int16_t     zone;
    uint16_t    padding00;
    float       x;
    float       y;
    float       z;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`_GP_MAP_GROUP`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `UniqueID`

_The server id of the party member._

### `zone`

_The zone id of the party member._

### `padding00`

_Padding; unused._

### `x`

_The x position of the party member._

### `y`

_The y position of the party member._

### `z`

_The z position of the party member._
