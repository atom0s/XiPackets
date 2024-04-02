# `GP_SERV_COMMAND_CHAR_PC`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_CHAR_PC` |
| **Client Handlers**       | `RecvDebugPc`, `RecvCharPc` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x000D` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the server to update other player entities information. _(Spawn, despawn, general updates etc.)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (Non-defined bitfield type.)
struct sendflags_t
{
    uint8_t     Position        : 1;
    uint8_t     ClaimStatus     : 1;
    uint8_t     General         : 1;
    uint8_t     Name            : 1;
    uint8_t     Model           : 1;
    uint8_t     Despawn         : 1;
    uint8_t     unused          : 2;
};

// PS2: (Unnamed bitfield struct.)
struct flags0_t
{
    uint32_t    MovTime         : 13;   // PS2: MovTime
    uint32_t    RunMode         : 1;    // PS2: RunMode
    uint32_t    unknown_1_6     : 1;    // PS2: TargetMode
    uint32_t    GroundFlag      : 1;    // PS2: GroundFlag
    uint32_t    KingFlag        : 1;    // PS2: KingFlag
    uint32_t    facetarget      : 15;   // PS2: facetarget
};

// PS2: (Unnamed bitfield struct.)
struct flags1_t
{
    uint32_t    MonsterFlag     : 1;    // PS2: MonsterFlag
    uint32_t    HideFlag        : 1;    // PS2: HideFlag
    uint32_t    SleepFlag       : 1;    // PS2: SleepFlag
    uint32_t    unknown_0_3     : 1;    // PS2: MonStat
    uint32_t    unknown_0_4     : 1;    // PS2: (unknown)
    uint32_t    ChocoboIndex    : 3;    // PS2: ChocoboIndex
    uint32_t    CliPosInitFlag  : 1;    // PS2: CliPosInitFlag
    uint32_t    GraphSize       : 2;    // PS2: GraphSize
    uint32_t    LfgFlag         : 1;    // PS2: LfgFlag
    uint32_t    AnonymousFlag   : 1;    // PS2: AnonymousFlag
    uint32_t    YellFlag        : 1;    // PS2: YellFlag
    uint32_t    AwayFlag        : 1;    // PS2: AwayFlag
    uint32_t    Gender          : 1;    // PS2: Gender
    uint32_t    PlayOnelineFlag : 1;    // PS2: PlayOnelineFlag
    uint32_t    LinkShellFlag   : 1;    // PS2: LinkShellFlag
    uint32_t    LinkDeadFlag    : 1;    // PS2: LinkDeadFlag
    uint32_t    TargetOffFlag   : 1;    // PS2: TargetOffFlag
    uint32_t    TalkUcoffFlag   : 1;    // PS2: TalkUcoffFlag
    uint32_t    unknown_2_5     : 1;    // PS2: PartyLeaderFlg
    uint32_t    unknown_2_6     : 1;    // PS2: AllianceLeaderFlg
    uint32_t    unknown_2_7     : 1;    // PS2: DebugClientFlg
    uint32_t    GmLevel         : 3;    // PS2: GmLevel
    uint32_t    HackMove        : 1;    // PS2: HackMove
    uint32_t    unknown_3_4     : 1;    // PS2: GMInvisFlag
    uint32_t    InvisFlag       : 1;    // PS2: InvisFlag
    uint32_t    TurnFlag        : 1;    // PS2: TurnFlag
    uint32_t    BazaarFlag      : 1;    // PS2: BazaarFlag
};

// PS2: (Unnamed bitfield struct.)
struct flags2_t
{
    uint32_t    r               : 8;    // PS2: r
    uint32_t    g               : 8;    // PS2: g
    uint32_t    b               : 8;    // PS2: b
    uint32_t    PvPFlag         : 1;    // PS2: PvPFlag
    uint32_t    ShadowFlag      : 1;    // PS2: ShadowFlag
    uint32_t    ShipStartMode   : 1;    // PS2: ShipStartMode
    uint32_t    CharmFlag       : 1;    // PS2: CharmFlag
    uint32_t    GmIconFlag      : 1;    // PS2: GmIconFlag
    uint32_t    NamedFlag       : 1;    // PS2: NamedFlag
    uint32_t    SingleFlag      : 1;    // PS2: SingleFlag
    uint32_t    AutoPartyFlag   : 1;    // PS2: AutoPartyFlag
};

// PS2: (Unnamed bitfield struct. This has been fully repurposed.)
struct flags3_t
{
    uint32_t    TrustFlag       : 1;    // PS2: (New; replaced 'PetMode'.)
    uint32_t    LfgMasterFlag   : 1;    // PS2: (New; replaced 'PetMode'.)
    uint32_t    PetNewFlag      : 1;    // PS2: PetNewFlag
    uint32_t    unknown_0_3     : 1;    // PS2: PetKillFlag
    uint32_t    MotStopFlag     : 1;    // PS2: MotStopFlag
    uint32_t    CliPriorityFlag : 1;    // PS2: CliPriorityFlag
    uint32_t    PetFlag         : 1;    // PS2: NpcPetFlag
    uint32_t    OcclusionoffFlag: 1;    // PS2: OcclusionoffFlag
    uint32_t    BallistaTeam    : 8;    // PS2: (New; did not exist.)
    uint32_t    MonStat         : 3;    // PS2: (New; did not exist.)
    uint32_t    unknown_2_3     : 1;    // PS2: (New; did not exist.)
    uint32_t    unknown_2_4     : 1;    // PS2: (New; did not exist.)
    uint32_t    SilenceFlag     : 1;    // PS2: (New; did not exist.)
    uint32_t    unknown_2_6     : 1;    // PS2: (New; did not exist.)
    uint32_t    NewCharacterFlag: 1;    // PS2: (New; did not exist.)
    uint32_t    MentorFlag      : 1;    // PS2: (New; did not exist.)
    uint32_t    unknown_3_1     : 1;    // PS2: (New; did not exist.)
    uint32_t    unknown_3_2     : 1;    // PS2: (New; did not exist.)
    uint32_t    unknown_3_3     : 1;    // PS2: (New; did not exist.)
    uint32_t    unknown_3_4     : 1;    // PS2: (New; did not exist.)
    uint32_t    unknown_3_5     : 1;    // PS2: (New; did not exist.)
    uint32_t    unknown_3_6     : 1;    // PS2: (New; did not exist.)
    uint32_t    unknown_3_7     : 1;    // PS2: (New; did not exist.)
};

// PS2: (New; did not exist.)
struct flags4_t
{
    uint8_t     unknown_0_0     : 1;    // PS2: (New; did not exist.)
    uint8_t     TrialFlag       : 1;    // PS2: (New; did not exist.)
    uint8_t     unknown_0_2     : 2;    // PS2: (New; did not exist.)
    uint8_t     unknown_0_4     : 2;    // PS2: (New; did not exist.)
    uint8_t     JobMasterFlag   : 1;    // PS2: (New; did not exist.)
    uint8_t     unknown_0_7     : 1;    // PS2: (New; did not exist.)
};

// PS2: (New; did not exist.)
struct flags5_t
{
    uint8_t     GeoIndiElement  : 4;    // PS2: (New; did not exist.)
    uint8_t     GeoIndiSize     : 2;    // PS2: (New; did not exist.)
    uint8_t     GeoIndiFlag     : 1;    // PS2: (New; did not exist.)
    uint8_t     unknown_0_7     : 1;    // PS2: (New; did not exist.)
};

// PS2: (New; did not exist.)
struct flags6_t
{
    uint32_t    GateId          : 4;    // PS2: (New; did not exist.)
    uint32_t    MountIndex      : 8;    // PS2: (New; did not exist.)
    uint32_t    unknown_1_3     : 20;   // PS2: (New; did not exist.)
};

// PS2: GP_SERV_CHAR_PC
struct GP_SERV_CHAR_PC
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    // PS2: GP_SERV_POS_HEAD
    uint32_t    UniqueNo;           // PS2: UniqueNo
    uint16_t    ActIndex;           // PS2: ActIndex
    sendflags_t SendFlg;            // PS2: SendFlg
    uint8_t     dir;                // PS2: dir
    float       x;                  // PS2: x
    float       z;                  // PS2: z
    float       y;                  // PS2: y
    flags0_t    Flags0;             // PS2: <bits> (Nameless bitfield.)
    uint8_t     Speed;              // PS2: Speed
    uint8_t     SpeedBase;          // PS2: SpeedBase
    uint8_t     Hpp;                // PS2: HpMax
    uint8_t     server_status;      // PS2: server_status
    flags1_t    Flags1;             // PS2: <bits> (Nameless bitfield.)
    flags2_t    Flags2;             // PS2: <bits> (Nameless bitfield.)
    flags3_t    Flags3;             // PS2: <bits> (Nameless bitfield.)
    uint32_t    BtTargetID;         // PS2: BtTargetID

    // PS2: GP_SERV_CHAR_PC remaining fields.
    uint16_t    CostumeId;          // PS2: (New; did not exist.)
    uint8_t     BallistaInfo;       // PS2: (New; did not exist.)
    flags4_t    Flags4;             // PS2: (New; did not exist.)
    uint32_t    CustomProperties[2];// PS2: (New; did not exist.)
    uint16_t    PetActIndex;        // PS2: (New; did not exist.)
    uint16_t    MonstrosityFlags;   // PS2: (New; did not exist.)
    uint8_t     MonstrosityNameId1; // PS2: (New; did not exist.)
    uint8_t     MonstrosityNameId2; // PS2: (New; did not exist.)
    flags5_t    Flags5;             // PS2: (New; did not exist.)
    uint8_t     ModelHitboxSize;    // PS2: (New; did not exist.)
    flags6_t    Flags6;             // PS2: (New; did not exist.)
    uint16_t    GrapIDTbl[9];       // PS2: GrapIDTbl
    uint8_t     name[16];           // PS2: name
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_CHAR_PC`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The players server id._

### `ActIndex`

_The players target index._

### `SendFlg`

_The update flags which state the fields that are populated in this packet._

  - When a player is first spawning, this will be set to `0x1F`.
  - When a player is being despawned, this will be set to `0x20`.
  - When a player is being updated, this will be set based on which flags are used.

_For more information about this field, see the structure information below for `sendflags_t`._

### `dir`

_The players heading direction._

This value is the players heading direction, encoded down into a single byte. The client will read and recalculate this value back into a float using the following:

```cpp
float __cdecl FUNC_enDirNetToCli(uint8_t dir)
{
    return dir * 6.283185 * 0.00390625;
}
```

### `x`

_The players X position._

### `z`

_The players Z position._

### `y`

_The players Y position._

### `Flags0`

_The players flags (0)._

### `Speed`

_The players movement speed._

This value is the players movement speed, encoded down into a single byte. The client will read and recalculate this value back into a float using the following:

```cpp
player->MovementSpeed2 = pkt->Speed * 0.1f;
```

### `SpeedBase`

_The players animation speed._

This value is the players animation speed, encoded down into a single byte. The client will read and recalculate this value back into a float using the following:

```cpp
player->AnimationSpeed = pkt->SpeedBase * 0.1f;
```

### `Hpp`

_The players health percent._

### `server_status`

_The players server status._

### `Flags1`

_The players flags (1)._

### `Flags2`

_The players flags (2)._

### `Flags3`

_The players flags (3)._

### `BtTargetID`

_The players claim status and claim target information._

### `CostumeId`

_The players costume id._

### `BallistaInfo`

_The players Ballista information._

This value is used as extended team/name flag values related to Ballista/pvp game modes. It has special handling for values `6` and `7`.

### `Flags4`

_The players flags (4)._

### `CustomProperties`

_The players custom properties._

This value is used to hold the players extended custom properties. This is mainly used with the Chocobo system(s) of the game. This information will be populated to show the players custom mount data such as breed, color, and other potential information.

_Currently, the client only seems to use the first `uint32_t` value of this data._

### `PetActIndex`

_The players pet target index._

### `MonstrosityFlags`

_The players Monstrosity flags._

### `MonstrosityNameId1`

_The players Monstrosity name id. (1)_

### `MonstrosityNameId2`

_The players Monstrosity name id. (2)_

### `Flags5`

_The players flags (5)._

### `ModelHitboxSize`

_The players model hitbox size._

This value is the players model hitbox size, encoded down into a single byte. The client will read and recalculate this value back into a float using the following:

```cpp
player->ModelHitboxSize = pkt->ModelHitboxSize * 0.1f;
```

### `Flags6`

_The players flags (6)._

### `GrapIDTbl`

_The players model information._

This array of values holds the players various visual information related to their model. This includes their race, face/hair style and the equipment they are currently wearing. _(If the player is style locked, then these ids will represent the model of the style locked equipment, not their actual equipped gear.)_

  - `GrapIDTbl[0]` - _The clients race and hair ids._
  - `GrapIDTbl[1]` - _The clients head equipment visual model id._
  - `GrapIDTbl[2]` - _The clients body equipment visual model id._
  - `GrapIDTbl[3]` - _The clients hands equipment visual model id._
  - `GrapIDTbl[4]` - _The clients legs equipment visual model id._
  - `GrapIDTbl[5]` - _The clients feet equipment visual model id._
  - `GrapIDTbl[6]` - _The clients main equipment visual model id._
  - `GrapIDTbl[7]` - _The clients sub equipment visual model id._
  - `GrapIDTbl[8]` - _The clients ranged equipment visual model id._

### `name`

_The players name._

## Structure Fields (`sendflags_t`)

### `Position`

_Flag set when the players position related information is being updated._

When this flag is set, the following packet fields are used:

  - `dir`, `x`, `z`, `y`
  - `Flags0`
  - `Speed`, `SpeedBase`

### `ClaimStatus`

_Flag set when the players claim status information is being updated._

When this flag is set, the following packet fields are used:

  - `BtTargetID`

### `General`

_Flag set when the player is being updated for multiple purposes._

When this flag is set, the following packet fields are used:

  - `Hpp`, `server_status`
  - `Flags1`, `Flags2`, `Flags3`, `Flags4`, `Flags5`, `Flags6`
  - `ModelHitboxSize`
  - `CustomProperties`
  - `MonstrosityFlags`, `MonstrosityNameId1`, `MonstrosityNameId2`

### `Name`

_Flag set when the players name is being updated._

When this flag is set, the following packet fields are used:

  - `name`

### `Model`

_Flag set when the players model visuals are being updated._

When this flag is set, the following packet fields are used:

  - `GrapIDTbl`

### `Despawn`

_Flag set when the player is being despawned._

### `unused`

_Unused bits._

### `Structure Footnotes`

The following fields are always checked by the client when not despawning a player:

  - `CostumeId`
  - `BallistaInfo`
  - `PetActIndex`
  - `Flags4`

## Structure Fields (`flags0_t`)

### `MovTime`

_The players current movement time._

This value increments as the player is moving around which the client uses to handle the walk/run animation timing.

### `RunMode`

_Unknown._

This value is used within events.

_The client reflects this value back when sending `0x0015` packets to the server._

### `unknown_1_6`

_Unknown._

_The client reflects this value back when sending `0x0015` packets to the server._

### `GroundFlag`

_Flag set if the player should ignore world collision._

When this flag is set, the player can walk through walls. It will still collide with other entities.

_The client reflects this value back when sending `0x0015` packets to the server._

### `KingFlag`

_Unused._

This bit is not used for players.

### `facetarget`

_The target index of the players current target._

The client uses this value to turn the players head towards the target entity.

## Structure Fields (`flags1_t`)

### `MonsterFlag`

_Flag set if the player is a monster._

If this flag is set, the client will show the `Attack` menu entry when this player is targeted.

### `HideFlag`

_Flag set if the player should be fully hidden and untargetable._

### `SleepFlag`

_Flag set if the player is currently in a sleep state. (Not sleep status related.)_

This flag is set when the player is in a state where their scheduler is currently suspended. This is used with scheduler related things and events. If this flag is set, the player will not be rendered and cannot be targeted.

### `unknown_0_3`

_Flag set if the player is within a zone-wide based treasure pool area._

When this flag is set, it will update the players `ZoneId` value with the current zone of the local clients global zone state. This is used during situations that the extended treasure pool system is shown and used. _(ie. instance based looting or zone-wide looting.)_

### `unknown_0_4`

_Unknown._

This bit is not used.

### `ChocoboIndex`

_The players chocobo index._

This value is used to define the players chocobo type if they are on a special chocobo vs. a normal one.

The client will specifically look for values `2` and `3` when using special cases that also make use of the `CustomProperties` information. Higher values will change the manner in which the player is seated on the expected chocobo / mount model.

### `CliPosInitFlag`

_Flag set if the players position information is being initialized._

This flag does not appear to be used similar to the client specific version of this flag in the `0x0037` packet.

### `GraphSize`

_The players model size._

  - `0` = _Small_
  - `1` = _Medium_
  - `2` = _Large_

### `LfgFlag`

_Flag set if the player is looking for party._

When this flag is set, it will change the players name icon to the black and green seeking party flag.

### `AnonymousFlag`

_Flag set if the player is anonymous. (`/anon`)_

When this flag is set, it will change the players name color to dark blue denoting they are hiding their job information.

### `YellFlag`

_Flag set if the player is called for help on._

When this flag is set, it will change the players name color to orange.

### `AwayFlag`

_Flag set if the player is away. (`/away`)_

When this flag is set, it will change the players name icon to the blue and white circle. This flag also represents that the player is currently away on their PlayOnline friends list.

### `Gender`

_The players gender._

This value is used to set the players gender. This is used with strings and/or during events when addressing the player as "sir", "ma'am", "his/her" or similar.

  - `0` = Female
  - `1` = Male

### `PlayOnelineFlag`

_Flag set if the player is logged out to PlayOnline._

When this flag is set, it will change the players name icon to the blue PlayOnline icon. This feature was removed from the game in 2006. However, the icon still exists and this flag still works.

### `LinkShellFlag`

_Flag set if the player is wearing a linkshell item. (ie. Pearl, Sack, Shell)_

When this flag is set, it will display a linkshell next to the players name. _(Other icons have higher priority to be shown first.)_

### `LinkDeadFlag`

_Flag set if the player is disconnecting / timing out._

When this flag is set, it will change the players name icon to the red disconnection circle. _(Also known as 'R0' in the community.)_

### `TargetOffFlag`

_Flag set if the player should not be targetable._

When this flag is set, the player cannot be targeted by normal means.

### `TalkUcoffFlag`

_Unknown._

This value is used with events.

### `unknown_2_5`

_Unknown._

This bit is unused.

### `unknown_2_6`

_Unknown._

This bit is unused.

### `unknown_2_7`

_Unknown._

This bit is unused.

### `GmLevel`

_The players GM level._

This value can be used to show a number of icons, however the client only uses values `>= 3` to determine GM level. When this value is set, it will also change the players name icon to one of the following:

  - `0` = _None_
  - `1` = _Trial Account Icon (Green/Yellow Arrow)_
  - `2` = _Trial Account Icon (Green/Yellow Arrow)_
  - `3` = _PlayOnline Icon_
  - `4` = _GM Icon (Red GM icon.)_
  - `5` = _GM Icon (Red GM icon.)_
  - `6` = _GM Icon (Red GM icon.)_
  - `7` = _GM Icon (SquareEnix GM icon.)_

### `HackMove`

_Flag set if the player should ignore world collision._

When this flag is set, the player can walk through walls. It will still collide with other entities.

### `unknown_3_4`

_Unknown._

The client will use this value but it does not appear to be used after being read from this handler.

### `InvisFlag`

_Flag set if the player is invisible._

### `TurnFlag`

_Flag used to lerp the players heading direction._

This value is used in special conditions to lerp the players heading direction to have them move over time instead of immediately turning.

### `BazaarFlag`

_Flag set if the player has a bazaar available._

When this flag is set, it will change the player name icon to the brown bag icon.

## Structure Fields (`flags2_t`)

### `r`

_The players linkshell color. (Red)_

### `g`

_The players linkshell color. (Green)_

### `b`

_The players linkshell color. (Blue)_

### `PvPFlag`

_Flag set if the player currently has Gate Breach status in PvP content._

### `ShadowFlag`

_Flag set if the players shadow should be hidden._

### `ShipStartMode`

_Unused._

This bit is unused.

### `CharmFlag`

_Flag set if the player is charmed._

If this flag is set, the client will show the `Attack` menu entry when this player is targeted.

### `GmIconFlag`

_Flag set if the player is a GM but is hiding their icon._

When this flag is set, the GM icon will be hidden allowing another flags icon, or none, to be shown instead.

### `NamedFlag`

_Flag set if the player is named._

When this flag is set, it will cause any mentions of this players name to not be prefixed with `The`.

### `SingleFlag`

_Flag set if the player should be referred to plurally._

When this flag is set, it will cause any references to this player to be considered plural.

### `AutoPartyFlag`

_Flag set if the player is auto-seeking a party._

When this flag is set, it will change the players name icon to the red party seeking icon.

## Structure Fields (`flags3_t`)

### `TrustFlag`

_Flag set if the entity is a Trust._

When this flag is set, it will cause the client to show the target sub-menu when clicking on the entity. If this flag is not set, and the entity is a trust, then they will be recalled immediately.

### `LfgMasterFlag`

_Flag set if the player is seeking a master party._

When this flag is set, it will change the players name icon to the green party seeking icon with the master star.

### `PetNewFlag`

_Flag set if the entity is a pet being spawned._

This flag changes the spawn animation used when the entity is first popped.

### `unknown_0_3`

_Unknown._

The purpose of this flag is unknown at this time.

### `MotStopFlag`

_Flag set if the player motion scheduler should be paused._

This flag is used when the player is petrified or terrored. It will cause the player to pause their current animation and freeze in place. _(Additional flags are set to lock the player in place.)_

### `CliPriorityFlag`

_Flag set if the entity is considered priority._

When this flag is set, it will override the internal limit when rendering entities to screen. Usually the client will loop through the entity array and draw each valid entity until a limit is reached. This flag will allow the entity to be drawn regardless of this limit being reached each frame.

### `PetFlag`

_Flag set if the player is a pet._

### `OcclusionoffFlag`

_Unused._

This flag is not used for player entities.

### `BallistaTeam`

_The players current Ballista team id._

This value is used to set the players current Ballista team id under certain conditions.

| Id | Purpose |
| --- | --- |
| `0` | _None_ |
| `1` | _San d'Oria (Square Flag)_ |
| `2` | _Bastok (Square Flag)_ |
| `3` | _Windurst (Square Flag)_ |
| `4` | _Wyverns (Blue Flag)_ |
| `5` | _Griffons (Green Flag)_ |
| `6` | _Wyverns (Blue Flag)_ |
| `7` | _Griffons (Green Flag)_ |

The values 6 amd 7 are special cases used in Brenner when the entity is currently holding a captured Flamme. These values are required in order to change the nameplate icon to the flamme rock. _(Requires additional packets to fully work.)_

### `MonStat`

_The entities sub-animation flag(s)._

This value can be used for multiple purposes depending on the given entity, type and other conditions. The main purpose of this value is the entities sub-animation flags. _(Depending on the entity type and other conditions this value is treated as either 2 or 3 bits.)_

_When this flag is used with player entities, it is generally due to Monstrosity._

### `unknown_2_3`

_Unknown._

This flag is not used for player entities.

### `unknown_2_4`

_Unknown._

This flag is not used for player entities.

### `SilenceFlag`

_Flag set if the entity should not make footstep sounds when moving._

### `unknown_2_6`

_Unknown._

This flag is not used for player entities.

### `NewCharacterFlag`

_Flag set if the player is a new character._

When this flag is set, it will change the players name icon to the red question mark icon.

### `MentorFlag`

_Flag set if the player is a mentor._

When this flag is set, it will change the players name icon to the mentor 'M' icon.

### `unknown_3_1`

_Unknown._

When this flag is set, it will cause the entities animations to use different values. For example, this will override the manner in which the entity will sit in a chair.

### `unknown_3_2`

_Unknown._

This flag is not used for player entities.

### `unknown_3_3`

_Unknown._

This flag is not used for player entities.

### `unknown_3_4`

When this flag is set, it will cause the entity to become non-blocking. This means that the local client can immediately pass through the entity without collision happening. _(This flag causes the entity to be skipped during the `XiControlActor::CheckContactActor` call.)_

### `unknown_3_5`

_Unknown._

This flag is not used for player entities.

### `unknown_3_6`

_Unknown._

This flag is not used for player entities.

### `unknown_3_7`

_Unknown._

This flag is not used for player entities.

## Structure Fields (`flags4_t`)

### `unknown_0_0`

_Unknown._

### `TrialFlag`

_Flag set if the player is on a trial account._

When this flag is set, it will change the players name icon to the green and yellow arrow icon.

### `unknown_0_2`

_Unknown._

### `unknown_0_4`

_Unknown._

### `JobMasterFlag`

_Flag set if the player has mastered their current job._

When this flag is set, it will display the job mastery stars over the players name.

### `unknown_0_7`

_Unknown._

## Structure Fields (`flags5_t`)

### `GeoIndiElement`

_The players indi element and color._

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

_The players indi size._

This value is used to increase the size of the indi aura.

### `GeoIndiFlag`

_Flag set if the player has an active indicolure spell._

When this flag is set, it will enable the indicolure to be rendered around the player.

### `unknown_0_7`

_Unused._

This bit is unused.

## Structure Fields (`flags6_t`)

### `GateId`

_The group id that the player is part of for special battle content._

This value is used under special conditions to hide entities that are not in the same 'group' as the local client player. This value is compared to the local clients own same id during certain conditions, if the values do not match then this player will be hidden from rendering to the local player. _(This is used for special gated content such as Domain Invasion.)_

### `MountIndex`

_The players mount index._

### `unknown_1_3`

_Unused._

These bits are unused.

## Additional Information

The server uses this packet to handle updates related to other player entities. This includes their initial spawning, despawning and various general updates to other information about the player. The packets `SendFlg` field is used to tell the client which fields in the packet are actually populated and will be used. The client has special handling for some of the flag combinations such as spawning and despawning.

The client will handle the packet in the following manner:

_**Note:** This is pseudo code; not meant to be an exact match._
```cpp
// Checks if the target index is a valid player index:
// pkt->ActIndex >= 1024 && pkt->ActIndex < 1792
if (!IsPlayerEntity(pkt->ActIndex))
    return 0;

// Create a new entity instance for the player if it does not already exist..
if (!ActBuffPtr[pkt->ActIndex])
    ActBuffPtr[pkt->ActIndex] = new XiAtelBuff();

// Obtain the player entity..
auto entity = ActBuffPtr[pkt->ActIndex];

// Reinitialize the entity object if the players server id does not match the previous existing value..
if (pkt->UniqueNo && pkt->UniqueNo != entity->ServerId)
{
    // Cleanup this entity to reset it to defaults..
    entity->EventDelete();
    entity->ObjectDelete();
    entity->Reset(0);

    // Cleanup all stale entities..
    XiAtelBuff::ResetStaleEntities(pkt->ActIndex, pkt->UniqueNo);
}

// Check if the player is spawning.. (Full update..)
if ((pkt->SendFlg & 0x1F) == 0x1F)
{
    // Cleanup all stale entities..
    XiAtelBuff::ResetStaleEntities(pkt->ActIndex, pkt->UniqueNo);

    entity->UpdateMask  = pkt->SendFlg;
    entity->IsDirty     = 0;
}

// Update the entities general id information..
entity->TargetIndex = pkt->ActIndex;
entity->ServerId    = pkt->UniqueNo;

// snipped updates..

// Check if the player is despawning..
if ((pkt->SendFlg & 0x20) != 0)
{
    entity->PetTargetIndex  = 0;
    entity->Render.Flags1   |= 0x1000;
    entity->UpdateMask      = 0;

    if ((entity->Render.Flags0 & 0x100) == 0)
    {
        entity->Render.Flags0 |= 0x4000;
        return 1;
    }
}

// snipped general non-flag dependent updates..

if (!entity->UpdateMask)
{
    // Validate this entity is being despawned by having no UpdateMask and not being spawned..
    if ((pkt->SendFlag & 0x1F) != 0x1F)
    {
        entity->EventDelete();
        entity->ObjectDelete();
        entity->Reset(0);
        entity->Render.Flags3 |= 0x8000;
        return 0;
    }

    entity->UpdateMask = pkt->SendFlg;
}

// Update the players name..
if ((pkt->SendFlg & 8) != 0)
{
    enQueStrCpy(buffer, 0x10, pkt->name, 92, *pkt >> 9);
    entity->SetName(buffer);
}

// Update the players position information..
if ((pkt->SendFlg & 1) != 0)
{
    entity->Movement.Move.X = pkt->x;
    entity->Movement.Move.Y = pkt->y;
    entity->Movement.Move.Z = pkt->z;

    if (!PTR_Event_UnknownFlag || !IsEvent(255) || (entity->Render.Flags0 & 0x80) != 0 || (entity->Render.Flags0 & 0x100) != 0)
    {
        if (VCalibrate(g_pTsZoneMap, pkt->x, pkt->z, pkt->y, 50.0f, &pos) == 1)
            pkt->z = pos.x;
    }

    entity->Movement.LastPosition.X     = pkt->x;
    entity->Movement.LastPosition.Y     = pkt->y;
    entity->Movement.LastPosition.Z     = pkt->z;
    entity->Movement.LastPosition.Roll  = 0.0f;
    entity->Movement.LastPosition.Yaw   = enDirNetToCli(pkt->dir);
    entity->Movement.LastPosition.Pitch = 0.0f;

    entity->Movement.LocalPosition.X    = pkt->x;
    entity->Movement.LocalPosition.Y    = pkt->y;
    entity->Movement.LocalPosition.Z    = pkt->z;
    entity->Movement.LocalPosition.Roll = 0.0f;
    entity->Movement.LocalPosition.Yaw  = enDirNetToCli(pkt->dir);
    entity->Movement.LocalPosition.Pitch= 0.0f;

    entity->MovementSpeed2 = pkt->Speed * 0.1f;
    entity->AnimationSpeed = pkt->SpeedBase * 0.1f;

    entity->UpdateMoveTime(pkt->Flags0 & 0x1FFF);

    // snipped flag updates..
}

// Update the players claim status information..
if ((pkt->SendFlg & 2) != 0)
    entity->ClaimStatus = pkt->BtTargetID;

// Update the players general information..
if ((pkt->SendFlg & 4) != 0)
{
    // snipped flag updates..

    entity->SetChocoboIndex((pkt->Flags1 >> 5) & 7);
    entity->SetCustomProperties(&pkt->CustomProperties);

    // snipped flag updates..

    entity->HPPercent = pkt->Hpp;

    if ((entity->Render.Flags0 & 2) == 0)
        entity->StatusServer = pkt->server_status;

    entity->LinkshellColor = pkt->Flags2.r + (pkt->Flags2.g + (pkt->Flags2.b << 8) << 8);

    // snipped flag updates..

    SetEntityNamedFlag(pkt->ActIndex, pkt->Flags2.NamedFlag);
    SetEntitySingleFlag(pkt->ActIndex, pkt->Flags2.SingleFlag);

    // snipped flag updates..

    if ((pkt->Flags1 & 8) != 0)
        entity->ZoneId = pGlobalNowZone->ZoneNo;
    else
        entity->ZoneId = 0;

    entity->SetMonstrosityFlags(pkt->MonstrosityFlags);
    entity->SetMonstrosityNameId(0, pkt->MonstrosityNameId1);
    entity->SetMonstrosityNameId(1, pkt->MonstrosityNameId2);

    // snipped flag updates..

    entity->SetGeoIndi(pkt->Flags5 & 0x0F, (pkt->Flags5 >> 4) & 3, (pkt->Flags5 >> 6) & 1);

    entity->ModelHitboxSize = pkt->ModelHitboxSize * 0.1f;
    entity->MountId = pkt->Flags6 >> 4;
}

// Reset the FPS divisor if this packet updates the local client player..
if (PTR_LoginActIndex == pkt->ActIndex)
    SetFpsDivisor(2);

return 1;
```

_The above code is a shortened example of how the handler for this packet works to demonstrate the means in which the `SendFlg` values are used, in what order, and how additional calls are made to prepare, reinitialize and otherwise destroy player entities._
