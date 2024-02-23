# `GP_SERV_COMMAND_MAPSCHEDULOR`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_MAPSCHEDULOR` |
| **Client Handler**        | `RecvMapSchedulor` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0039` |
| **Size**                  | `0x0014` |

## Description

This packet is sent by the server to inform the client of a map scheduler animation/event that should play.

_This is used to play additional environment/map animations._

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_MAPSCHEDULOR
struct GP_SERV_MAPSCHEDULOR
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    UniqueNoCas;    // PS2: UniqueNoCas
    uint32_t    UniqueNoTar;    // PS2: UniqueNoTar
    uint32_t    id;             // PS2: id
    uint16_t    ActIndexCast;   // PS2: ActIndexCast
    uint16_t    ActIndexTar;    // PS2: ActIndexTar
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_MAPSCHEDULOR`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNoCas`

_The caster entity server id._

### `UniqueNoTar`

_The target entity server id._

### `id`

_The 'FourCC' code tag for the scheduler to load._

### `ActIndexCast`

_The caster entity target index._

### `ActIndexTar`

_The target entity target index._
