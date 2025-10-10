# `GP_SERV_COMMAND_TROPHY_LIST`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_TROPHY_LIST` |
| **Client Handler**        | `RecvTrophyList` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00D2` |
| **Size**                  | `0x003C` |

## Description

This packet is sent by the server when an item has been found and would be added to the treasure pool or be split amongst the party. _(This is used for items and gold found when defeating an enemy, opening a chest, identifying group based items, etc. Anything that would cause an item to be placed into the treasure pool or be split amongst the party will use this packet.)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_TROPHY_LIST
struct GP_SERV_TROPHY_LIST
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    TrophyItemNum;      // PS2: TrophyItemNum
    uint32_t    TargetUniqueNo;     // PS2: TargetUniqueNo
    uint16_t    Gold;               // PS2: Gold
    uint16_t    padding0E;          // PS2: (New; was Exp originally.)
    uint16_t    TrophyItemNo;       // PS2: TrophyItemNo
    uint16_t    TargetActIndex;     // PS2: TargetActIndex
    uint8_t     TrophyItemIndex;    // PS2: TrophyItemIndex
    uint8_t     Entry;              // PS2: Entry
    uint8_t     IsContainer;        // PS2: (New; did not exist.)
    uint8_t     padding17;          // PS2: (New; did not exist.)
    uint32_t    StartTime;          // PS2: StartTime
    uint16_t    IsLocallyLotted;    // PS2: (New; did not exist.)
    uint16_t    Point;              // PS2: (New; did not exist.)
    uint32_t    LootUniqueNo;       // PS2: (New; did not exist.)
    uint16_t    LootActIndex;       // PS2: (New; did not exist.)
    uint16_t    LootPoint;          // PS2: (New; did not exist.)
    uint8_t     LootActName[16];    // PS2: (New; did not exist.)
    uint8_t     NamedFlag   : 1;    // PS2: (New; did not exist.)
    uint8_t     SingleFlag  : 1;    // PS2: (New; did not exist.)
    uint8_t     Flags_2     : 2;    // PS2: (New; did not exist.)
    uint8_t     Flags_4     : 1;    // PS2: (New; did not exist.)
    uint8_t     Flags_5     : 1;    // PS2: (New; did not exist.)
    uint8_t     Flags_6     : 1;    // PS2: (New; did not exist.)
    uint8_t     Flags_7     : 1;    // PS2: (New; did not exist.)
    uint8_t     padding39[3];       // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_TROPHY_LIST`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `TrophyItemNum`

_The number of `TrophyItemNo` that dropped._

This value is the item count of the item being dropped with this packet. _(`TrophyItemNo`)_ The client does not have proper handling for this value to be anything other than 1. If multiple items were to drop in the same slot _(ie. 3 fire crystals instead of 1)_, the client will still only print the message `You find a fire crystal on <name>.`. _(This shows that it is unlikely there is any drop in the game that will be multiple in a single slot of the treasure pool.)_

_**Note:** While the message printed does not show the proper item count, the treasure pool itself will._

### `TargetUniqueNo`

_The server id of the entity (or object) that dropped (or contained) the item and/or gold._

### `Gold`

_The amount of gold that was found._

This value is set to the amount of gil that will be obtained by the player. _(See notes below for more info.)_

### `padding0E`

_Padding; unused._

### `TrophyItemNo`

_The item id._

This value is set to the item id of the found item, if one was found. _(This can be 0 if the packet is only showing found gold.)_

### `TargetActIndex`

_The server id of the entity (or object) that dropped (or contained) the item and/or gold._

### `TrophyItemIndex`

_The treasure pool index that this item is being placed into._

### `Entry`

_The items treasure pool entry flag for the local player._

  - `0` - `GC_ITEM_TROPHY_ENTRY_KIND_NONE` - _The local client has not passed/lotted on the item yet._
  - `1` - `GC_ITEM_TROPHY_ENTRY_KIND_NO` - _The local client has passed on the item._
  - `2` - `GC_ITEM_TROPHY_ENTRY_KIND_YES` - _The local client has lotted on the item._

### `IsContainer`

_Flag set if the item (and/or gold) was found within a container._

This value is set when the item (and/or gold) gold that was found was inside of a container, such as a chest. This value is used by the client to change the message id used to print the `You found..` message. When set, this will cause the message to print as:

  - `0` - `You find [a] <item name> on [the] <name>.`
  - `1` - `You find [a] <item name> in the <name>.`

### `padding17`

_Padding; unused._

### `StartTime`

_The timestamp of when the item was found and added to the treasure pool._

### `IsLocallyLotted`

_Flag set if the local client has lotted on the item._

This value is used to determine if the client will make use of the `Point` value. If this value is 0, then the `Point` value is treated as 0 as well.

### `Point`

_The local players lot for the item._

This value is only used when `IsLocallyLotted` is set to 1. It holds the local players lot for the item. This will cause the item in the loot box to show as orange/red to the local client properly when first entering the pool.

### `LootUniqueNo`

_The current winning lotters server id._

This value is set to the current winning lotters server id.

### `LootActIndex`

_The current winning lotters target index._

This value is set to the current winning lotters target index.

### `LootPoint`

_The current winning lotters lot value._

### `LootActName`

_The current winning lotters name._

The client will use this value to display in the treasure box (when using the extended treasure box mode) instead of the actual entity name pulled from its entity object.

### `NamedFlag`

_Flag set if the entity (or object) that dropped the item is named._

When this flag is set, it will cause any mentions of the entity (or object) that dropped the item to have their name to no longer be prefixed with `The`. Most notorious monsters will have this set.

### `SingleFlag`

_Flag set if the entity (or object) should be referred to plurally._

When this flag is set, it will cause any references to the entity (or object) that dropped the item to be considered plural. This changes wording such as `seem` vs. `seems`.

### `Flags_2`, `Flags_4`, `Flags_5`, `Flags_6`, `Flags_7`

_Unknown._

These flag values are used by the client when handling different verbiage for certain languages. This is mainly used for the discontinued French and German languages. _(Due to the European languages being discontinued, testing for what this values all are is not worth spending time on currently.)_

### `padding39`

_Padding; unused._

## Additional Information

This packet is used to handle treasure _(referred to as Trophies internally to the client)_ found throughout the game. Any item or gold that would otherwise be added to the treasure pool or split between the party will make use of this packet. Even if the local client is playing solo and items drop immediately to you, it still technically went to the treasure pool first and was then distributed to you as the only valid person to obtain said treasure. Gold is treated the same with this packet anytime it would be split between the party, such as from defeating Beastman, certain HNMs, opening chests, identifying items that are added to the treasure pool, etc.

The server will send a separate packet for each item that is discovered and to be added to the pool. Gold is treated in a special manner depending on what else was found at the same time.

If only gold is obtained through defeating an enemy, opening a chest, etc. that would otherwise be split between the party _(even if solo)_, then this packet is used. The server will populate the `Gold` value with the amount of gold the local player will obtain from the split, if in a party, or the full amount obtained if solo. When this happens, only the following fields in the packet are populated:

  - `TrophyItemNum` _(set to 1)_
  - `TargetUniqueNo` _(if valid)_
  - `Gold`
  - `TargetActIndex` _(if valid)_
  - `StartTime`

The rest of the packet will be either set to `0` or contain junk data from non-clearing buffer reuse.

If the player obtains gold and items from the same source, then the server will treat the gold in a special manner. In the event that gold and items are obtained at the same time _(from the same source)_, then the gold is added to the first items packet instead of sending a separate packet just for the gold. For example, if the player defeats a Beastman with Signet on, and the Beastman drops 1 fire crystal and 25 gold, then the server will send a single instance of this packet. The `Gold` value will be set to 25 _(or the players portion of the split if in a party)_ and the other item fields will be set to the fire crystal information. If the enemy drops multiple items and gold, then the same rule will apply for the gold and first item, with the rest of the other items each having their own packet with `Gold` set to 0.

If a player, within a party, enters into a zone that has treasure in the pool already, they will be sent a packet of this type per-item that exists in the treasure pool currently. The packet will populate the highest winning lotter information in the `LootUniqueNo`, `LootActIndex`, `LootActName` and `LootPoint` values. The server uses a separate copy of the current highest lotting value for an item separate from the actual used information when deciding who gets items when their time expires. This is done so that the server can show potential edge cases.

Take the following for example:

  - A group of 3 players form a party and defeat Fafnir. _(`PlayerA`, `PlayerB`, `PlayerC`)_
  - Fafnir drops a Ridill to the treasure pool.
  - `PlayerA` rolls a 900 on the item. _(The other two members do not roll or pass.)_
  - `PlayerA` leaves the zone.
  - `PlayerA` reenters the zone and sees the Ridill in the treasure pool again.

During this case, `PlayerA` will be sent a copy of this packet with the Ridill being in the treasure pool. The packets highest lotter information will still populate with `PlayerA`'s original 900 lot. However, during this case, `PlayerA` technically no longer has a roll on the item. This means that `PlayerA` is free to lot or pass on the item again.

_While the needed information exists in this packet to prevent this issue from happening, SE does not seem to enforce it and allows players to try and reroll on items. A players lot/pass status on an item is only valid as long as they remain in the zone or the party. If they leave the zone or drop party, then their previous status for an item is removed. If they then reenter the zone or rejoin the party, they are free to take action on the item again._
