# `GP_SERV_COMMAND_GUILD_BUY`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_GUILD_BUY` |
| **Client Handler**        | `gcRecvGuildBuy` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0082` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the server to inform the client of a completed purchase from a guild vendor.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: _GP_GUILD_TRADE
struct _GP_GUILD_TRADE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    ItemNo;
    uint8_t     Count;
    int8_t      Trade;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`_GP_GUILD_TRADE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ItemNo`

_The item id being purchased._

This value is set to 0 when the purchase has failed. The `Trade` value is then used to determine why.

### `Count`

_The count of items the guild has available._

The client does not use this value in the main handler. The sub-handler for this packet makes use of the `Count` value to populate and update the guilds current available item count for the given item.

### `Trade`

_The amount of items purchased._

This value is also used to determine the error state of the purchase when it fails.

## Additional Information

This packet is used in response to purchases being made in a guild shop. The client makes use of the `ItemNo` and `Trade` values to determine the success of the purchase and to determine what message will be printed based on the result. If the purchase out-right fails, then the `ItemNo` value will be set to zero and the `Trade` value will then be used to determine the cause of the failure and what message to print to the client.

When `ItemNo` is zero, then the `Trade` value will be used as follows:

  - `-3` - The client attempted to purchase items too quickly.
    - _The client will print: `Please wait longer before making another purchase.`_
  - `-5` - The client attempted to purchase an item they can only hold one of.
    - _The client will print: `Transaction cancelled. You can only hold one item of that type.`_

All other failure values are treated the same and will print: `You were unable to carry out that transaction.`

When `ItemNo` is non-zero, then it holds the item id that was purchased or attempted to be purchased. The `Trade` value will be used to determine the amount if items that were successfully purchased, if any. There are possible errors that can happen in this state based on what the `Trade` value is set to.

  - `0 (Zero)` - The client failed to purchase any of the requested item. _(Their inventory is full or the guild ran out of stock.)_
    - _The client will print: `You cannot make that purchase. Either your inventory is full or the guild is out of stock.`_
  - `> 0 (Positive)` - The client purchased all of the requested item.
    - _The client will print: `You buy <num> <item> from the guild.`_
  - `< 0 (Negative)` - The client purchased part of the requested item.
    - _See notes below._

When the `ItemNo` value is non-zero and the `Trade` value is negative, it means that the client successfully purchased part of their requested amount of the item. This happens when the clients inventory becomes full making the purchase or guilds stock of an item was lowered below the clients requested amount before they attempted their purchase purchase but there was still some items remaining that could complete part of the purchase request. When this happens, the `Trade` value will be negative. It is the number of items that the client successfully purchased. The client will print two messages when this happens:

  - _The client will print: `You buy <num> <item> from the guild.`_
  - _The client will print: `Your order could not be filled completely. Either your inventory became full or the guild ran out of stock.`_

### Partial Purchase Example

Here is an example of when this partial purchase situation can happen:

  - `[Guild]` The guild shop opens and has `12 Galkan Sausage's` available for purchase.
  - `[Player1]` Opens the shop menu as soon as the guild opens. Sees `12 Galkan Sausage's` available for purchase.
  - `[Player2]` Opens the shop menu as soon as the guild opens. Sees `12 Galkan Sausage's` available for purchase.
  - `[Player1]` Purchases `8 Galkan Sausage's` as soon as possible.
  - `[Player2]` Attempts to now purchase `12 Galkan Sausage's` however, the available stock is now only `4`.
  - `[Player2]` Will successfully partial-purchase `4 Galkan Sausage's`.

In this example, the `Trade` value in `[Player2]`'s packet will be `-4` instead of `4`.
