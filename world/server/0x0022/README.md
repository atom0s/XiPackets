# `GP_SERV_COMMAND_ITEM_TRADE_RES`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_ITEM_TRADE_RES` |
| **Client Handlers**       | `RecvItemTradeRes`, `CTkTrade::TkRecvItemTradeRes` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0022` |
| **Size**                  | `0x0010` |

## Description

This packet is sent by the server to inform the player of a trade result action.

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

// PS2: GP_SERV_ITEM_TRADE_RES
struct GP_SERV_ITEM_TRADE_RES
{
    uint16_t                    id: 9;
    uint16_t                    size: 7;
    uint16_t                    sync;
    uint32_t                    UniqueNo;   // PS2: UniqueNo
    GP_ITEM_TRADE_RES_KIND      Kind;       // PS2: Kind
    uint16_t                    ActIndex;   // PS2: ActIndex
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_ITEM_TRADE_RES`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The server id of the entity causing the trade action._

### `Kind`

_The kind of trade action being reported._

### `ActIndex`

_The target index of the entity causing the trade action._
