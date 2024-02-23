# `GP_SERV_COMMAND_SERVERSTATUS`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_SERVERSTATUS` |
| **Client Handler**        | `RecvServerStatus` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0037` |
| **Size**                  | `0x0060` |

## Description

This packet is sent by the server to update the local players information and status.

## Packet Layout

The layout of this packet is the following:

```cpp
struct flags0_t
{
    uint32_t    HideFlag        : 1;
    uint32_t    SleepFlag       : 1;
    uint32_t    GroundFlag      : 1;
    uint32_t    CliPosInitFlag  : 1;
    uint32_t    LfgFlag         : 1;
    uint32_t    AnonymousFlag   : 1;
    uint32_t    CfhFlag         : 1;
    uint32_t    AwayFlag        : 1;
    uint32_t    Gender          : 1;
    uint32_t    unknown_1_9     : 1;
    uint32_t    unknown_1_10    : 1;
    uint32_t    GraphSize       : 2;
    uint32_t    Chocobo_Index   : 3;
    uint32_t    hpp             : 8;
    uint32_t    PlayOnelineFlag : 1;
    uint32_t    LinkShellFlag   : 1;
    uint32_t    LinkDeadFlag    : 1;
    uint32_t    TargetOffFlag   : 1;
    uint32_t    unknown_3_28    : 1;
    uint32_t    GmLevel         : 3;
};

struct flags1_t
{
    uint32_t    Speed           : 12;
    uint32_t    Hackmove        : 1;
    uint32_t    FreezeFlag      : 1;
    uint32_t    unknown_1_14    : 1;
    uint32_t    InvisFlag       : 1;
    uint32_t    unknown_2_16    : 1;
    uint32_t    SpeedBase       : 8;
    uint32_t    unknown_3_25    : 4;
    uint32_t    BazaarFlag      : 1;
    uint32_t    CharmFlag       : 1;
    uint32_t    GmIconFlag      : 1;
};

struct flags2_t
{
    uint32_t    NamedFlag       : 1;
    uint32_t    SingleFlag      : 1;
    uint32_t    AutoPartyFlag   : 1;
    uint32_t    PetIndex        : 16;
    uint32_t    MotStopFlag     : 1;
    uint32_t    CliPriorityFlag : 1;
    uint32_t    unknown_2_21    : 8;
    uint32_t    unknown_3_29    : 3;
};

struct flags3_t
{
    uint32_t    LfgMasterFlag   : 1;
    uint32_t    TrialFlag       : 1;
    uint32_t    unknown_0_2     : 1;
    uint32_t    NewCharacterFlag: 1;
    uint32_t    MentorFlag      : 1;
    uint32_t    unknown_0_5     : 1;
    uint32_t    unknown_0_6     : 1;
    uint32_t    unknown_0_7     : 1;
    uint32_t    unknown_1_8     : 8;
    uint32_t    unknown_2_16    : 16;
};

struct flags4_t
{
    uint8_t     GeoIndiElement  : 4;
    uint8_t     GeoIndiSize     : 2;
    uint8_t     GeoIndiFlag     : 1;
    uint8_t     JobMasterFlag   : 1;
};

struct flags5_t
{
    uint8_t     unknown_0_0     : 2;
    uint8_t     unknown_0_2     : 2;
    uint8_t     unknown_0_4     : 4;
};

struct flags6_t
{
    uint32_t    unknown_0_0     : 1;
    uint32_t    unknown_0_1     : 1;
    uint32_t    unknown_0_2     : 1;
    uint32_t    unknown_0_3     : 1;
    uint32_t    unknown_0_4     : 1;
    uint32_t    unknown_0_5     : 1;
    uint32_t    unknown_0_6     : 1;
    uint32_t    unknown_0_7     : 25;
};

struct status_bits_t
{
    uint8_t     Status1         : 2;
    uint8_t     Status2         : 2;
    uint8_t     Status3         : 2;
    uint8_t     Status4         : 2;
    uint8_t     Status5         : 2;
    uint8_t     Status6         : 2;
    uint8_t     Status7         : 2;
    uint8_t     Status8         : 2;
    uint8_t     Status9         : 2;
    uint8_t     Status10        : 2;
    uint8_t     Status11        : 2;
    uint8_t     Status12        : 2;
    uint8_t     Status13        : 2;
    uint8_t     Status14        : 2;
    uint8_t     Status15        : 2;
    uint8_t     Status16        : 2;
    uint8_t     Status17        : 2;
    uint8_t     Status18        : 2;
    uint8_t     Status19        : 2;
    uint8_t     Status20        : 2;
    uint8_t     Status21        : 2;
    uint8_t     Status22        : 2;
    uint8_t     Status23        : 2;
    uint8_t     Status24        : 2;
    uint8_t     Status25        : 2;
    uint8_t     Status26        : 2;
    uint8_t     Status27        : 2;
    uint8_t     Status28        : 2;
    uint8_t     Status29        : 2;
    uint8_t     Status30        : 2;
    uint8_t     Status31        : 2;
    uint8_t     Status32        : 2;
};

// PS2: GP_SERV_SERVERSTATUS
struct GP_SERV_SERVERSTATUS
{
    uint16_t        id: 9;
    uint16_t        size: 7;
    uint16_t        sync;
    uint8_t         BufStatus[32];          // PS2: BufStatus
    uint32_t        UniqueNo;               // PS2: UniqueNo
    flags0_t        Flags0;                 // PS2: <bits> (Nameless bitfield.)
    flags1_t        Flags1;                 // PS2: <bits> (Nameless bitfield.)
    uint8_t         server_status;          // PS2: server_status
    uint8_t         r;                      // PS2: r
    uint8_t         g;                      // PS2: g
    uint8_t         b;                      // PS2: b
    flags2_t        Flags2;                 // PS2: <bits> (Nameless bitfield.)
    flags3_t        Flags3;                 // PS2: <bits> (New; did not exist.)
    uint32_t        dead_counter1;          // PS2: (New; did not exist.)
    uint32_t        dead_counter2;          // PS2: (New; did not exist.)
    uint16_t        costume_id;             // PS2: (New; did not exist.)
    uint16_t        warp_target_index;      // PS2: (New; did not exist.)
    uint16_t        fellow_target_index;    // PS2: (New; did not exist.)
    uint8_t         fishing_timer;          // PS2: (New; did not exist.)
    uint8_t         padding00;              // PS2: (New; did not exist.)
    status_bits_t   BufStatusBits;          // PS2: (New; did not exist.)
    uint16_t        monstrosity_info;       // PS2: (New; did not exist.)
    uint8_t         monstrosity_name_id1;   // PS2: (New; did not exist.)
    uint8_t         monstrosity_name_id2;   // PS2: (New; did not exist.)
    flags4_t        Flags4;                 // PS2: (New; did not exist.)
    uint8_t         model_hitbox_size;      // PS2: (New; did not exist.)
    flags5_t        Flags5;                 // PS2: (New; did not exist.)
    uint8_t         mount_id;               // PS2: (New; did not exist.)
    flags6_t        Flags6;                 // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_SERVERSTATUS`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `BufStatus`

_The entities status effects._

### `UniqueNo`

_The entities server id._

### `Flags0`

_Misc flags. (See full breakdown below.)_

### `Flags1`

_Misc flags. (See full breakdown below.)_

### `server_status`

_The entities server status._

### `r`

_The entities linkshell color. (red)_

### `g`

_The entities linkshell color. (green)_

### `b`

_The entities linkshell color. (blue)_

### `Flags2`

_Misc flags. (See full breakdown below.)_

### `Flags3`

_Misc flags. (See full breakdown below.)_

### `dead_counter1`

_Timer value used when the entity is dead and waiting to homepoint._

### `dead_counter2`

_Timer value used when the entity is dead and waiting to homepoint._

### `costume_id`

_The entities costume id._

### `warp_target_index`

_The entities warp target index._

### `fellow_target_index`

_The entities fellow target index._

### `fishing_timer`

_The entities fishing timer._

### `padding00`

_Padding; unused._

### `BufStatusBits`

_The entities status effects. (High-bits used to calculate the full status effect id.)_

### `monstrosity_info`

_The entities monstrosity information._

### `monstrosity_name_id1`

_The entities monstrosity name id. (1)_

### `monstrosity_name_id2`

_The entities monstrosity name id. (2)_

### `Flags4`

_Misc flags. (See full breakdown below.)_

### `model_hitbox_size`

_The entities model hitbox size._

### `Flags5`

_Misc flags. (See full breakdown below.)_

### `mount_id`

_The entities mount id._

### `Flags6`

_Misc flags. (See full breakdown below.)_

## Status Effects (`BufStatus` & `BufStatusBits`)

The `BufStatus` and `BufStatusBits` values are both used to calculate the full proper status effect ids that are currently affecting the client. Originally, the game only required `BufStatus` to hold these values, but as the game has developed over time, the id of status effects has grown beyond the `255` value limit that a `uint8_t` can hold. Due to this, a change was required to be able to support these higher value ids.

Based on how packet changes happen over time, it is assumed that SE generally chooses to add onto packets vs. editing any of their base fields to potentially be backward-compatible with older clients _(allows for easy-rollbacks if needed)_ unless the packet (or the system it relates to) is being fully overhauled. Due to this, the manner in which they have added support for higher value status ids is by adding a new `uint64_t` value that holds the high-value masking bits for each status effect. The `BufStatusBits` value is used for this purpose, in which each of the 32 status effects has 2 bits in this value dedicated to it to extend its value beyond `255`.

The client recalculates the proper status effect ids in the following manner:

```cpp
for (auto x = 0; x < 32; ++x)
{
    if (pkt->BufStatus[x] != 0xFF || (((uint8_t*)&pkt->BufStatusBits + (x >> 0x02)) >> (0x02 * (x % 0x04)) & 0x03) != 0)
    {
        // Recalculate the full status effect id..
        PTR_status_data.Buffs[x] = pkt->BufStatus[x] | (((uint8_t*)&pkt->BufStatusBits + (x >> 0x02)) >> (0x02 * (x % 0x04))) & 0x03;
    }
    else
    {
        // No status effect for this slot..
        PTR_status_data.Buffs[x] = -1;
    }
}
```

_The `status_bits_t` structure shows how this can be directly accessed using bits as well instead of manually doing the math above._

## Flags: `flags0_t`

### `HideFlag`

_Unknown._

When this flag is set, it will cause the local player entity to be hidden. It also disables/hides various UI elements such as the status effect icons, clock, location, entity dots on the compass. It will also close open menus and deselect the current target.

### `SleepFlag`

_Unknown._

When this flag is set, it will cause the local player entity to be hidden. It also disables/hides various UI elements such as the status effect icons, clock, location, entity dots on the compass. It will also close open menus and deselect the current target.

### `GroundFlag`

_Flag set if the entity should ignore world collision._

When this flag is set, the client entity can walk through walls. It will still collide with other entities.

### `CliPosInitFlag`

_Unknown._

This flag appears to be used to determine if the client entity is ready for rendering and movement. It is used in a check to determine if the entity model should be hidden from rendering.

### `LfgFlag`

_Flag set if the entity is looking for party._

When this flag is set, it will change the entities name icon to the black and green seeking party flag.

### `AnonymousFlag`

_Flag set if the entity is anonymous. (/anon)_

When this flag is set, it will change the entities name color to dark blue denoting they are hiding their job information.

### `CfhFlag`

_Flag set if the entity is marked as 'called for help'._

When this flag is set, it will change the entities name color to orange.

### `AwayFlag`

_Flag set if the entity if away. (/away - POL status.)_

When this flag is set, it will change the entities name icon to the blue and white circle. This flag also represents that the entity is currently away on their PlayOnline friends list.

### `Gender`

_The entities gender._

This value is used to set the entities gender. This is used during events when addressing the entity as "sir", "ma'am", or similar.

  - `0` = Female
  - `1` = Male

### `unknown_1_9`

_Unknown._

If this flag is set, it will cause the entities `ZoneNo` field to be set with the current `pGlobalNowZone->ZoneNo` value.

### `unknown_1_10`

_Unknown._

This value is used with the main `GC_ZONE` object. _(It is assumed that this is the new `MonStat` value that was used in this packet previously.)_

### `GraphSize`

_The entities model size._

  - `0` = Small
  - `1` = Medium
  - `2` = Large

### `Chocobo_Index`

_Unknown._

This value is used when the entity is mounted. It appears to be used as an animation lookup index in regards to which mounting model animation is played. This is generally seen as being set to `1` when on a chocobo.

### `hpp`

_The entities current health percent._

### `PlayOnelineFlag`

_Flag set if the entity is logged out to PlayOnline._

When this flag is set, it will change the entities name icon to the blue PlayOnline icon. This feature was removed from the game in 2006. However, the icon still exists and this flag still works.

### `LinkShellFlag`

_Flag set if the entity is wearing a linkshell item. (ie. Pearl, Sack, Shell)_

When this flag is set, it will display a linkshell next to the players name. _(Other icons have higher priority to be shown first.)_

### `LinkDeadFlag`

_Flag set if the entity is disconnecting / timing out._

When this flag is set, it will change the entities name icon to the red disconnection circle. _(Also known as 'R0' in the community.)_

### `TargetOffFlag`

_Flag set if the entity should not be targetable._

When this flag is set, the entity cannot be targeted by normal means.

### `unknown_3_28`

_Unknown._

### `GmLevel`

_The entities GM level._

This value can be used to show a number of icons, however the client only uses values `>= 3` to determine GM level. When this value is set, it will also change the entities name icon to one of the following:

  - `0` = None
  - `1` = Trial Account Icon (Green/Yellow Arrow)
  - `2` = Trial Account Icon (Green/Yellow Arrow)
  - `3` = PlayOnline Icon
  - `4` = GM Icon (Red GM icon.)
  - `5` = GM Icon (Red GM icon.)
  - `6` = GM Icon (Red GM icon.)
  - `7` = GM Icon (SquareEnix GM icon.)

## Flags: `flags1_t`

### `Speed`

_The entities movement speed._

This value is calculated as: `entity->MovementSpeed2 = pkt->Flags1.Speed * 0.1;`

### `Hackmove`

_Flag set if the entity should ignore world collision._

When this flag is set, the client entity can walk through walls. It will still collide with other entities.

### `FreezeFlag`

_Flag set if the client is locked in place._

When this flag is set, it locks the client entity in place and removes the compass.

### `unknown_1_14`

_Unknown._

This flag is previously known as `GMInvisFlag` on the PS2 beta.

### `InvisFlag`

_Flag set if the entity is invisible._

### `unknown_2_16`

_Unknown._

This flag is previously known as `PvPFlag` on the PS2 beta.

### `SpeedBase`

_The entities animation speed._

This value is calculated as: `entity->AnimationSpeed = pkt->Flags1.SpeedBase * 0.1;`

### `unknown_3_25`

_Unknown._

The bits of this value used to be part of the `SpeedBase` value, but are no longer used.

### `BazaarFlag`

_Flag set if the entity has a bazaar available._

When this flag is set, it will change the entities name icon to the brown bag icon.

### `CharmFlag`

_Flag set if the entity is charmed._

### `GmIconFlag`

_Flag set if the entity is a GM but is hiding their icon._

When this flag is set, the GM icon will be hidden allowing another flags icon, or none, to be shown instead.

## Flags: `flags2_t`

### `NamedFlag`

_Flag set if the entity is named._

When this flag is set, it will cause any mentions of this entities name to no longer be prefixed with `The`. Most notorious monsters will have this set.

### `SingleFlag`

_Flag set if the entity should be referred to plurally._

When this flag is set, it will cause any references to this entity to be considered plural. For example, when checking an entity it will change the message wording such as `seem` vs. `seems`.

### `AutoPartyFlag`

_Flag set if the entity is auto-seeking a party._

When this flag is set, it will change the entities name icon to the red party seeking icon.

### `PetIndex`

_The entities pet target index._

This value is used to hold the target index of the entities pet.

### `MotStopFlag`

_Flag set if the entities motion scheduler should be paused._

This flag is used when the entity is petrified or terrored. It will cause the entity to pause their current animation and freeze in place. _(Additional flags are set to lock the entity in place.)_

### `CliPriorityFlag`

_Flag set if the entity is considered priority._

When this flag is set, it will override the internal limit when rendering entities to screen. Usually the client will loop through the entity array and draw each valid entity until a limit is reached. This flag will allow the entity to be drawn regardless of this limit being reached each frame.

### `unknown_2_21`

_The entities ballista information._

The client uses this as the clients Ballista flags. _(The values for this are not currently reversed.)_

### `unknown_3_29`

_Unknown._

The client uses this value with Monstrosity. _(The values for this are not currently reversed.)_

## Flags: `flags3_t`

### `LfgMasterFlag`

_Flag set if the entity is seeking a master party._

When this flag is set, it will change the entities name icon to the party seeking icon with the mastery star.

### `TrialFlag`

_Flag set if the entity is on a trial account._

When this flag is set, it will change the entities name icon to the green and yellow arrow icon.

### `unknown_0_2`

_Unknown._

### `NewCharacterFlag`

_Flag set if the entity is a new character._

When this flag is set, it will change the entities name icon to the red question mark icon.

### `MentorFlag`

_Flag set if the entity is a mentor._

When this flag is set, it will change the entities name icon to the mentor 'M' icon.

### `unknown_0_5`

_Unknown._

### `unknown_0_6`

_Unknown._

### `unknown_0_7`

_Unknown._

### `unknown_1_8`

_Unknown._

This value is used with Ballista and Pankration related events. _(The values for this are not currently reversed.)_

### `unknown_2_16`

_Unknown._

This value does not appear to be used currently.

## Flags: `flags4_t`

### `GeoIndiElement`

_The entities indi element and color._

This value is used to set the indi aura element and color. This can be one of the following values:

  - `0x00` = Fire
  - `0x01` = Ice
  - `0x02` = Wind
  - `0x03` = Earth
  - `0x04` = Lightning
  - `0x05` = Water
  - `0x06` = Light
  - `0x07` = Darkness
  - `0x08` = Fire _(Smoked edge.)_
  - `0x09` = Ice _(Smoked edge.)_
  - `0x0A` = Wind _(Smoked edge.)_
  - `0x0B` = Earth _(Smoked edge.)_
  - `0x0C` = Lightning _(Smoked edge.)_
  - `0x0D` = Water _(Smoked edge.)_
  - `0x0E` = Light _(Smoked edge.)_
  - `0x0F` = Darkness _(Smoked edge.)_

Values `0x00` to `0x07` are used with defensive buffs. Values `0x08` to `0x0F` are used of offensive debuffs.

### `GeoIndiSize`

_The entities indi size._

This value is used to increase the size of the indi aura.

### `GeoIndiFlag`

_Flag set if the entity has an active indicolure spell._

When this flag is set, it will enable the indicolure to be rendered around the entity.

### `JobMasterFlag`

_Flag set if the entity has mastered their current job._

When this flag is set, it will draw the 3 job mastery stars over the entities name.

## Flags: `flags5_t`

### `unknown_0_0`

_Unknown._

### `unknown_0_2`

_Unknown._

### `unknown_0_4`

_Unknown._

## Flags: `flags6_t`

### `unknown_0_0`

_Unknown._

### `unknown_0_1`

_Unknown._

### `unknown_0_2`

_Unknown._

### `unknown_0_3`

_Unknown._

### `unknown_0_4`

_Unknown._

### `unknown_0_5`

_Unknown._

### `unknown_0_6`

_Unknown._

### `unknown_0_7`

_Unknown._

The bits of this value are extra and are not used.
