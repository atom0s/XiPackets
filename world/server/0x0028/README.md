# `GP_SERV_COMMAND_BATTLE2`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_BATTLE2` |
| **Client Handler**        | `RecvBattleCalc2` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0028` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the server to inform clients of action related events. This includes things such as:

  - Entities attacking another entity. _(Basic attacks, ranged attacks, etc.)_
  - Entities casting a spell.
  - Entities using an item.
  - Entities using an ability, skill, weaponskill, etc.
  - etc.

Each event has different types of responses that this packet can contain. For example, casting a spell can:

  - Send a packet marking the start of a spell cast happening.
  - Send a packet marking the end of the spell cast happening. _(Successful cast.)_
  - Send a packet marking the spell was interrupted.

The client uses this packet to do various tasks related to actions such as:

  - Playing animations related to the action. _(ie. causing an entity to start casting a spell, use an item, swing their weapon, etc.)_
  - Playing additional animations, such as 'proc' effects. _(ie. Enspells, HP/MP drain effects on weapons or sambas, etc.)_
  - Playing additional animations, such as 'react' effects. _(ie. Spikes)_
  - Displaying chat messages related to the action. _(ie. `'X' starts casting 'Y'.`, `'X' uses Sneak Attack.`)_
  - Displaying floating damage numbers if the player has that configuration enabled.
  - Causing knockback effects, pushing entities backward.
  - etc.

## Packet Layout

This packet does not follow a traditional structured layout.

For more information on this packets layout, please see: **[Reversing Information](REVERSING.md)**

Once the client has unpacked and parsed the content of this packet, its information is stored into a block of structures.\
Those structures are defined as follows:

```cpp
// PS2: BattleResult
struct BattleResult
{
    uint32_t        miss;           // PS2: miss
    uint16_t        kind;           // PS2: kind
    uint16_t        sub_kind;       // PS2: sub_kind
    uint16_t        info;           // PS2: info
    uint16_t        scale;          // PS2: scale
    uint32_t        value;          // PS2: value
    uint32_t        message;        // PS2: message
    uint32_t        bit;            // PS2: bit
    uint16_t        proc_kind;      // PS2: proc_kind
    uint16_t        proc_info;      // PS2: proc_info
    uint32_t        proc_value;     // PS2: proc_value
    uint16_t        proc_message;   // PS2: proc_message
    uint16_t        react_kind;     // PS2: react_kind
    uint16_t        react_info;     // PS2: react_info
    uint16_t        react_value;    // PS2: react_value
    uint16_t        react_message;  // PS2: react_message
};

// PS2: BattleTarget
struct BattleTarget
{
    XiAtelBuff**    m_ppTargetAtel; // PS2: m_ppTargetAtel
    uint32_t        m_uID;          // PS2: m_uID
    uint32_t        result_sum;     // PS2: result_sum
    BattleResult    result[8];      // PS2: result
};

// PS2: MainToCalc
struct MainToCalc
{
    XiAtelBuff**    m_ppCasterAtel;         // PS2: m_ppCasterAtel
    uint32_t        m_uID;                  // PS2: m_uID
    uint32_t        cmd_no;                 // PS2: cmd_no
    uint32_t        cmd_arg[1];             // PS2: cmd_arg
    uint32_t        info;                   // PS2: info
    uint32_t        res_sum;                // PS2: (New; did not exist.)
    uint32_t        trg_sum;                // PS2: trg_sum
    BattleTarget    target[64];             // PS2: target
    char            m_bMessagePutFlag[64];  // PS2: m_bMessagePutFlag
}
```

_**Note:** The packet data **DOES NOT** directly map/translate to these structures! Please review the reversing information linked above to understand how this packet is fully and properly parsed/unpacked!_

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`MainToCalc`)

### `m_ppCasterAtel`

_The actor pointer of the entity that caused the action. (The client refers to this as the 'caster'.)_

**Note:** This value is not part of the packet data. It is populated using the `m_uID` value after its read.

### `m_uID`

_The server id of the entity that caused the action._

### `cmd_no`

_The command number of the action._

  - `0` - None
  - `1` - Basic Attack
  - `2` - Range Attack (Finish)
  - `3` - Skill (Finish) _(Weapon Skills)_
  - `4` - Magic (Finish) _(This is also sent if a weapon skill fails due to being too far from the target.)_
  - `5` - Item (Finish)
  - `6` - Ability (Finish) _(Dancer Flourish)_
  - `7` - Skill (Start) _(Monster Skills, Weapon Skills)_
  - `8` - Magic (Start)
  - `9` - Item (Start) _(Also sent if the item use is interrupted.)_
  - `10` - Ability (Start)
  - `11` - Monster Skill (Finish), Trust Attacks _(ie. Shantotto Melee Attack)_
  - `12` - Range Attack (Start)
  - `13` - Unknown
  - `14` - Dancer Ability (Flourish, Jig, Samba, Step, Waltz, etc.)
  - `15` - Rune Fencer Effusion/Ward

_**Note:** DNC job abilities are a mix between `cmd_no` 6 and `cmd_no` 14, depending on the type, such as Flourishes._

### `cmd_arg`

_The command argument of the action._

This value differs based on the current `cmd_no` of the action.

  - `cmd_no: 0` - _None_
  - `cmd_no: 1` - `atk0` _(812348513)_
  - `cmd_no: 2` - `shlg` _(1735157875)_
  - `cmd_no: 3` - _The value will be equal to the weapon skill id._
  - `cmd_no: 4` - _The value will be equal to the spell id._
  - `cmd_no: 5` - _The value will be equal to the item id._
  - `cmd_no: 6` - _The value will be equal to the ability id._
  - `cmd_no: 7` - _The value will depend on the action state:_
    - Use: `cate` _(1702125923)_
    - Interrupt: `spte` _(1702129779)_
  - `cmd_no: 8` - _The value depends on the type of magic being casted, and the action state:_
    - White Magic: `cawh` _(1752654179)_, Interrupt: `spwh` _(1752658035)_
    - Black Magic: `cabk` _(1801609571)_, Interrupt: `spbk` _(1801613427)_
    - Blue Magic: `cabl` _(1818386787)_, Interrupt: `spbl` _(1818390643)_
    - Ninjutsu: `canj` _(1785618787)_, Interrupt: `spnj` _(1785622643)_
    - Songs: `caso` _(1869832547)_, Interrupt: `spso` _(1869836403)_
    - Summon: `casm` _(1836278115)_, Interrupt: `spsm` _(1836281971)_
    - Geomancy: `cage` _(1701273955)_, Interrupt: `spge` _(1701277811)_
    - Trust (Faith): `cafa` _(1634099555)_, Interrupt: `spfa` _(1634103411)_
  - `cmd_no: 9` - _The value will depend on the action state:_
    - Use: `cait` _(1953063267)_
    - Interrupt: `spit` _(1953067123)_
  - `cmd_no: 10` - _Unknown._
  - `cmd_no: 11` - _The value will be equal to the monster skill id._
  - `cmd_no: 12` - _The value will depend on the action state:_
    - Use: `calg` _(1735156067)_
    - Interrupt: `splg` _(1735159923)_
  - `cmd_no: 13` - _Unknown._
  - `cmd_no: 14` - _The value will be equal to the ability id._
  - `cmd_no: 15` - _The value will be equal to the ability id._

If an action is interrupted, then a secondary action with the same `cmd_no` will be sent with the `cmd_arg` being marked as interrupted. _(It's prefix will start with `sp` when denoting an interrupt.)_

### `info`

_The action info. (Used as an additional value.)_

This value differs based on the current `cmd_no` of the action.

  - `cmd_no: 0` - _N/A_
  - `cmd_no: 1` - _The value is 0. (Unused.)_
  - `cmd_no: 2` - _The value is 0. (Unused.)_
  - `cmd_no: 3` - _The value is 0. (Unused.)_
  - `cmd_no: 4` - _The value will be the recast time (in seconds) for the spell._
  - `cmd_no: 5` - _The value is 0. (Unused.)_
  - `cmd_no: 6` - _The value is 0. (Unused.)_
  - `cmd_no: 7` - _The value is 0. (Unused.)_
  - `cmd_no: 8` - _The value is 0. (Unused.)_
  - `cmd_no: 9` - _The value is 0. (Unused.)_
  - `cmd_no: 10` - _Unknown._
  - `cmd_no: 11` - _The value is 0. (Unused.)_
  - `cmd_no: 12` - _The value is 0. (Unused.)_
  - `cmd_no: 13` - _The value is 0. (Unused.)_
  - `cmd_no: 14` - _The value is 0. (Unused.)_
  - `cmd_no: 15` - _The value is 0. (Unused.)_

### `res_sum`

_The number of results in the action._

This value does not appear to be currently used, it's value will always be 0.

### `trg_sum`

_The number of targets affected by the action._

### `target`

_The array of targets affected by the action._

### `m_bMessagePutFlag`

_The array of flags used to track if the client has processed the target entries information via the `CXiSchStatus::PutMessage` function._

Like the `target` array, this array is also has 64 entries, one for each target. After the client parses the action information, it will be forwarded to the proper schedular(s) which in turn will eventually attempt to process the message through the clients `CXiSchStatus::PutMessage` function. When the given target entry inside of the action is processed, it's matching `m_bMessagePutFlag` will be set to 1 to mark it as handled.

## Structure Fields (`BattleTarget`)

### `m_ppTargetAtel`

_The actor pointer of the entity for this target entry. (The client refers to this as the 'target'.)_

**Note:** This value is not part of the packet data. It is populated using the `m_uID` value after its read.

### `m_uID`

_The server id of the entity for this target entry._

### `result_sum`

_The number of results that are being applied to this target._

### `result`

_The array of results that are being applied to this target._

Each result entry counts as a separate effect that will happen against the target. Actions that cause multiple hits will have each hit separated into its own result entry. _(For example, if a dual-wielding THF triple attacks with both their main-hand and off-hand weapons in a single attack round, then the resulting action sent to clients will have 6 result entries against the given target.)_

## Structure Fields (`BattleResult`)

**Note:** Not all action commands (`cmd_no`) make use of every value contained inside of the `BattleResult` structure.  Because of this, you may notice some actions contaiing odd/unexpected values for certain things. _For example, the `miss` value may retain a value from a previous action when receiving a 'Starting' type of action._

### `miss`

_The result miss state value._

  - `0` - Hit _(The action hit the target.)_
  - `1` - Miss _(The action missed the target.)_
  - `2` - Guard _(The action was guarded by the target.)_
  - `3` - Parry _(The action was parried by the target.)_
  - `4` - Block _(The action was blocked by the target.)_
  - `5` - _Unknown_
  - `6` - _Unknown_
  - `7` - _Unknown_
  - `8` - _Unknown_
  - `9` - Evade _(The action was evaded by the target.)_

### `kind` & `sub_kind`

_The result kind and sub\_kind values._

These values relate to the current action `cmd_no` value. The client will make use of these values to determine the animation(s) that will be played.

  - `cmd_no: 1` - (Basic Attack)
    - `kind == 1 && sub_kind == 0` - The entity will attack/swing with their main hand weapon.
    - `kind == 1 && sub_kind == 1` - The entity will attack/swing with their off-hand weapon.
    - `kind == 1 && sub_kind == 2` - The entity will attack/swing with their right foot. _(Only works when the entity is using hand-to-hand weapons, or no weapon equipped.)_
    - `kind == 1 && sub_kind == 3` - The entity will attack/swing with their left foot. _(Only works when the entity is using hand-to-hand weapons, or no weapon equipped.)_
    - `kind == 1 && sub_kind == 4` - The entity will throw their ranged weapon.
  - `cmd_no: 2` - (Range Attack (Finish))
    - `kind` does not appear to be used. _(Always `1`, other values have no affect.)_
    - `sub_kind` does not appear to be used.
  - `cmd_no: 3` - (Skill (Finish))
    - `kind` is set to `3`.
    - `sub_kind` is set to the animation id to be played. _(ie. Shark Bite will set this to `39`. Steal will set this to `181` or `182` depending if its successful.)_
  - `cmd_no: 4` - (Magic (Finish))
    - `kind` is set to `0`.
    - `sub_kind` is set to the animation id to be played. _(ie. Phalanx II will set this to `107`.)_
  - `cmd_no: 5` - (Item (Finish))
    - `kind` is set to `1`.
    - `sub_kind` is set to the animation id to be played. _(ie. Echo Drops will set this to `4`.)_
  - `cmd_no: 6` - (Ability (Finish)) _(Dancer Flourish)_
    - `kind` is set to `2`.
    - `sub_kind` is set to the animation id to be played. _(ie. Sneak Attack will set this to `17`.)_
  - `cmd_no: 7` - (Skill (Start))
    - `kind` does not appear to be used. _(Always `1`, other values have no affect.)_
    - `sub_kind` does not appear to be used.
  - `cmd_no: 8` - (Magic (Start))
    - `kind` does not appear to be used.
    - `sub_kind` does not appear to be used.
  - `cmd_no: 9` - (Item (Start)) _(Also sent if the item use is interrupted.)_
    - `kind` does not appear to be used. _(Always `1`, other values have no affect.)_
    - `sub_kind` does not appear to be used.
  - `cmd_no: 10` - (Ability (Start))
    - _Unknown at this time._
  - `cmd_no: 11` - (Monster Skill (Finish), Trust Attacks _(ie. Shantotto Melee Attack)_)
    - `kind` is set to `2` for monster skills, or `3` for Trust attacks.
    - `sub_kind` is set to the animation id to be played. _(ie. Crabs using Big Scissors will set this to `188`., Shantotto melee attack sets this to `2546`.)_
  - `cmd_no: 12` - (Range Attack (Start))
    - `kind` does not appear to be used. _(Always `0`, other values have no affect.)_
    - `sub_kind` is set to `48`, the ranged attack animation id. _(This value is not required. The client will still play the proper animation from the `cmd_arg`.)_
  - `cmd_no: 13` - (Unknown)
    - _Unknown at this time._
  - `cmd_no: 14` - (Dancer Ability (Flourish, Jig, Samba, Step, Waltz, etc.))
    - `kind` is set to `2`.
    - `sub_kind` is set to the animation id to be played. _(ie. Spectral Jig will set this to `12`.)_
  - `cmd_no: 15` - (Rune Fencer Effusion/Ward)
    - `kind` is set to `3`.
    - `sub_kind` is set to the animation id to be played. _(ie. Effusion > Lunge will set this to `5`.)_

### `info`

_The result info value._

For general attacks, this value is used to determine the the severity of the hit.

  - `0` - Normal hit. _(Retail picks between `0` and `1` without a known cause.)_
  - `1` - Normal hit. _(Retail picks between `0` and `1` without a known cause.)_
  - `2` - Critical hit. _(Retail picks between `2` and `3` without a known cause.)_
  - `3` - Critical hit. _(Retail picks between `2` and `3` without a known cause.)_

For `cmd_no: 15`, when using Ward or Effusion, this value is used to set the element color and type:

  - `0` - _Unknown_
  - `1` - Ignis
  - `2` - Gelus
  - `3` - Flabra
  - `4` - Tellus
  - `5` - Suplor
  - `6` - Unda
  - `7` - Lux
  - `8` - Tenebrae

### `scale`

_The result scale value._

This value is used differently depending on the type of `cmd_no` and additional parameters for the given action.

For general attacks, this value is used to determine the model hit distortion animation that will play from the attack. _(The animation that plays to show your character bending backward from the shoulder/waist.)_ This value is used as an index into a lookup table for the distortion amount. The value is read from the packet as: `auto idx = scale & 3;`

The client uses a hardcoded lookup table for the distortion amounts which includes:

  - `0` - 0.0
  - `1` - 0.25
  - `2` - 0.5
  - `3` - 1.0

_Normal hits will generally use 0, 1, or 2 while critical hits will generally use 2 or 3._

For abilities and attacks that cause a knockback effect to happen, this value is used to determine the knockback amount. This value is used as an index into a lookup table for the knockback amount. The value is read from the packet as: `auto idx = scale >> 2;`

The client uses a hardcoded lookup table for the knockback information which includes:

  - `0` - Vec: `0.083333336`, Dumper: `0.075000003`, Timer: `5.0`
  - `1` - Vec: `0.16666667`, Dumper: `0.15000001`, Timer: `5.0`
  - `2` - Vec: `0.16666667`, Dumper: `0.15000001`, Timer: `10.0`
  - `3` - Vec: `0.16666667`, Dumper: `0.15000001`, Timer: `18.0`
  - `4` - Vec: `0.16666667`, Dumper: `0.125`, Timer: `30.0`
  - `5` - Vec: `0.16666667`, Dumper: `0.1`, Timer: `35.0`
  - `6` - Vec: `0.16666667`, Dumper: `0.050000001`, Timer: `45.0`

### `value`

_The result value._

This value is used differently depending on the type of `cmd_no` and additional parameters for the given action.

  - `cmd_no: 0` - _Unknown._
  - `cmd_no: 1` - This value is set to the amount of damage by the attack.
  - `cmd_no: 2` - This value is set to the amount of damage by the attack.
  - `cmd_no: 3` - This value is set to the amount of damage by the attack.
  - `cmd_no: 4` - This value is set depending on the type of magic.
    - For spells that deal damage, this value is set to the amount of damage by the spell.
    - For spells that heal, this value is set to the amount of healing by the spell.
    - For spells that buff (grant status effects), this value is set to the status effect id granted. _(ie. Refresh is `43`)_
    - For spells that debuff (grant negative status effects), this value is set to the status effect id granted. _(ie. Paralyze is `4`)_
  - `cmd_no: 5` - This value is set to `0`. _(Note: Depending on the item used, this may result in a value being set related to the argument(s) used within the message printed.)_
  - `cmd_no: 6` - This value is set depending on the type of ability used.
    - For abilities that buff, this value is set to the status effect id granted. _(ie. Sneak Attack is `65`)_
  - `cmd_no: 7` -
  - `cmd_no: 8` - This value is set to the spell id being casted.
  - `cmd_no: 9` - This value is set to the item id being used. _(If interrupted, this value is set to 0 instead.)_
  - `cmd_no: 10` - _Unknown._
  - `cmd_no: 11` - This value is set depending on the type of action.
    - For Trust Attacks, this value is set to the amount of damage by the attack.
    - For Monster Skills, this value is set based on the same rules as `cmd_no: 4`.
  - `cmd_no: 12` - This value is set to `0`.
  - `cmd_no: 13` - _Unknown._
  - `cmd_no: 14` - This value is set depending on the type of DNC ability used.
    - For Steps, this value is set to the current level affecting the target.
    - For Flourishes that deal damage, this value is set to the amount of damage dealt.
    - For Flourishes that cause an effect, this value is set to the status effect id granted. _(ie. Desperate Flourish causes weight `12`)_
  - `cmd_no: 15` - This vlaue is set depending on the type of RUN ability used.
    - For Ward > Vallation, this value is set to the status effect id granted. _(ie. Vallation is `531`)_
    - For Effusion > Swipe/Lunge, this value is set to the amount of damage dealt.

### `message`

_The result message id value._

This value is used to hold the message id of the message that will be printed to the chat log. Message ids are used to lookup and pull a message formatter string from the games DAT files. The following files hold the main action message strings used for this lookup:

  - JP: `ROM/27/71.DAT` (File Id: `7026`)
  - NA: `ROM/27/72.DAT` (File Id: `7027`)

_The messages stored in these files are formatter strings used with a format-style function like `sprintf`. The arguments populated into the string can come from various sources depending on the format token used. The main argument used from the action packet in general is the `value` field._

### `bit`

_The result bit value._

This value is used to hold the extended message modifier flags.

  - `0x00` - _None._
  - `0x01` - Cover!
  - `0x02` - Resist!
  - `0x04` - Magic Burst!
  - `0x08` - Immunobreak!
  - `0x10` - Critical Hit!

_**Note:** Not all actions/results that would cause these conditions use these flags. These are generally only used for specific purposes._

## Structure Fields (BattleTarget -> Procs)

The fields within the `BattleTarget` structure that are prefixed with `proc_` are used to hold the given results additional effect information. Generally, procs are effects that happen by the caster, onto the target. _(ie. Enspell additional damage effects, skill chains, etc.)_

When no proc effect has happened on the result, then this information is not included in the packet. _(There is a single bit value put just before this block of data, if it is set to 0 then the block of proc information is not included in the packet!)_

### `proc_kind`

_The result proc kind value._

This value is used differently depending on the type of `cmd_no`. However, in each instance, the value is used to determine which animation to play.

For general and ranged attacks (`cmd_no: 1, 2`), this value is used to play the additional effect animation. _(Some ids are shared between multiple animations, depending on other factors.)_

  - `0` - _None._
  - `1` - Element Damage (Fire) _(ie. Enfire)_
  - `2` - Element Damage (Ice) _(ie. Enblizzard)_
  - `3` - Element Damage (Wind) _(ie. Enaero)_
  - `4` - Element Damage (Earth) _(ie. Enstone)_
  - `5` - Element Damage (Lightning) _(ie. Enthunder)_
  - `6` - Element Damage (Water) _(ie. Enwater)_
  - `7` - Element Damage (Light) _(ie. Enlight, Treasure Hunter procs use this effect as well.)_
  - `8` - Element Damage (Dark) _(ie. Endark)_
  - `9` - Sleep
  - `10` - Poison
  - `11` - _Unknown effect._
  - `12` - Blind
  - `13` - Silence
  - `14` - Petrify
  - `15` - Plague
  - `16` - Stun
  - `17` - Curse
  - `18` - Weaken _(Attack, Defense, Evasion, etc.)_
  - `19` - Death
  - `20` - Shield
  - `21` - HP Drain _(ie. Drain Samba)_
  - `22` - MP Drain, TP Drain _(ie. Aspir Samba)_
  - `23` - Haste _(ie. Haste Samba)_

For weapon skills (`cmd_no: 3`), this value is used to determine the skill chain effect.

  - `0` - _None._
  - `1` - Skillchain: Light
  - `2` - Skillchain: Darkness
  - `3` - Skillchain: Gravitation
  - `4` - Skillchain: Fragmentation
  - `5` - Skillchain: Distortion
  - `6` - Skillchain: Fusion
  - `7` - Skillchain: Compression
  - `8` - Skillchain: Liquefaction
  - `9` - Skillchain: Induration
  - `10` - Skillchain: Reverberation
  - `11` - Skillchain: Transfixion
  - `12` - Skillchain: Scission
  - `13` - Skillchain: Detonation
  - `14` - Skillchain: Impaction
  - `15` - Skillchain: Radiance
  - `16` - Skillchain: Umbra

### `proc_info`

_The result proc info value._

### `proc_value`

_The result proc value._

This value is used differently depending on the type of `cmd_no`.

  - `cmd_no: 0` - _None._
  - `cmd_no: 1, 2`
    - If the proc is damage related, it will hold the amount of damage.
    - If the proc is effect related _(ie. Treasure Hunter proc)_, it will hold the current value of the proc.
    - If the proc is absorb related _(ie. Drain Samba proc)_, it will hold the amount of health drained.
  - `cmd_no: 2` - _Unknown._
  - `cmd_no: 3` - _Unknown._
  - `cmd_no: 4` - _Unknown._
  - `cmd_no: 5` - _Unknown._
  - `cmd_no: 6`
    - For skills like `Scavange`, this value will hold the amount of items found.
  - `cmd_no: 7` - _Unknown._
  - `cmd_no: 8` - _Unknown._
  - `cmd_no: 9` - _Unknown._
  - `cmd_no: 10` - _Unknown._
  - `cmd_no: 11` - _Unknown._
  - `cmd_no: 12` - _Unknown._
  - `cmd_no: 13` - _Unknown._
  - `cmd_no: 14` - _Unknown._
  - `cmd_no: 15` - _Unknown._

### `proc_message`

_The result proc message id value._

This value is used to hold the message id of the message that will be printed to the chat log. This works the same as the main `message` field value works.

## Structure Fields (BattleTarget -> Reacts)

The fields within the `BattleTarget` structure that are prefixed with `react_` are used to hold the given results reaction effect information. Generally, reacts are effects that happen by the target, onto the caster. _(ie. Countering, Spike Damage, etc.)_

When no react effect has happened on the result, then this information is not included in the packet. _(There is a single bit value put just before this block of data, if it is set to 0 then the block of proc information is not included in the packet!)_

### `react_kind`

_The result react kind value._

This value is used differently depending on the type of `cmd_no`. However, in each instance, the value is used to determine which animation to play.

For general and ranged attacks (`cmd_no: 1, 2`), this value is used to play the reaction effect animation. _(Some ids are shared between multiple animations, depending on other factors.)_

  - `0` - _None._
  - `1` - Spike Effect (Blaze)
  - `2` - Spike Effect (Ice)
  - `3` - Spike Effect (Dread)
  - `4` - Spike Effect (Curse)
  - `5` - Spike Effect (Shock)
  - `6` - Spike Effect (Reprisal)
  - `7` - Spike Effect (Wind)
  - `8` - Spike Effect (Earth)
  - `9` - Spike Effect (Water)
  - `10` - Spike Effect (Death)
  - `63` - Counter, Retaliation

### `react_info`

_The result react info value._

### `react_value`

_The result react value._

This value holds the argument value used with the reaction message.

  - For spike effects, this value will hold the amount of damage (or health healed) from the spike effect.
  - For counter attacks, this value will depend if the caster has shadows and if the counter is absorbed by them:
    - If the caster has shadows and will they absorb the counter attack, this value will be the number of shadows that absorb the attack.
    - If the caster does not have shadows (or they wont absorb the counter attack), this value will be the amount of damage the caster will take from being countered.

### `react_message`

_The result react message id value._

This value is used to hold the message id of the message that will be printed to the chat log. This works the same as the main `message` field value works.
