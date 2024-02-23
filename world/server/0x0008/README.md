# `GP_SERV_COMMAND_ENTERZONE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_ENTERZONE` |
| **Client Handler**        | `RecvEnterZone` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0008` |
| **Size**                  | `0x0034` |

## Description

This packet is sent by the server to inform the client of the zones it has previously entered.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_ENTERZONE
struct GP_SERV_ENTERZONE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     EnterZoneTbl[48];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_ENTERZONE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `EnterZoneTbl`

_Array holding the previous entered zone information, in bits._

## Additional Information

The client makes use of this information to determine how to render lists the list of game zones when performing actions such as searching for other players, viewing maps, etc.

The initial block of data is directly copied from the packet into the main zone system as follows:

```cpp
qmemcpy(zone->EnterZoneTbl, pkt + 4, sizeof(zone->EnterZoneTbl));
```

The client makes use of the available bits within this block of data via the following:

```cpp
const auto has_visited = (PTR_pGlobalNowZone->EnterZoneTbl[zid >> 3] & (1 << (zid & 7))) != 0;
```
