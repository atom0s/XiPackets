# `GP_SERV_COMMAND_CHAR_NPC`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_CHAR_NPC` |
| **Client Handlers**       | `RecvDebugNpc`, `RecvCharNpc` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x000E` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the server to update non-entity entities information. _(Spawn, despawn, general updates etc.)_

_**Note:** While this packet shares the same flag structures as packet `0x000D` (for player entities), many of the flags actual usage/purposes are different regardless of their names. Please be sure to take note of this when reviewing this packets information!_

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
    uint8_t     Name2           : 1;
    uint8_t     unused          : 1;
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

// PS2: GP_SERV_CHAR_NPC
struct GP_SERV_CHAR_NPC
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

    // PS2: GP_SERV_CHAR_NPC_HEAD
    uint16_t    SubKind: 3;         // PS2: SubKind
    uint16_t    Status: 13;         // PS2: Status

    // PS2: GP_SERV_CHAR_NPC remaining fields.
    uint8_t     Data[];             // PS2: (New; did not exist.) (Data that fills the rest of the packet is based on the SendFlg and SubKind values!)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_CHAR_PC`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The entities server id._

### `ActIndex`

_The entities target index._

### `SendFlg`

_The update flags which state the fields that are populated in this packet._

_For more information about this field, see the structure information below for `sendflags_t`._

### `dir`

_The entities heading direction._

This value is the entities heading direction, encoded down into a single byte. The client will read and recalculate this value back into a float using the following:

```cpp
float __cdecl FUNC_enDirNetToCli(uint8_t dir)
{
    return dir * 6.283185 * 0.00390625;
}
```

### `x`

_The entities X position._

### `z`

_The entities Z position._

### `y`

_The entities Y position._

### `Flags0`

_The entities flags (0)._

### `Speed`

_The entities movement speed._

This value is the entities movement speed, encoded down into a single byte. The client will read and recalculate this value back into a float using the following:

```cpp
entity->MovementSpeed2 = pkt->Speed * 0.1f;
```

### `SpeedBase`

_The entities animation speed._

This value is the entities animation speed, encoded down into a single byte. The client will read and recalculate this value back into a float using the following:

```cpp
entity->AnimationSpeed = pkt->SpeedBase * 0.1f;
```

### `Hpp`

_The entities health percent._

### `server_status`

_The entities server status._

### `Flags1`

_The entities flags (1)._

### `Flags2`

_The entities flags (2)._

### `Flags3`

_The entities flags (3)._

### `BtTargetID`

_The entities claim status and claim target information._

### `SubKind`

_The packet sub-kind._

This value is used as a means to determine the kind of entity that is being updated in the packet. The handler uses this value, with the `SendFlg` value, to determine how to handle the various bits of data populated in the packet.

| SubKind | Purpose |
| --- | --- |
| `0` | _Used with fixed-model npcs._ |
| `1` | _Used with equipped npcs. (Wearing player equipment models.)_ |
| `2` | _Used with doors._ |
| `3` | _Used with elevators and other movables._ |
| `4` | _Used with airships, boats, and similar npcs. (Some gardening plants use this `SubKind` as well.)_ |
| `5` | _Used with misc npcs._ |
| `6` | _Used with misc npcs. (ie. Automatons)_ |
| `7` | _Used with misc npcs._ |

### `Status`

_The packet status._

This value is not used.

### `Data`

_The additional packet data._

The rest of the packets data will be populated based on the current `SendFlg` and `SubKind` values.

## Structure Fields (`sendflags_t`)

### `Position`

_Flag set when the entities position related information is being updated._

When this flag is set, the following packet fields are used:

  - `dir`, `x`, `z`, `y`
  - `Flags0`, `Flags1`
  - `Speed`, `SpeedBase`

### `ClaimStatus`

_Flag set when the entities claim status information is being updated._

When this flag is set, the following packet fields are used:

  - `BtTargetID`
  - `Flags1`

### `General`

_Flag set when the entity is being updated for multiple purposes._

When this flag is set, the following packet fields are used:

  - `Hpp`, `server_status`
  - `Flags1`, `Flags2`, `Flags3`

### `Name`

_Flag set when the entities name is being updated._

There are special cases/conditions for when this is used and what values will land up being used with it. The packets `Data` information will be populated with additional information that this mode will use.

### `Model`

_Flag set when the entities model visuals are being updated._

There are special cases/conditions for when this is used and what values will land up being used with it. The packets `Data` information will be populated with additional information that this mode will use.

### `Despawn`

_Flag set when the entity is being despawned._

### `Name2`

_Flag set when the entities name is being updated._

There are special cases/conditions for when this is used and what values will land up being used with it. The packets `Data` information will be populated with additional information that this mode will use.

### `unused`

_Unused bit._

## Structure Fields (`flags0_t`)

### `MovTime`

_The entities current movement time._

This value increments as the entity is moving around which the client uses to handle the walk/run animation timing.

### `RunMode`

_Unknown._

This value is used within events.

### `unknown_1_6`

_Unknown._

### `GroundFlag`

_Flag set if the entity should ignore world collision._

When this flag is set, the entity can walk through walls. It will still collide with other entities.

### `KingFlag`

_Flag set if the entity should be marked as a King._

This flag is used to when the client is expected to wait upon entering a zone until a specific number of `King` entities have loaded. This is sent inside of the `0x000A` login packet as the `SendCount` value. Until the expected number of `King` entities is seen, the client will be locked in place and cannot move.

### `facetarget`

_The target index of the entities current target._

The client uses this value to turn the entities head towards the target entity.

## Structure Fields (`flags1_t`)

### `MonsterFlag`

_Flag set if the entity is a monster._

This flag is used to differ between normal NPCs and monsters. If this flag is not set, the entities name will be defaulted to green and is non-attackable. Otherwise, if this flag is set, the entities name will be defaulted to yellow and be attackable.

### `HideFlag`

_Flag set if the entity should be fully hidden and untargetable._

### `SleepFlag`

_Flag set if the entity is currently in a sleep state. (Not sleep status related.)_

This flag is set when the entity is in a state where their scheduler is currently suspended. This is used with scheduler related things and events. If this flag is set, the entity will not be rendered and cannot be targeted.

### `unknown_0_3`

_Unused._

This bit is not used.

### `unknown_0_4`

_Unknown._

The purpose of this flag is unknown at this time.

### `ChocoboIndex`

_The entities chocobo index._

This value is used to define the entities chocobo type if they are on a special chocobo vs. a normal one.

### `CliPosInitFlag`

_Flag set if the entities position information is being initialized._

### `GraphSize`

_The entities model size._

  - `0` = _Small_
  - `1` = _Medium_
  - `2` = _Large_

### `LfgFlag`

_Unknown._

This flag is used for a language-specific purpose. _(Generally used with the French and German language strings.)_

### `AnonymousFlag`

_Unknown._

This flag is used for a language-specific purpose. _(Generally used with the French and German language strings.)_

### `YellFlag`

_Flag set if the entity is called for help on._

When this flag is set, it will change the entities name color to orange.

### `AwayFlag`

_Unknown._

The purpose of this flag is unknown at this time.

### `Gender`

_The entities gender._

This value is used to set the entities gender. This is used with strings and/or during events when addressing the entity as "sir", "ma'am", "his/her" or similar.

  - `0` = Female
  - `1` = Male

### `PlayOnelineFlag`

_Flag set if the entity health bar, when targeted, should be hidden._

When this flag is set, it will cause the entities health bar to be hidden when targeted.

### `LinkShellFlag`

_Unknown._

This flag is used for a language-specific purpose. _(Generally used with the French and German language strings.)_

### `LinkDeadFlag`

_Unknown._

This flag is used for a language-specific purpose. _(Generally used with the French and German language strings.)_

### `TargetOffFlag`

_Flag set if the entity should not be targetable._

When this flag is set, the entity cannot be targeted by normal means.

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

_The entities GM level._

This value can be used to show a number of icons, however the client only uses values `>= 3` to determine GM level. When this value is set, it will also change the entities name icon to one of the following:

  - `0` = _None_
  - `1` = _Trial Account Icon (Green/Yellow Arrow)_
  - `2` = _Trial Account Icon (Green/Yellow Arrow)_
  - `3` = _PlayOnline Icon_
  - `4` = _GM Icon (Red GM icon.)_
  - `5` = _GM Icon (Red GM icon.)_
  - `6` = _GM Icon (Red GM icon.)_
  - `7` = _GM Icon (SquareEnix GM icon.)_

### `HackMove`

_Unknown._

This bit is unused.

### `unknown_3_4`

_Unknown._

The purpose of this flag is unknown at this time.

### `InvisFlag`

_Unknown._

The purpose of this flag is unknown at this time. When this flag is set, the entity will not render on the clients compass.

### `TurnFlag`

_Flag used to lerp the entities heading direction._

This value is used in special conditions to lerp the entities heading direction to have them move over time instead of immediately turning.

### `BazaarFlag`

_Flag set if the entity has a bazaar available._

When this flag is set, it will change the entity name icon to the brown bag icon.

## Structure Fields (`flags2_t`)

### `r`

_Misc usages._

This value is used for different purposes depending on the entity type and flags. It can be used for the following purposes:

  - The entities Ballista name flags. _(If in pvp.)_
  - The entities mount id. _(If not in pvp.)_

### `g`

_The entities model hitbox size._

This value is the entities model hitbox size, encoded down into a single byte. The client will read and recalculate this value back into a float using the following:

```cpp
entity->ModelHitboxSize = pkt->Flags2.g * 0.1f;
```

### `b`

_Misc usages._

This value is used for different purposes depending on the entity type and flags. It can be used for the following purposes:

  - The entities GEO Indi information. _(`pkt->Flags2.b & 0x0F`)_
  - Used for various flags. _(Unknown flag purposes at this time. This value is broken into a different set of bits.)_

### `PvPFlag`

_Flag set if the entity currently has Gate Breach status in PvP content._

### `ShadowFlag`

_Flag set if the entities shadow should be hidden._

### `ShipStartMode`

_Unused._

This bit is unused.

### `CharmFlag`

_Flag set if the entity is charmed._

### `GmIconFlag`

_Flag set if the entity is a GM but is hiding their icon._

When this flag is set, the GM icon will be hidden allowing another flags icon, or none, to be shown instead.

### `NamedFlag`

_Flag set if the entity is named._

When this flag is set, it will cause any mentions of this entities name to not be prefixed with `The`.

### `SingleFlag`

_Flag set if the entity should be referred to plurally._

When this flag is set, it will cause any references to this entity to be considered plural.

### `AutoPartyFlag`

_Flag set if the entity is invisible and untargetable._

When this flag is set, the entity will be invisible and untargeted by normal means. If this entity attacks a player, then their auto-targeting will allow this entity to automatically be targeted.

## Structure Fields (`flags3_t`)

### `TrustFlag`

_Flag set if the entity is a Trust._

When this flag is set, it will cause the client to show the target sub-menu when clicking on the entity. If this flag is not set, and the entity is a trust, then they will be recalled immediately.

### `LfgMasterFlag`

_Unused._

This bit is unused.

### `PetNewFlag`

_Flag set if the entity is a pet being spawned._

This flag changes the spawn animation used when the entity is first popped.

### `unknown_0_3`

_Unknown._

The purpose of this flag is unknown at this time.

### `MotStopFlag`

_Flag set if the entity motion scheduler should be paused._

This flag is used when the entity is petrified or terrorized. It will cause the entity to pause their current animation and freeze in place. _(Additional flags are set to lock the entity in place.)_

### `CliPriorityFlag`

_Flag set if the entity is considered priority._

When this flag is set, it will override the internal limit when rendering entities to screen. Usually the client will loop through the entity array and draw each valid entity until a limit is reached. This flag will allow the entity to be drawn regardless of this limit being reached each frame.

### `PetFlag`

_Flag set if the entity is a pet._

### `OcclusionoffFlag`

_Flag set if the entity should not be included in the entity occlusion testing._

### `BallistaTeam`

_The entities current Ballista team id._

This value is used to set the entities current Ballista team id under certain conditions.

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

_This value can also be used for other purposes such as Monstrosity._

### `unknown_2_3`

_Unknown._

This flag is used for a language-specific purpose. _(Generally used with the French and German language strings.)_

### `unknown_2_4`

_Unused._

This bit is unused.

### `SilenceFlag`

_Flag set if the entity should not make footstep sounds when moving._

### `unknown_2_6`

_Unknown._

The purpose of this flag is unknown at this time.

### `NewCharacterFlag`

_Unknown._

The purpose of this flag is unknown at this time.

### `MentorFlag`

_Flag set if the entity is an A.M.A.N. Liaison._

When this flag is set, it will change the entities name icon to the tutorial 'i' icon.

### `unknown_3_1`

_Unknown._

When this flag is set, it will cause the entities animations to use different values. For example, this will override the manner in which the entity will sit in a chair.

### `unknown_3_2`

_Unknown._

When this flag is set, it will cause the entity to use `SubAnimation` as 3 bits instead of 2 by default. _(This is also true if the `MonsterFlag` is set.)_ This flag is also used when the entity is being popped/spawned.

### `unknown_3_3`

_Unknown._

This flag has multiple purposes depending on the entity type it is being used with. The following has been observed with this flag:

  - Certain entities will become untargetable. (If the client targets the entity, their target will be unset immediately after.)
  - Certain entities will no longer render on the clients compass.
  - Certain entities name will be hidden above their head.

### `unknown_3_4`

_Unknown._

When this flag is set, it will cause the entity to become non-blocking. This means that the local client can immediately pass through the entity without collision happening. _(This flag causes the entity to be skipped during the `XiControlActor::CheckContactActor` call.)_

### `unknown_3_5`

_Unknown._

When this flag is set, it will cause the entities health bar to be hidden and the name above their head to not be rendered.

### `unknown_3_6`

_Unknown._

When this flag is set, the entity will not be drawn on the clients compass.

### `unknown_3_7`

_Unknown._

When this flag is set, it will cause the entity to become half-transparent. _(This flag overrides the alpha value used during the clients `XiSkeletonActor::CalcAlphaByDistance` call on the entity to be `0.5`.)_

## Additional Information

The server uses this packet to handle updates related to non-player entities (NPCs). This includes their initial spawning, despawning and various general updates to other information about the entity. The packets `SendFlg` and `SubKind` fields are used to tell the client which fields in the packet are actually populated and will be used. While this packet shares similar structures to the `0x000D` player entity packet, its fields are used differently. This packet also does not have a single full-format that all entities will use, instead there are multiple ways the rest of the packets `Data` information is populated based on the `SendFlg`, `SubKind` and overall entity type. _(NPC, Monster, Door, Ship, etc.)_

## Additional Information - `SubKind` - `0`

When this `SubKind` is used, the packet is treated as a means to update a fixed-model NPC.

The packet `Data` will contain the following:

```cpp
struct packet_data_t
{
    uint16_t model_id;
};
```

The client will perform the following checks and adjustments to the entity:

```cpp
    // Check if the entity is wearing a costume..
    if ((entity->Render.Flags0 & 0x20) == 0)
    {
        // Not wearing a costume, this is a modeled NPC entity..
        entity->Type = 2;
    }

    // Unknown flag check..
    if ((entity->Render.Flags3 & 0x40) != 0)
        goto LABEL_1;

    // Read the model id from the packet data..
    const auto mid = *reinterpret_cast<uint16_t*>(pkt->Data);

    // Check if the model id is a Shadow..
    if (mid < 531 || mid > 552)
    {
        if (!entity->Race)
            goto LABEL_1;
        entity->Race = 0;
    }
    else
    {
        if (entity->Race == 3)
            goto LABEL_1;
        entity->Race = 3;
    }

    // Blink the entity; refreshing it visually..
    entity->Render.Flags0 |= 1;

LABEL_1:

    // Check if the entities model id is different than what the packet contains..
    if (entity->Look.Hair != mid)
    {
        // Note: When a modeled NPC is not using a costume, then their
        // model id is stored in the `Look.Hair` property of the entity!

        entity->ModelUpdateFlags |= 1;
        entity->Look.Hair = mid;
    }

    // Check if the model is a crystal, teleport, elavator, etc. of some sort.. (Home Point, Telepoint, etc.)
    if (mid >= 50 && mid <= 59)
    {
LABEL_2:
        // Hide the entity name..
        entity->Render.Flags2 |= 8;
        goto LABEL_3;
    }

    // Check if the model is a Waypoint or Resource Node..
    if (mid < 2420 || mid > 2424)
    {
        // Unknown model id check.. (Invisible model type..)
        if (mid >= 1847 && mid <= 1862)
            goto LABEL_2;

        // Check if the model is a Conflux..
        if (mid >= 2490 && mid <= 2494)
            goto LABEL_2;

        // Check if the model is a type of treasure chest/coffer..
        if (mid < 960 || mid > 969)
        {
            // Check if the model is a type of flag.. (Nation Flags, Beastman Flag, etc..)
            if (mid >= 814 && mid <= 817)
                goto LABEL_2;

            // Check if the model is a special treasure chest/coffer.. (Dark purple/blood red, etc.)
            if (mid >= 2425 && mid <= 2429)
                goto LABEL_2;

            // Unknown model id check.. (Invisible model type..)
            if (mid < 2495 || mid > 2499)
            {
LABEL_3:
                // Check if model id is a Wyvern.. (DRG pet model..)
                if (mid == 24)
                {
                    entity->Unknown0002 = 1;
                    return 1;
                }

                return 1;
            }
        }
    }

    goto LABEL_1;
```

## Additional Information - `SubKind` - `1`

When this `SubKind` is used, the packet is used to equip an NPC entity with normal player equippable gear. The packet will contain additional model id information to update the entities `Look` data. _(**Note:** The `Data` information will only be populated if the `SendFlg::Model` flag is set!)_

The packet `Data` will contain the following:

```cpp
struct packet_data_t
{
    uint16_t GrapIDTbl[9];
};
```

The client will perform the following checks and adjustments to the entity:

```cpp
    if ((entity->Render.Flags3 & 0x40000000) != 0)
    {
        if ((entity->Render.Flags0 & 0x20) == 0 && !entity->MonstrosityFlag)
            entity->Type = 0;
    }
    else if ((entity->Render.Flags0 & 0x20) == 0 && !entity->MonstrosityFlag)
    {
        entity->Type = 1;
    }

    if (pkt->SendFlg.Model == 0)
        return 1;

    // Update the entities model visuals..
    FUNC_SetPctypeEquip(pkt->ActIndex, pkt->Data, 1);
    return 1;
```

## Additional Information - `SubKind` - `2`

When this `SubKind` is used, the packet is used to update and/or prepare a door.

The packet `Data` will contain the following:

```cpp
struct packet_data_t
{
    uint16_t unused;
    uint32_t DoorId;
};
```

The client will perform the following checks and adjustments to the entity:

```cpp
    if (entity->Race)
    {
        // Refresh the entity to have no race..
        entity->Race = 0;
        entity->Flags0 |= 1;
    }

    entity->Type = 3;

    // Check if the entity has a model set..
    if (entity->Look.Hair)
    {
        // Remove the model and refresh the entity..
        entity->ModelUpdateFlags |= 1;
        entity->Look.Hair = 0;
    }

    entity->DoorId = pkt->Data.DoorId;

    // Hide the entity name..
    entity->Render.Flags2 |= 8;
    return 1;
```

## Additional Information - `SubKind` - `3`

When this `SubKind` is used, the packet is used to update and/or prepare an elevator or similar movable entities.

The packet `Data` will contain the following:

```cpp
struct packet_data_t
{
    uint16_t unused;
    uint32_t DoorId;
    uint32_t Time;
    uint32_t EndTime;
};
```

The client will perform the following checks and adjustments to the entity:

```cpp
    if (entity->Race)
    {
        // Refresh the entity to have no race..
        entity->Race = 0;
        entity->Flags0 |= 1;
    }

    entity->Type = 4;

    // Check if the entity has a model set..
    if (entity->Look.Hair)
    {
        // Remove the model and refresh the entity..
        entity->ModelUpdateFlags |= 1;
        entity->Look.Hair = 0;
    }

    entity->DoorId          = pkt->Data.DoorId;
    entity->ModelStartTime  = pkt->Data.Time;
    entity->ModelTime       = pkt->Data.Time + pkt->Data.EndTime;

    // Hide the entity name..
    entity->Render.Flags2 |= 8;
    return 1;
```

## Additional Information - `SubKind` - `4`

When this `SubKind` is used, the packet is used to update and/or prepare an airship, boat, or similar kinds of entities. _(**Note:** Some gardening plants use this `SubKind` as well!)_

The packet `Data` will contain the following:

```cpp
struct packet_data_t
{
    uint16_t unused;
    uint32_t DoorId;
    uint32_t Time;
    uint32_t EndTime;
};
```

The client will perform the following checks and adjustments to the entity:

```cpp
    if (entity->Race)
    {
        // Refresh the entity to have no race..
        entity->Race = 0;
        entity->Flags0 |= 1;
    }

    entity->Type = 5;

    // Check if the entity has a model set..
    if (entity->Look.Hair)
    {
        // Remove the model and refresh the entity..
        entity->ModelUpdateFlags |= 1;
        entity->Look.Hair = 0;
    }

    // Refresh the entity visually if the door id does not match..
    if (entity->DoorId && entity->DoorId != pkt->Data.DoorId)
        entity->Render.Flags0 |= 1;

    entity->DoorId          = pkt->Data.DoorId;
    entity->ModelStartTime  = pkt->Data.Time;
    entity->ModelTime       = pkt->Data.Time + pkt->Data.EndTime;

    // Hide the entity name..
    entity->Render.Flags2 |= 8;
    return 1;
```

## Additional Information - `SubKind` - `5`

When this `SubKind` is used, the packet is used to update and/or prepare an unknown type of entity.

The packet `Data` will contain the following:

```cpp
struct packet_data_t
{
    uint16_t model_id;
};
```

The client will perform the following checks and adjustments to the entity:

```cpp
    entity->Type = 6;

    // Calculate an unknown model visual id..
    auto val = pkt->Data.model_id | (0x90 << 8);

    // Update the entities model visuals if it does not match..
    if (entity->Look.Unknown0000 != val)
    {
        entity->ModelUpdateFlags |= 0x20;
        entity->Look.Unknown0000 = val;
    }

    if (!entity->ModelUpdateFlag)
        return 1;

    entity->Look.Hair = 0;
    return 1;
```

## Additional Information - `SubKind` - `6`

When this `SubKind` is used, the packet is used to update and/or prepare an unknown type of entity. _(**Note:** This is also used with Automatons to set their visual model as well as other various entities such as certain Lamia and Trolls.)_

The packet `Data` will contain the following:

```cpp
struct packet_data_t
{
    uint16_t model_id;
};
```

The client will perform the following checks and adjustments to the entity:

```cpp
    entity->Type = 7;

    // Calculate an unknown model visual id.. (Used with Automaton models..)
    auto val = pkt->Data.model_id | (0x90 << 8);

    // Update the entities model visuals if it does not match..
    if (entity->Look.Unknown0000 != val)
    {
        entity->ModelUpdateFlags |= 0x20;
        entity->Look.Unknown0000 = val;
    }

    if (entity->ModelUpdateFlags)
        entity->Look.Hair = 0;

    // Check if the model id is an Automaton..
    if (pkt->Data.model_id >= 1976 && pkt->Data.model_id <= 1995)
        entity->Unknown0002 = 2;
    if (pkt->Data.model_id < 2003 || pkt->Data.model_id > 2010)
        return 1;

    entity->Unknown0002 = 2; // Unknown flag; set when the model id is used for Automatons..
    return 1;
```

## Additional Information - `SubKind` - `7`

When this `SubKind` is used, the packet is used to update and/or prepare an unknown type of entity with normal player equippable gear. The packet will contain additional model id information to update the entities `Look` data. _(**Note:** The `Data` information will only be populated if the `SendFlg::Model` flag is set!)_

The packet `Data` will contain the following:

```cpp
struct packet_data_t
{
    uint16_t GrapIDTbl[9];
};
```

The client will perform the following checks and adjustments to the entity:

```cpp
    entity->Type = 8;

    if (pkt->SendFlg.Model == 0)
        return 1;

    // Update the entities model visuals..
    FUNC_UnknownEquipmentSet(pkt->ActIndex, pkt->Data);
    return 1;
```

_The function `FUNC_UnknownEquipmentSet` is used to update the entities full visual information. It will update their `Race` as well as all of their `Look` model visuals where values do not match their current visuals. This function works similar to how `FUNC_SetPctypeEquip` works, except it does not have the same level of checks for PC vs. NPC entities or extra gender handling._

## Additional Information - Entity Name

This packet can be used to rename an entity on the fly based on certain conditions. This will mainly depend on the `SendFlg` and `SubKind` values being used with the packet. There are also additional conditions that must be met in order for the renaming to work. The client has 3 sections of entities which hold expected entity types:

| Starting Index | Ending Index | Purpose |
| :---: | :---: | --- |
| `0`   | `1023` | `Static NPCs` |
| `1024`| `1792` | `Players` |
| `1792`| `2303` | `Spawnables` _(Pets, Trusts, etc.)_ |

The client contains DAT files that hold the expected names for entities that fall under the `Static NPCs` range of target indexes. When the client enters a zone, it will load the DAT file that contains the names of the static entities for the current zone. It is not common for the server to attempt to rename these entities. Entities that fall under the `Spawnables` range will be named through this packet. _(Players are not affected by this packet.)_

The first manner in which this packet handler will rename entities is for `Spawnable` entities. The entity name is stored at offset `0x34` within the packet during this condition. The handler section for this looks like:

```cpp
// Packet Data Layout
struct packet_data_t
{
    uint16_t    data;       // SendFlg dependent data.
    uint8_t     Name[16];   // The entity name.
};

//
// Handler block..
//

// snipped prior code..

const auto skind = pkt->SubKind;
if ((skind < 2 || skind == 7 || skind == 5 || skind == 6) && pkt->SendFlg.Name && pkt->ActIndex >= 1792)
{
    enQueStrCpy(name_buffer, 0x10, pkt->Data.Name, 0x38, pkt->size);
    XiAtelBuff::SetName(entity, name_buffer);

    // snipped..
}

// snipped..
```

The next manner in which the name is handled is within a second part of the handler with other conditions. During this block of the handler, the entity name is stored at offsets `0x44` or `0x35` within the packet depending on the conditions. The handler section for this looks like:

```cpp
// Packet Data Layout (Usage 1)
struct packet_data_t
{
    uint8_t     data[18];   // SendFlg dependent data.
    uint8_t     Name[16];   // The entity name.
};

// Packet Data Layout (Usage 2)
// Note: This is intentionally unaligned; be careful when defining this structure to pack accordingly!
struct packet_data_t
{
    uint16_t    data;       // SendFlg dependent data.
    uint8_t     HasName;    // Flag set to 1 if the entity name field is populated.
    uint8_t     Name[16];   // The entity name.
};

//
// Handler block..
//

    // snipped prior code..

    if (skind == 1)
    {
        // Handle spawnable renames..
        if (pkt->SendFlg.Name2 && pkt->ActIndex >= 1792)
        {
            // Data Usage 1
            enQueStrCpy(name_nuffer, 0x10, pkt->Data.Name, 0x48, pkt->size);
            XiAtelBuff::SetName(entity, name_buffer);

            goto OTHER_PROCESSING;
        }

        // Handle other renames..
        if (pkt->SendFlg.Name2)
        {
            // Skip over player/spawnable renames..
            if (pkt->ActIndex >= 1024)
                goto LABEL_4;

            if (pkt->Data.Name[0] > ' ')
            {
                // Data Usage 1
                enQueStrCpy(name_nuffer, 0x10, pkt->Data.Name, 0x48, pkt->size);
                XiAtelBuff::SetName(entity, name_buffer);

                goto OTHER_PROCESSING;
            }
        }
    }

    // Allow for static entity renaming.. (Data Usage 2)
    if (pkt->ActIndex < 1024 && pkt->SendFlg.Name && pkt->Data.HasName == 1)
    {
        name_buffer = pkt->Data.Name;
        if (pkt->Data.Name[0] > ' ')
        {
            XiAtelBuff::SetName(entity, name_buffer);
            goto OTHER_PROCESSING;
        }
    }

LABEL_4:
    if (pkt->ActIndex >= 1792)
        goto OTHER_PROCESSING;

    name_buffer = GetMonsterName2(pkt->UniqueNo);
    if (!name_buffer)
        name_buffer = "NPC";

    XiAtelBuff::SetName(entity, name_buffer);
    goto OTHER_PROCESSING;
```

## Additional Information - Other Processing

Along with the above information in how certain parts of this packet is handled, there is additional processing that will happen for other conditions.

When this packet is first received, the handler will check to see if an entity instance exists for the given `ActIndex`. If there is no entity created yet, then a new `XiAtelBuff` instance will be created for the entity. The client will then check to ensure the server id for the entity at the given index matches the incoming `UniqueNo` within the packet. If these do not match, then the client will delete and recreate the entity. It will also reset all current stale entities to ensure no other entity has the same server id. The client will then check if the entity is within/part of an event and also determine if the incoming `SendFlg` and `SubKind` values will require another call to reset all stale entities.

_**Note:** This is pseudo code; not meant to be an exact match._
```cpp
// Check if the target index is a valid NPC or spawnable entity index:
// pkt->ActIndex < 1024 || (pkt->ActIndex >= 1792 && pkt->ActIndex < 2304)
if (!IsNpcOrSpawnedEntity(pkt->ActIndex))
    return 0;

// Create a new entity instance for the NPC if it does not already exist..
if (!ActBuffPtr[pkt->ActIndex])
    ActBuffPtr[pkt->ActIndex] = new XiAtelBuff();

// Obtain the entity..
auto entity = ActBuffPtr[pkt->ActIndex];

// Reinitialize the entity object if the NPC server id does not match the previous existing value..
if (pkt->UniqueNo != entity->ServerId)
{
    // Fully reset the entity if it had a previous server id..
    if (entity->ServerId)
    {
        entity->EventDelete();
        entity->ObjectDelete();
        entity->Reset(0);
    }

    // Cleanup all stale entities..
    XiAtelBuff::ResetStaleEntities(pkt->ActIndex, pkt->UniqueNo);
}

// Check if the entity is part of an event..
auto event_flag = (entity->Render.Flags6 & 0x80000000) != 0

// Override the SendFlg value if needed..
auto send_flag  = pkt->SendFlg;
if (pkt->SubKind == 1 || send_flag = 7, pkt->SubKind == 7)
    send_flag = 0x17; // Position, ClaimStatus, General, Model

// Cleanup all stale entities..
if ((pkt->SendFlag & send_flag) == send_flag)
    XiAtelBuff::ResetStaleEntities(pkt->ActIndex, pkt->UniqueNo);

const auto skind = pkt->SubKind;
if ((skind < 2 || skind == 7 || skind == 5 || skind == 6) && pkt->SendFlg.Name && pkt->ActIndex >= 1792)
{
    enQueStrCpy(name_buffer, 0x10, pkt->Data.Name, 0x38, pkt->size);
    XiAtelBuff::SetName(entity, name_buffer);

// This is the label jumped to in several of the above blocks of information above..
OTHER_PROCESSING:

    entity->TargetIndex     = pkt->ActIndex;
    entity->ServerId        = pkt->UniqueNo;
    entity->Render.Flags1   &= ~0x1000;

    // Check if the entity is being despawned and is not part of an event..
    if (pkt->SendFlg.Despawn && !event_flag)
    {
        if (skind == 5 || skind == 6 || skind == 7)
        {
            entity->Render.Flags0       &= ~1;
            entity->Render.Flags3       &= 0xFFFFFFBF;
            entity->Race                = 0;
            entity->ModelUpdateFlags    = 0;

            std::memset(&entity->Look.Hair, 0x00, 32);
        }

        entity->Render.Flags1   &= 0xFFFFFFF1;
        entity->Render.Flags1   &= (entity->Render.Flags1 & 0xFFFFFF8F) & 0x0F | 0x80;
        entity->Render.Flags1   &= entity->Render.Flags1 & 0xF000 | 0x800;
        entity->Render.Flags1   |= 0x1000u;

        entity->UpdateMask      = 0;

        if ((pkt->Render.Flags0 & 0x100) == 0)
        {
            pkt->Render.Flags0 |= 0x4000;
            return 1;
        }

        return 1;
    }

    if (!entity->UpdateMask)
    {
        if (!event_flag)
        {
            auto reset = false;
            if (skind == 1 || skind == 7)
                reset = (send_flag & 0x17) == 0x17;
            else
                reset = (send_Flag & 0x07) == 0x07;

            if (!reset)
            {
                entity->EventDelete();
                entity->ObjectDelete();
                entity->Reset(0);
                entity.Render.Flags3 |= 0x8000;

                return 0;
            }
        }

        entity->UpdateMask = send_flag;
    }

    // snipped..
}
```

Following this block of code, the handler will then continue into processing the various parts of the packet based on the `SendFlg` values that are set.

_**Note:** This is pseudo code; not meant to be an exact match._
```cpp
const auto flag = event_flag && (entity->Render.Flags7 & 1) != 0;

// Update the entities position information..
if ((pkt->SendFlg & 1) != 0 && !flag)
{
    entity->Movement.Move.X = pkt->x;
    entity->Movement.Move.Y = pkt->y;
    entity->Movement.Move.Z = pkt->z;

    // Calibrate the entity if their GroundFlag is not set..
    if ((pkt->UniqueNo & 0xFF000000) == 0x10000000 && (pkt->Flags0 & 0x8000) == 0)
    {
        if (skind != 2 && skind != 3 && VCalibrate(g_pTsZoneMap, pkt->x, pkt->z, pkt->y, &pos) == 1)
            pkt->z = pos.x
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

    if (!event_flag)
    {
        // snipped flag updates..
    }
}

// Update the entities claim status information..
if ((pkt->SendFlg & 2) != 0 && !flag)
{
    entity->ClaimStatus = pkt->BtTargetID;

    // snipped flag updates..
}

// Update the entities general information..
if ((pkt->SendFlg & 4) != 0)
{
    // snipped flag updates..

    entity->SetChocoboIndex((pkt->Flags1 >> 5) & 7);

    // snipped flag updates..

    entity->HPPercent = pkt->Hpp;

    if ((entity->Render.Flags0 & 0x02) == 0)
    {
        if ((pkt->Flags3 & 1) == 0 || pkt->server_status != 33 && pkt->status_server != 47 && !XiActor::StatusCheck_IsSitChair(entity, pkt->server_status))
            entity->StatusServer = pkt->server_status;
    }

    // snipped flag updates..

    entity->LinkshellColor = 0;

    // snipped flag updates..

    SetEntityNamedFlag(pkt->ActIndex, pkt->Flags2.NamedFlag);
    SetEntitySingleFlag(pkt->ActIndex, pkt->Flags2.SingleFlag);

    // snipped flag updates..
    // snipped misc checks that result in just flag updates..

    entity->ModelHitboxSize = pkt->Flags2.g * 0.1;

    entity->SetGeoIndi(pkt->Flags2.b & 0x0F, 0, 0);

    // snipped flag updates..
    // snipped calls to setup gender specific language handling..
}
```

_The above code blocks are shortened examples of how the handler for this packet works to demonstrate the means in which the `SendFlg` values are used, in what order, and how additional calls are made to prepare, reinitialize and otherwise destroy entities. Several chunks of code have been stripped out to save space._

## Additional Information - Example Updates (`Shikigami Weapon`)

There are a handful of entities throughout the world of Vana'diel that have special conditions in how they are handled. One example of these entities is `Shikigami Weapon`. This notorious monster has a unique manner of spawning into view and becoming claimable as well as a special case on how it goes unclaimed when the player/party fighting it has wiped.

When `Shikigami Weapon` spawns, it will spawn fully invisible. This kind of invisible makes it not targetable. Instead, he is intended to be found using `Wide Scan` or by just spam casting magic until the player is close enough to aggro him. While he is roaming around, he will send position update packets such as:

```
0000h: 0E 1C 5E 01 C7 A0 07 01 C7 00 01 FD 6A BC 31 C2  ..^.Ç ..Ç..ýj¼1Â
0010h: 00 00 D0 C0 D1 22 11 41 01 19 00 00 28 28 00 00  ..ÐÀÑ".A....((..
0020h: 03 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
0030h: 00 00 DE 01 00 00 00 00                          ..Þ.....
```

_The following values of interest are populated in this packet:_

  - `SendFlg`: `0x01` [`Position`]
  - `Flags0`
    - `MovTime`: _Will be set while moving._
  - `Flags1`
    - `MonsterFlag`: `1`
    - `HideFlag`: `1`
  - `SubKind`: `0x00` _Uses a fixed-model id update._
  - `Data`: `478` _Set to the model id for Shikigami Weapon; Evil Weapon._

When the player successfully aggros `Shikigami Weapon`, he will become visible and unclaimed. The packet sent for this looks like:

```
0000h: 0E 1C 75 01 C7 A0 07 01 C7 00 07 74 35 5E CA C1  ..u.Ç ..Ç..t5^ÊÁ
0010h: 00 00 D0 C0 FC A9 4F 41 1F 03 00 00 28 28 64 01  ..ÐÀü©OA....((d.
0020h: 01 85 02 00 00 16 00 00 00 00 08 00 00 00 00 00  .…..............
0030h: 00 00 DE 01 00 00 00 00                          ..Þ.....
```

_The following values of interest are populated in this packet:_

  - `SendFlg`: `0x07` [`Position`, `ClaimStatus`, `General`]
  - `Flags0`
    - `MovTime`: _Will be set while moving._
  - `Flags1`
    - `MonsterFlag`: `1`
    - `CliPosInitFlag`: `1`
    - `GraphSize`: `2` _Shikigami Weapon is a largest-sized Evil Weapon model._
    - `Gender`: `1` _He a big boi!_
    - `LinkShellFlag`: `1`
  - `Flags2`
    - `g`: `22`
  - `Flags3`
    - `unknown_2_3`: `1`
  - `SubKind`: `0x00` _Uses a fixed-model id update._
  - `Data`: `478` _Set to the model id for Shikigami Weapon; Evil Weapon._

While fighting, general updates will continue as `Shikigami Weapon` either moves around, has various information updates, such as health, etc. If the player, or party, that has claim on `Shikigami Weapon` fully wipes, then another special case handling will happen. Unlike normal monsters that simply go unclaimed and can be immediately claimed by another player/party, `Shikigami Weapon` will instead become unclaimable with a green name. Eventually, he will cast `Invisible` on himself and despawn back to the fully hidden mode that he spawns as.

When he becomes unclaimable with a green name, the packet sent will look like this:

```
0000h: 0E 1C D9 01 C7 A0 07 01 C7 00 07 74 35 5E CA C1  ..Ù.Ç ..Ç..t5^ÊÁ
0010h: 00 00 D0 C0 FC A9 4F 41 BD 03 00 00 28 28 64 00  ..ÐÀü©OA½...((d.
0020h: 00 85 02 00 00 16 00 00 00 00 08 00 00 00 00 00  .…..............
0030h: 00 00 DE 01 00 00 00 00                          ..Þ.....
```

_The following values of interest are populated in this packet:_

  - `SendFlg`: `0x07` [`Position`, `ClaimStatus`, `General`]
  - `Flags0`
    - `MovTime`: _Will be set while moving._
  - `Flags1`
    - `MonsterFlag`: `0` _This is now set to 0, causing him to have a green name._
    - `CliPosInitFlag`: `1`
    - `GraphSize`: `2`
    - `Gender`: `1`
    - `LinkShellFlag`: `1`
  - `Flags2`
    - `g`: `22`
  - `Flags3`
    - `unknown_2_3`: `1`
  - `SubKind`: `0x00` _Uses a fixed-model id update._
  - `Data`: `478` _Set to the model id for Shikigami Weapon; Evil Weapon._

Eventually, `Shikigami Weapon` will cast `Invisible` on himself. This will cast the normal spell, however it will not cause the normal `Invisible` effect that happens on players. Instead, this will cause him to fully despawn and go back to his original fully invisible spawned state requiring players to have to begin using `Wide Scan` _(or magic cast aggro)_ again to find him. When this happens, the following packet is sent:

```
0000h: 0E 1C F2 01 C7 A0 07 01 C7 00 07 74 35 5E CA C1  ..ò.Ç ..Ç..t5^ÊÁ
0010h: 00 00 D0 C0 FC A9 4F 41 BD 03 00 00 28 28 64 00  ..ÐÀü©OA½...((d.
0020h: 06 85 0A 20 00 16 00 00 00 00 08 18 00 00 00 00  .…. ............
0030h: 00 00 DE 01 00 00 00 00                          ..Þ.....
```

_The following values of interest are populated in this packet:_

  - `SendFlg`: `0x07` [`Position`, `ClaimStatus`, `General`]
  - `Flags0`
    - `MovTime`: _Will be set while moving._
  - `Flags1`
    - `MonsterFlag`: `0` _This remains set to 0 to keep him unclaimable._
    - `HideFlag`: `1` _Sets himself back to fully invisible._
    - `SleepFlag`: `1`  _Sets his scheduler to stop; also causing him to become invisible._
    - `CliPosInitFlag`: `1`
    - `GraphSize`: `2`
    - `Gender`: `1`
    - `LinkShellFlag`: `1`
    - `TargetOffFlag`: `1` _Prevents himself from being targeted._
    - `InvisFlag`: `1` _Prevents himself from drawing on the clients compass._
  - `Flags2`
    - `g`: `22`
  - `Flags3`
    - `unknown_2_3`: `1`
    - `unknown_3_3`: `1` _Used to hide the entity in multiple manners._
    - `unknown_3_4`: `1` _Used to remove the entity collision._
  - `SubKind`: `0x00` _Uses a fixed-model id update._
  - `Data`: `478` _Set to the model id for Shikigami Weapon; Evil Weapon._

Once `Shikigami Weapon` has returned to his invisible spawned state, he will become roaming again. He will remain unclaimable for a period of time when this happens. The server will send a few full update packets along with some general position update packets. An example of a full update packet during this state looks like:

```
0000h: 0E 24 00 02 C7 A0 07 01 C7 00 0F 7E B6 F3 E3 C1  .$..Ç ..Ç..~¶óãÁ
0010h: 00 00 D0 C0 B6 F3 4D 41 99 00 00 00 28 28 64 00  ..ÐÀ¶óMA™...((d.
0020h: 03 85 0A 00 00 14 00 00 00 00 08 18 00 00 00 00  .…..............
0030h: 00 00 DE 01 53 68 69 6B 69 67 61 6D 69 57 65 61  ..Þ.ShikigamiWea
0040h: 70 6F 00 00 00 00 00 00                          po......
```

_The following values of interest are populated in this packet:_

  - `SendFlg`: `0x07` [`Position`, `ClaimStatus`, `General`, `Name`]
  - `Flags0`
    - `MovTime`: _Will be set while moving._
  - `Flags1`
    - `MonsterFlag`: `1`
    - `HideFlag`: `1`
    - `CliPosInitFlag`: `1`
    - `GraphSize`: `2`
    - `Gender`: `1`
    - `LinkShellFlag`: `1`
    - `TargetOffFlag`: `1`
  - `Flags2`
    - `g`: `20`
  - `Flags3`
    - `unknown_2_3`: `1`
    - `unknown_3_3`: `1`
    - `unknown_3_4`: `1`
  - `SubKind`: `0x00`
  - `Data`
    - `uint16_t`: `478` _Set to the model id for Shikigami Weapon; Evil Weapon._
    - `uint8_t[]`: `Shikigami Weapon` _His name._

Once a period of time has passed, he will go back to a claimable state while invisible, just as he was when he was originally invisible after first spawning.
