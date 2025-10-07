# `GP_CLI_COMMAND_AUC`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_AUC` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x004E` |
| **Size**                  | `0x003C` |

## Description

This packet is sent by the client when interacting with the auction house system.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_AUC_BOX
struct GP_AUC_BOX
{
    uint8_t     Stat;           // PS2: Stat
    uint8_t     padding01;      // PS2: pad01
    uint8_t     ItemIndex;      // PS2: ItemIndex
    uint8_t     padding03;      // PS2: pad03
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
    uint16_t    padding06;      // PS2: dummy
    uint32_t    ItemStacks;     // PS2: ItemStacks
};

// PS2: GP_AUC_PARAM_BID
struct GP_AUC_PARAM_BID
{
    uint32_t    BidPrice;       // PS2: BidPrice
    uint16_t    ItemNo;         // PS2: ItemNo
    uint16_t    padding06;      // PS2: dummy
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
        GP_AUC_PARAM_LOT        LotIn;      // PS2: LotIn
        GP_AUC_PARAM_BID        Bid;        // PS2: Bid
        GP_AUC_PARAM_SUMMARY    Summary;    // PS2: Summary
        GP_AUC_PARAM_HISTORY    History;    // PS2: History
        GP_AUC_PARAM_ASKCOMMIT  AskCommit;  // PS2: AskCommit
        GP_AUC_PARAM_TRANS      Trans;      // PS2: Trans
    } param;
};

// PS2: GP_CLI_AUC
struct GP_CLI_AUC
{
    uint16_t        id: 9;
    uint16_t        size: 7;
    uint16_t        sync;

    uint8_t         Command;        // PS2: Command
    int8_t          AucWorkIndex;   // PS2: AucWorkIndex
    int8_t          Result;         // PS2: Result
    int8_t          ResultStatus;   // PS2: ResultStatus
    GP_AUC_PARAM    Param;          // PS2: Param
    GP_AUC_BOX      Parcel;         // PS2: Parcel
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_AUC`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Command`

_The packet command id._

The retail client currently makes use of the following commands:

| Id | Command | Purpose |
| --- | --- | --- |
| `0x04` | `ASK_COMMIT` | _Used when placing an item up for sale; before confirmation._ |
| `0x05` | `INFO`       | _Used when opening the 'Sales Status' window._ |
| `0x0A` | `WORK_CHECK` | _Used when opening the auction house._ |
| `0x0B` | `LOT_IN`     | _Used when placing an item up for sale; after confirmation._ |
| `0x0C` | `LOT_CANCEL` | _Used when cancelling the sale of an item._ |
| `0x0D` | `LOT_CHECK`  | _Used when querying the status of items up for sale in the 'Sales Status' window._ |
| `0x0E` | `BID`        | _Used when bidding on an item._ |

### `AucWorkIndex`

_The auction work index._

This value is used as the index into the internal `GC_AUC_SYS->AucBoxCli` array which is the client side instance of the item being interacted with. _(Only used with specific `Command` values.)_

### `Result`

_The command result._

This value is always set to `0`.

### `ResultStatus`

_The command result status._

This value is always set to `0`.

### `Param`

_The command parameter._

This union type is populated based on the kind of `Command` that is being sent.

| Command | Union Type Used |
| --- | --- |
| `0x04` | `GP_AUC_PARAM_ASKCOMMIT` |
| `0x05` | _None. (No extra data is sent from the client.)_ |
| `0x0A` | _None. (No extra data is sent from the client.)_ |
| `0x0B` | `GP_AUC_PARAM_LOT` |
| `0x0C` | _None. (No extra data is sent from the client.)_ |
| `0x0D` | _None. (No extra data is sent from the client.)_ |
| `0x0E` | `GP_AUC_PARAM_BID` |

_**Note:** The other union types are used with the response from the server._

### `Parcel`

_The auction item information._

This value is not set or used by the client request packets. It is used with the server responses.

## Additional Information

This packet is used by the client when interacting with the auction house. For more information on how this packet and its response from the server works, please check the server response packet documentation here: [**Header**](/world/server/0x004C/README.md)

_The additional structures that are used by this packet are the same as the server response packet related to the auction house. Rather than reiterate the same information here, please make use of the server documentation page linked above for additional context and information related to how this packet and its response from the server work._

The client will populate some additional information for its requests based on the `Command` being used. Not all `Command` values make use of additional parameters and just expect response information from the server when sent. The client has a helper function it uses to prepare building an auction house request packet. This helper will prepare the various fields of the packet for their default usage, while the calling function to this helper can then populate the individual fields it makes use of. This helper function sets the following:

  - `Command` - _Set to the desired `Command` id._
  - `AucWorkIndex` - _Set to `0`._
  - `Result` - _Set to `0`._
  - `ResultStatus` - _Set to `0`._
  - `Param` - _Set to `0`. (Whole object is written to as `0`.)_
  - `Parcel` - _Set to `0`. (Whole object is memset to `0`.)_

## Additional Information - `Command` - `0x04` (`ASK_COMMIT`)

This `Command` treats the `Param` field as a `GP_AUC_PARAM_ASKCOMMIT` union type. The following fields will be populated:

  - `AskCommit.Commission` - _The amount of gil the player wishes to sell an item for._
  - `AskCommit.ItemWorkIndex` - _The index in the players inventory where the item is located._
  - `AskCommit.ItemNo` - _The item id._
  - `AskCommit.ItemStacks` - _The item stack flag. (`0` = Stack, `1` = No Stack)_

## Additional Information - `Command` - `0x05` (`INFO`)

_No additional data is populated for this `Command`._

## Additional Information - `Command` - `0x0A` (`WORK_CHECK`)

  - `AucWorkIndex` - Set to `-1`.

## Additional Information - `Command` - `0x0B` (`LOT_IN`)

  - `AucWorkIndex` - _Set to the clients next available auction house slot index._

This `Command` treats the `Param` field as a `GP_AUC_PARAM_LOT` union type. The following fields will be populated:

  - `LotIn.LimitPrice` - _The amount of gil the player wishes to sell an item for._
  - `LotIn.ItemWorkIndex` - _The index in the players inventory where the item is located._
  - `LotIn.ItemStacks` - _The item stack flag. (`0` = Stack, `1` = No Stack)_

## Additional Information - `Command` - `0x0C` (`LOT_CANCEL`)

  - `AucWorkIndex` - _Set to the clients auction house slot index of the item to cancel the sale of._

## Additional Information - `Command` - `0x0D` (`LOT_CHECK`)

  - `AucWorkIndex` - _Set to the clients auction house slot index of the item to check._

## Additional Information - `Command` - `0x0E` (`BID`)

  - `AucWorkIndex` - _Set to the clients next available auction house slot index._

This `Command` treats the `Param` field as a `GP_AUC_PARAM_BID` union type. The following fields will be populated:

  - `Bid.BidPrice` - _The price to bid on the desired item._
  - `Bid.ItemNo` - _The item id._
  - `Bid.ItemStacks` - _The item stack flag. (`0` = Stack, `1` = No Stack)_
