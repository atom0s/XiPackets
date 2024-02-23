# `GP_SERV_COMMAND_AUC`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_AUC` |
| **Client Handler**        | `RecvAucCommon` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x004C` |
| **Size**                  | `0x003C` |

## Description

This packet is sent by the server when the client is interacting with the auction house.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_AUC_BOX
struct GP_AUC_BOX
{
    uint8_t     Stat;           // PS2: Stat
    uint8_t     padding00;      // PS2: padding00
    uint8_t     ItemIndex;      // PS2: ItemIndex
    uint8_t     padding01;      // PS2: padding01
    uint8_t     Name[16];       // PS2: Name
    uint16_t    ItemNo;         // PS2: ItemNo
    uint8_t     ItemQuantity;   // PS2: ItemQuantity
    uint8_t     ItemCategory;   // PS2: ItemCategory
    uint32_t    Price;          // PS2: Price
    uint32_t    MarketNo;       // PS2: MarketNo
    uint32_t    LotNo;          // PS2: LotNo
    uint32_t    TimeStamp;      // PS2: TimeStamp
};

// PS2: GP_AUC_PARAM_LOT
struct GP_AUC_PARAM_LOT
{
    uint32_t    LimitPrice;     // PS2: LimitPrice
    uint16_t    ItemWorkIndex;  // PS2: ItemWorkIndex
    uint16_t    padding00;      // PS2: padding00
    uint32_t    ItemStacks;     // PS2: ItemStacks
};

// PS2: GP_AUC_PARAM_BID
struct GP_AUC_PARAM_BID
{
    uint32_t    BidPrice;       // PS2: BidPrice
    uint16_t    ItemNo;         // PS2: ItemNo
    uint16_t    padding00;      // PS2: dummy
    uint32_t    ItemStacks;     // PS2: ItemStacks
};

// PS2: GP_AUC_PARAM_SUMMARY
struct GP_AUC_PARAM_SUMMARY
{
    uint32_t    Kind;           // PS2: Kind
    uint16_t    ItemNo;         // PS2: ItemNo
};

// PS2: GP_AUC_PARAM_HISTORY
struct GP_AUC_PARAM_HISTORY
{
    uint32_t    Range;          // PS2: Range
    uint16_t    ItemNo;         // PS2: ItemNo
};

// PS2: GP_AUC_PARAM_ASKCOMMIT
struct GP_AUC_PARAM_ASKCOMMIT
{
    uint32_t    Commission;     // PS2: Commission
    uint16_t    ItemWorkIndex;  // PS2: ItemWorkIndex
    uint16_t    ItemNo;         // PS2: ItemNo
    uint32_t    ItemStacks;     // PS2: ItemStacks
};

// PS2: GP_AUC_PARAM_TRANS
struct GP_AUC_PARAM_TRANS
{
    uint16_t    Signature;      // PS2: Signature
    uint16_t    TotalSize;      // PS2: TotalSize
    uint16_t    Offset;         // PS2: Offset
    uint16_t    Size;           // PS2: Size
    uint16_t    FragmentNo;     // PS2: FragmentNo
    uint16_t    TotalFragments; // PS2: TotalFragments
};

// PS2: GP_AUC_PARAM
struct GP_AUC_PARAM
{
    union
    {
        GP_AUC_PARAM_LOT        LotIn;      // LotIn
        GP_AUC_PARAM_BID        Bid;        // Bid
        GP_AUC_PARAM_SUMMARY    Summary;    // Summary
        GP_AUC_PARAM_HISTORY    History;    // History
        GP_AUC_PARAM_ASKCOMMIT  AskCommit;  // AskCommit
        GP_AUC_PARAM_TRANS      Trans;      // Trans
    } param;
};

// PS2: GP_SERV_AUC
struct GP_SERV_AUC
{
    uint16_t        id: 9;
    uint16_t        size: 7;
    uint16_t        sync;
    uint8_t         Command;        // PS2: Command
    char            AucWorkIndex;   // PS2: AucWorkIndex
    char            Result;         // PS2: Result
    char            ResultStatus;   // PS2: ResultStatus
    GP_AUC_PARAM    Param;          // PS2: Param
    GP_AUC_BOX      Parcel;         // PS2: Parcel
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_AUC`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Command`

_The packet command id._

This value represents the auction house action that is being performed by this packet. The following table shows the command names for the respective ids:

| Id | Command Name |
| --- | --- |
| `0x00` | `NONE` |
| `0x01` | `BARE_TRANSFER` |
| `0x02` | `MOPEN` |
| `0x03` | `MCLOSE` |
| `0x04` | `ASK_COMMIT` |
| `0x05` | `INFO` |
| `0x06` | `(6)` |
| `0x07` | `(7)` |
| `0x08` | `(8)` |
| `0x09` | `(9)` |
| `0x0A` | `WORK_CHECK` |
| `0x0B` | `LOT_IN` |
| `0x0C` | `LOT_CANCEL` |
| `0x0D` | `LOT_CHECK` |
| `0x0E` | `BID` |
| `0x0F` | `GET` |
| `0x10` | `CLEAR` |

### `AucWorkIndex`

_The auction work index._

This value is used as the index into the internal `GC_AUC_SYS->AucBoxCli` array which is the client side instance of the item being updated.

### `Result`

_The command result._

This value is used to inform the client if the previous action that caused this packet was successful or not. It is also used to tell the client different status values of the auction system.

| Value | State |
| --- | --- |
| `0xFF` | `ERR_ON_AUCTION` |
| `0xFE` | `ERR_IN_ITEMSYS` |
| `0xFD` | `ERR_BAD_SLOT_NO` |
| `0xFC` | `AUC_ERR_NOT_HAVE_GIL` |
| `0xFB` | `ERR_SLOT_NOT_SUITE` |
| `0xFA` | `ERR_ILLIGAL_ITEM_NO` |
| `0xF9` | `ERR_WRONG_PLACE` |
| `0xF8` | `ERR_REFUSED_TOO_MANY` |
| `0xF7` | `ERR_REFUSED_THISTIME` |
| `0xF6` | `ERR_LIMIT` |
| `0xF5` | `(unknown)`  |
| `0xF3` | `ERR_LAST_SLOT` |
| `0xF2` | `ERR_ITEMWORK_LOCKED` |
| `0xF1` | `ERR_NOT_ENOUGH_STACK` |
| `0xF0` | `ERR_NOT_FOR_POST` |
| `0xEF` | `ERR_NOT_FOR_AUCTION` |
| `0xEE` | `ERR_NO_ITEM_IN_WORK` |
| `0xE5` | `ERR_PROB_INVENTRY_FULL` |
| `0xE4` | `ERR_YOU_ALREADY_HAVE_LORE` |
| `0xDA` | `ERR_ALREADY_GONE` |
| `0xC5` | `ERR_CAN_NOT_BUY` |
| `---`  | `---`  |
| `0x00` | `FAIL` |
| `0x01` | `SUCCESS` |
| `0x02` | `INTERIM` |

_**Note:** Some `Command` values will alter how the `Result` value is treated for error handling. The `Result` value is also used to determine what error message is printed to the chatlog._

  - **Command: (All Commands)**
    - **Result:** `0xF5` - `(Auction house services are currently unavailable.)`
    - **Result:** `0xFF` - `Auction house is temporarily closed for trading.`
  - **Command:** `0x02`
    - **Result:** `!1 && !0xF6` - `Auction house is temporarily closed for trading.`
    - **Result:** `0xF6` - `Please try again in a little while.`
  - **Command:** `0x04`
    - **Result:** `0xF3` - `You can only place seven items on auction at once.`
  - **Command:** `0x05`
    - **Result:** `0xF6` - `Please try again in a little while.`
  - **Command:** `0x0B`
    - **Result:** `0x01` - `Merchanise placed on auction.`
    - **Result:** `!0x02` - `Failed to place merchandise on auction.`
  - **Command:** `0x0C`
    - **Result:** `0xE4` - `You cannot remove that item from auction.`
    - **Result:** `0xE5` - `You cannot remove that item from auction.`
  - **Command:** `0x0D`
    - **Result:** `0xF6` - _Causes a beep sound to be played._
  - **Command:** `0x0E`
    - **Result:** `0xE4` - `You cannot bid on that merchandise.`
    - **Result:** `0xE5` - `You cannot bid on that merchandise.`

### `ResultStatus`

_The command result status._

| Value | Meaning |
| --- | --- |
| `0x00` | `NOT_FOUND` |
| `0x01` | `IN_POOL` |
| `0x02` | `IN_OUTLOT` |
| `0x03` | `IN_TIMEOUT` |
| `0x04` | `IN_CANCEL` |
| `0x05` | `IN_SOLDOUT` |
| `0x06` | `IN_SENDBACK` |
| `0x07` | `IN_INCOME` |
| `0x08` | `WRONG_M` |
| `0x09` | `(unknown)` |

### `Param`

_The command parameter._

This value will vary based on the kind of packet being sent. _(This is a union type value.)_

### `Parcel`

_The auction item information._

## Structure Fields (`GP_AUC_PARAM`)

### `param`: `LotIn`, `Bid`, `Summary`, `History`, `AskCommit`, `Trans`

_The packet parameter data._

This field is a union type that is populated based on the packet `Command`.

## Structure Fields (`GP_AUC_BOX`)

### `Stat`

_The parcel status._

| Stat | Meaning |
| --- | --- |
| `0x00` | `NONE` |
| `0x01` | `LOT_SET` |
| `0x02` | `LOTIN_GOING` |
| `0x03` | `LOTIN_DONE` |
| `0x04` | `CANCEL_GOING` |
| `0x05` | `CANCEL_DONE` |
| `0x06` | `BID_SET` |
| `0x07` | `BID_GOING` |
| `0x08` | `BID_SUCCESS` |
| `0x09` | `BID_FAIL` |
| `0x0A` | `SOLDOUT` |
| `0x0B` | `SENDBACK` |
| `0x0C` | `SOLD_POST` |
| `0x0D` | `BACK_POST` |
| `0x0E` | `SENDBACK_PEND` |
| `0x0F` | `SOLDOUT_PEND` |
| `0x10` | `LOTIN_CHK` |
| `0x11` | `BACK_PEND_CHK` |
| `0x12` | `SOLD_PEND_CHK` |
| `0x13` | `BACK_POST_CHK` |
| `0x14` | `SOLD_POST_CHK` |
| `0x15` | `SOLD_POST_CHKED` |
| `0x16` | `BACK_POST_CHKED` |
| `0x17` | `unknown` |
| `0x18` | `NULL` |

### `padding00`

_Padding; unused._

### `ItemIndex`

_The parcel index._

### `padding01`

_Padding; unused._

### `Name`

_The parcel name._

### `ItemNo`

_The parcel item id._

### `ItemQuantity`

_The parcel quantity._

### `ItemCategory`

_The parcel item category._

### `Price`

_The parcel price._

### `MarketNo`

_The parcel market number._

### `LotNo`

_The parcel lot number._

### `TimeStamp`

_The parcel timestamp._
