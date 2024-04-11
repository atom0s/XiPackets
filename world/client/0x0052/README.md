# `Packet: 0x0052`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0052` |
| **Size**                  | `0x004C` |

## Description

This packet is sent by the client when changing items in an equipset.

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
    uint8_t                 EquipKind;
    uint8_t                 padding00[3];
    equipsetrequestitem_t   ItemChange;
    equipsetrequestitem_t   Equipment[16];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `EquipKind`

_The equipment slot being changed._

### `padding00`

_Padding; unused._

### `ItemChange`

_The item change being applied to the slot._

### `Equipment`

_The additional items included already in the equipment set._

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

The client sends this packet to the server to validate changes being made to an equipment set. The server checks to ensure that the clients requested equipment set changes are valid, to try and prevent equipping invalid items in the wrong slot(s) or other attempted exploits.

When the client changes an item in an equipset, it will send this packet. The `ItemChange` field will hold the item information that the client has changed causing this packet to be sent. The `Equipment` array will be populated with the rest of the equipment set items that are already set and validated.

When an item has been changed, the client sets the `ItemChange` entry information to the desired item to be equipped. The `Equipment` entry for the slot the given item will be replacing is also set to all `0`.
