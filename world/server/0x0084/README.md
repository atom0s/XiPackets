# `GP_SERV_COMMAND_GUILD_SELL`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_GUILD_SELL` |
| **Client Handlers**       | `gcRecvGuildSell`, `RecvGuildSell` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0084` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the server to inform the client of a completed sale to a guild vendor.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: _GP_GUILD_TRADE
struct _GP_GUILD_TRADE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    ItemNo; // PS2: ItemNo
    uint8_t     Count;  // PS2: Count
    int8_t      Trade;  // PS2: Trade
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`_GP_GUILD_TRADE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ItemNo`

_The item id being sold._

This value is set to 0 when the sale has failed. The `Trade` value is then used to determine why.

### `Count`

_The count of items the guild has available._

The client does not use this value in the main handler. The sub-handler for this packet makes use of the `Count` value to populate and update the guilds current available item count for the given item.

### `Trade`

_The amount of items sold._

## Additional Information

This packet is used in response to sales being made to a guild shop. The client makes use of the `ItemNo` and `Trade` values to determine the success of the sale and to determine what message will be printed based on the result. If the sale out-right fails, then the `ItemNo` value will be set to zero.

When `ItemNo` is zero, the client will print: `You were unable to carry out that transaction.`

When `ItemNo` is non-zero, then it holds the item id that was sold or attempted to be sold. The `Trade` value will be used to determine the amount if items that were successfully sold, if any. There are possible errors that can happen in this state based on what the `Trade` value is set to.

  - `0 (Zero)` - The client failed to sell any of the item. _(The shops stock for the item is full.)_
    - _The client will print: `The guild would not buy that from you. Its stock of that item is full.`_
  - `> 0 (Positive)` - The client sold all of the given item.
    - _The client will print: `You buy <num> <item> from the guild.`_
  - `< 0 (Negative)` - The client sold part of the given item.
    - _See notes below._

When the `ItemNo` value is non-zero and the `Trade` value is negative, it means that the client successfully sold only part of the given amount of items to the guild shop. This happens when the guilds stock for the item becomes full while handling the clients sale. When this happens, the `Trade` value will be negative. It is the number of items that the client successfully sold. The client will print two messages when this happens:

  - _The client will print: `You sell <num> <item> to the guild.`_
  - _The client will print: `The guild could not purchase the full amount. Its stock of that item became full.`_
