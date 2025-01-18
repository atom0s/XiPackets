# `GP_SERV_COMMAND_BAZAAR_BUY`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_BAZAAR_BUY` |
| **Client Handlers**       | `RecvBuy`, `RecvBazarBuy` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0106` |
| **Size**                  | `0x001C` |

## Description

This packet is sent by the server when the client has made, or attempted to make, a purchase from another players Bazaar.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_BAZAAR_BUY_STATE
enum GP_BAZAAR_BUY_STATE
{
    GP_BAZAAR_BUY_STATE_OK,
    GP_BAZAAR_BUY_STATE_ERR,
    GP_BAZAAR_BUY_STATE_END
};

// PS2: GP_SERV_BAZAAR_BUY
struct GP_SERV_BAZAAR_BUY
{
    uint16_t            id: 9;
    uint16_t            size: 7;
    uint16_t            sync;
    GP_BAZAAR_BUY_STATE State;      // PS2: State
    uint8_t             sName[16];  // PS2: sName
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_BAZAAR_BUY`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `State`

_The state of the purchase._

| State | Name | Purpose |
| --- | --- | ---
| `0` | `GP_BAZAAR_BUY_STATE_OK`    | _The purchase was successful._ |
| `1` | `GP_BAZAAR_BUY_STATE_ERR`   | _There was an error attempting the purchase._ |

### `sName`

_The name of the player whos bazaar the item was purchased from._
