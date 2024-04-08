# `GP_CLI_COMMAND_ITEM_TRADE_RES`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_ITEM_TRADE_RES` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0033` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the client when responding to a trade request.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_ITEM_TRADE_RES_KIND
enum class GP_ITEM_TRADE_RES_KIND : uint32_t
{
    GP_ITEM_TRADE_RES_KIND_START,
    GP_ITEM_TRADE_RES_KIND_CANCELL,
    GP_ITEM_TRADE_RES_KIND_MAKE,
    GP_ITEM_TRADE_RES_KIND_MAKECANCELL,
    GP_ITEM_TRADE_RES_KIND_ERR_ETC,
    GP_ITEM_TRADE_RES_KIND_ERR_NOSEARCHYOU,
    GP_ITEM_TRADE_RES_KIND_ERR_NOWREQ,
    GP_ITEM_TRADE_RES_KIND_ERR_YOUTRADE,
    GP_ITEM_TRADE_RES_KIND_ERR_LOGOUT,
    GP_ITEM_TRADE_RES_KIND_END
};

// PS2: GP_CLI_ITEM_TRADE_RES
struct GP_CLI_ITEM_TRADE_RES
{
    uint16_t                id: 9;
    uint16_t                size: 7;
    uint16_t                sync;
    GP_ITEM_TRADE_RES_KIND  Kind;           // PS2: Kind
    uint16_t                TradeCounter;   // PS2: TradeCounter
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_ITEM_TRADE_RES`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Kind`

_The response kind._

The client only makes use of values 0, 1, 2 and 3.

### `TradeCounter`

_The clients trade counter._
