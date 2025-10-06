# `GP_CLI_COMMAND_ITEM_TRADE_REQ`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_ITEM_TRADE_REQ` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0032` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the client when it is requesting to trade with another player.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_ITEM_TRADE_REQ
struct GP_CLI_ITEM_TRADE_REQ
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    UniqueNo;   // PS2: UniqueNo
    uint16_t    ActIndex;   // PS2: ActIndex
    uint16_t    padding00;  // PS2: dammy2
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_ITEM_TRADE_REQ`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The server id of the player to trade with._

### `ActIndex`

_The target index of the player to trade with._

### `padding00`

_Padding; unused._
