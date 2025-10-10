# `GP_SERV_COMMAND_BAZAAR_CLOSE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_BAZAAR_CLOSE` |
| **Client Handlers**       | `RecvClose`, `RecvBazarClose` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0107` |
| **Size**                  | `0x0018` |

## Description

This packet is sent by the server when the selling players bazaar has closed while the client was viewing it. _(Either all items have been purchased, or the seller manually closed it._)

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_BAZAAR_CLOSE
struct GP_SERV_BAZAAR_CLOSE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     sName[16];      // PS2: sName
    uint8_t     padding14[4];   // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_BAZAAR_CLOSE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `sName`

_The name of the player whos bazaar was closed._

### `padding14`

_Padding; unused._
