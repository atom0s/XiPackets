# `GP_SERV_COMMAND_TROPHY_SOLUTION`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_TROPHY_SOLUTION` |
| **Client Handler**        | `RecvTrophySolution` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00D3` |
| **Size**                  | `0x003C` |

## Description

This packet is sent by the server when an item in the treasure pool has had an action taken against it. _(Lot, Pass, Distribution)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_TROPHY_SOLUTION
struct GP_SERV_TROPHY_SOLUTION
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    LootUniqueNo;           // PS2: LootUniqueNo
    uint32_t    EntryUniqueNo;          // PS2: EntryUniqueNo
    uint16_t    LootActIndex;           // PS2: LootActIndex
    int16_t     LootPoint;              // PS2: LootPoint
    uint16_t    EntryActIndex   : 15;   // PS2: EntryActIndex
    uint16_t    EntryFlg        : 1;    // PS2: EntryFlg
    int16_t     EntryPoint;             // PS2: EntryPoint
    uint8_t     TrophyItemIndex;        // PS2: TrophyItemIndex
    uint8_t     JudgeFlg;               // PS2: JudgeFlg
    uint8_t     sLootName[16];          // PS2: sLootName
    uint8_t     sLootName2[16];         // PS2: (New; did not exist.)
    uint8_t     padding36[6];           // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_TROPHY_SOLUTION`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `LootUniqueNo`

_The current winning lotters server id._

### `EntryUniqueNo`

_The casting players server id._

### `LootActIndex`

_The current winning lotters target index._

### `LootPoint`

_The current winning lotters lot value._

### `EntryActIndex`

_The casting players target index._

### `EntryFlg`

_Flag that states if the casting player lotted or passed on the item._

The client will check this bit to determine the entry kind based on the following enum:

  - `0` - `GC_ITEM_TROPHY_ENTRY_KIND_NONE` - _None. (This should not happen with this packet.)_
  - `1` - `GC_ITEM_TROPHY_ENTRY_KIND_NO` - _The player passed on the item. (Used if `EntryFlg` is 0.)_
  - `2` - `GC_ITEM_TROPHY_ENTRY_KIND_YES` - _The player lotted on the item. (Used if `EntryFlg` is 1.)_

### `EntryPoint`

_The casting players lot roll for the item._

### `TrophyItemIndex`

_The treasure pool item index._

### `JudgeFlg`

_Flag that states the packet is used for the final judgement of the item._

See the notes below for more information on how this value works.

### `sLootName`

_The current winning lotters name._

### `sLootName2`

_The casting players name._

### `padding36`

_Padding; unused._

## Additional Information

Each time a party member rolls for or passes an item, the server will send all other valid party members this packet to inform them of the action taken on the item in the treasure pool. The server will populate the current winning lot information based on whoever has rolled the highest for the item. The 'casting player' mentioned in the field information above is the player who caused this packet to be sent when a new lot/pass has been made. This information is used to tell the client how to properly update the local treasure pool information to be displayed both in the treasure pool _(when using hte expanded pool mode)_ as well as on the side of the party list.

The client has a special check for when the `EntryActIndex` is the local players target index, which will populate the local players lot information differently.

The client will then check and use the `JudgeFlg` to determine if a judgement has been made on the item. If no judgement has been made _(`JudgeFlg` is 0)_ then the client checks to see if `EntryPoint` and `sLootName2` are set. If they are, then the client will print the casting players lot roll to chat. _(Passes are not announced.)_ If the `JudgeFlg` is set _(> 0)_, then the client will use its value to determine what message is printed based on the judgement result.

_**Note:** While the true size of `sLootName` and `sLootName2` are both 16 bytes, the way the client is coded introduces a common C/C++ memory bug called a buffer overrun due to the manner in which the strings are being copied. The client will instead attempt to read 24 bytes for each name. When `sLootName` is read, the read will overrun into `sLootName2`. The same happens with `sLootName2` reading into `padding36`._

## Additional Information - `JudgeFlg`

The `JudgeFlg` value is used as a mode value to determine the final 'judgement' of a treasure pool item. Some of these modes have additional sub-conditions that will alter how the client will treat the judgement and what message it will print about the item.

### `JudgeFlg` - Mode: 0

This is the default mode used when normal lotting and passing is taking place on treasure pool items.

When this mode is used, the client will check and ensure that the `EntryPoint` is greater than 0 and that the `sLootName2` name is set. If these are both true, then the client will print the message stating that a player has lotted on an item and what their roll was.

```
(sLootName2)'s lot for the ItemNameHere: (EntryPoint) points.
```

### `JudgeFlg` - Mode: 1

This mode is used when an item has been successfully won _(or randomly distributed)_.

When this mode is used, the client will perform two checks to allow up to two messages to be printed. The first check is to see if the last packet contains a lot attempt for the item. This will check if the `EntryPoint` is greater than 0 and that the `sLootName2` name is set. If these are both true, the client will then treat the `EntryUniqueNo` value differently. It will instead hold a message id offset that the client then uses to determine which message is printed, stating who tried to do the final lot on the item. The `EntryUniqueNo` is treated as follows in this condition:

```
// EntryUniqueNo == 0
You cast lots for the ItemNameHere.

// EntryUniqueNo >= 1
(sLootName2)'s lot for the ItemNameHere: (EntryPoint) points.
```

Next, the client will then print the message of who won the item. The client will treat the `LootUniqueNo` value differently during this message. It will instead hold a message id offset that the client then uses to determine which message is printed based on who won the item. This is treated as follows:

```
// LootUniqueNo == 0
You obtain a ItemNameHere.

// LootUniqueNo >= 1
(sLootName) obtains a ItemNameHere.
```

### `JudgeFlg` - Mode: 2

This mode is used when an item has been won, but the winner does not meet the requirements to obtain the item and it is lost. This is usually the result of the winners inventory being full or the winner has already obtained the given item and cannot obtain another, such as a rare item.

When this mode is used, the client will treat the `LootUniqueNo` value differently. It uses the `LootUniqueNo` value as a boolean to determine which message is printed. This is treated as follows:

```
// LootUniqueNo == 0
You to not meet the requirements to obtain the ItemNameHere.
ItemNameHere lost.

// LootUniqueNo >= 1
(sLootName) does not meet the necessary requirements to obtain the ItemNameHere.
ItemNameHere lost.
```

### `JudgeFlg` - Mode: 3

This mode is used when an item is lost but no message will be printed. The client will silently clear _(memset to 0)_ the treasure pool entry for the item.

_**Note:** The client treats all `JudgeFlg` values that are >= 3 the same. It will silently clear the treasure pool item at the given `TrophyItemIndex` without printing any messages._
