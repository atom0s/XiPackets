# `Unknown - Packet: 0x0116`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0116` |
| **Size**                  | `0x0048` |

## Description

This packet is sent by the server to confirm _(or deny)_ a clients equipment set item change.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct equipsetrequestitem_t
{
    uint8_t         HasItemFlg      : 1;
    uint8_t         RemoveItemFlg   : 1;
    uint8_t         Category        : 6;
    uint8_t         ItemIndex;
    uint16_t        ItemNo;
};

// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t                id: 9;
    uint16_t                size: 7;
    uint16_t                sync;
    equipsetrequestitem_t   Items[17];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Items`

_The array of items describing the equipment set._

This value holds the various equipment set slots information, including the keep/remove state flags, as well as the potential item being equipped into the slot.

The first entry into this array is the currently changed items response from the server. The rest of the 16 slots are aligned to the equipment slot array.

## Structure Fields (`equipsetrequestitem_t`)

### `HasItemFlg`

_Flag set if the slot has an item equipped into it._

### `RemoveItemFlg`

_Flag set if the slot should remove any equipped piece. (Shows red X.)_

This value is set if the slot was marked as 'Remove piece.'.

### `Category`

_The item container that holds the desired equipment item._

### `ItemIndex`

_The item index within the parent container._

### `ItemNo`

_The item id of the item that should be equipped._

## Additional Information

This packet is used to confirm that the equipment the client is attempting to put into a set is valid and can be properly equipped together. This validates that the client is not trying to do things that should otherwise not be possible, such as equipping a head item with a cloak, equipping a two-hand weapon with a shield, etc.

The client will send a request to the server with the item they are currently changing in a set, along with the rest of the current set items. The server will respond with the same equipment set information if all is ok. Otherwise, the server will remove the invalid item(s) from the respective slots if there were errors found within the equipment set.

If both `HasItemFlg` and `RemoveItemFlg` are not set, this is treated the same as if the client chose the 'Keep same piece.' option in the equipment set window.

_If an item in the equipment set is invalid, the server will change the given items information to all 0's._
