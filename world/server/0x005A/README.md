# `GP_SERV_COMMAND_MOTIONMES`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_MOTIONMES` |
| **Client Handler**        | `RecvEmotionMes` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x005A` |
| **Size**                  | `0x0038` |

## Description

This packet is sent by the server when interacting with an NPC that handles world passes.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_MOTIONMES
struct GP_SERV_MOTIONMES
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    CasUniqueNo;        // PS2: CasUniqueNo
    uint32_t    TarUniqueNo;        // PS2: TarUniqueNo
    uint16_t    CasActIndex;        // PS2: CasActIndex
    uint16_t    TarActIndex;        // PS2: TarActIndex
    uint16_t    MesNum;             // PS2: MesNum
    uint16_t    Param;              // PS2: (New; did not exist.)
    uint16_t    unknown14;          // PS2: (New; did not exist.)
    uint8_t     Mode;               // PS2: Mode
    uint8_t     padding17;          // PS2: (New; did not exist.)
    uint32_t    FaithUniqueNo[5];   // PS2: (New; did not exist.)
    uint16_t    FaithActIndex[5];   // PS2: (New; did not exist.)
    uint16_t    padding36;          // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_MOTIONMES`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `CasUniqueNo`

_The caster entity server id._

### `TarUniqueNo`

_The target entity server id._

If the caster did not target an entity with the emote, this will be `0`.

### `CasActIndex`

_The caster entity target index._

### `TarActIndex`

_The target entity target index._

If the caster did not target an entity with the emote, this will be `0`.

### `MesNum`

_The emote id._

### `Param`

_The emote param._

### `unknown14`

_Unknown._

The client reads this value but does not appear to make use of it.

### `Mode`

_The emote mode._

This value represents how the emote should be executed.

| Mode | Functionality |
| --- | --- |
| `0` | `Both` _(The emote will play the motion and display its text to the chat log.)_ |
| `1` | `Text` _(The emote will only display its text to the chat log.)_ |
| `2` | `Motion` _(The emote will only play the motion.)_ |

### `padding17`

_Padding; unused._

### `FaithUniqueNo`

_The server ids for each of the casters summoned trusts._

These values are used to enable the `/emotefaith` command emote replication.

### `FaithActIndex`

_The target indexs for each of the casters summoned trusts._

These values are used to enable the `/emotefaith` command emote replication.

### `padding36`

_Padding; unused._

## Additional Information: `Param`

The parameter value is used as an extra value for certain emotes. If the given emote does not need or use the `Param` value, then its default value is the casters home nation id.

  - `0` - `San d'Oria`
  - `1` - `Bastok`
  - `2` - `Windurst`

When the emote being played is the `/hurray` emote, then the `Param` value will be replaced with the casters main hand weapon item id. If the caster does not have an item equipped in their main hand slot, then `Param` will instead be set to `0`. _(The client does not validate this value against the casters actual weapon item id and instead simply uses it to determine which version of the emote animation to play. If this value is set to `0` while they have a weapon equipped, it will play the non-equipped weapon variant still. If it is set to anything >= 1 and they have a weapon equipped it will play the animation using their weapon. If this value is >= 1 and they have no weapon equipped, it will still play the equipped weapon animation, but with an invisible weapon instead.)_

When the emote being played is the `/aim` emote, then the `Param` value will be replaced with the casters ranged weapon item id or ammo item id when the proper conditions are met.

  - If the caster is wearing a bow, crossbow, or similar kind of weapon that expects ammo _(ie. arrows)_ then the following will happen:
    - If no ammo is equipped, `Param` will be set to `0xFFFF`.
    - If ammo is equipped but is not a valid arrow kind for the equipped ranged weapon, `Param` will be set to `0xFFFF`.
    - If both the ranged weapon and ammo are valid, then `Param` will be set to the casters ranged weapon item id.
  - If the caster is wearing a thrown ranged weapon (ie. boomerang), then the following will happen:
    - If the ranged weapon is valid, then `Param` will be set to the casters ranged weapon item id.
  - If the caster is wearing ammo that is thrown by itself _(ie. Pebbles)_, then the following will happen:
    - If the ammo is not throwable, `Param` will be set to `0xFFFF`.
    - If the ammo is throwable, `Param` will be set to the casters ammo item id.

When the emote being played is any of the `/jobemote` variants, then the `Param` will be replaced with the job id _(minus one)_ that owns the emote.

| Emote Command | Param Value |
| --- | --- |
| `/jobemote war` | `0` |
| `/jobemote mnk` | `1` |
| `/jobemote whm` | `2` |
| `/jobemote blm` | `3` |
| `/jobemote rdm` | `4` |
| `/jobemote thf` | `5` |
| `/jobemote pld` | `6` |
| `/jobemote drk` | `7` |
| `/jobemote bst` | `8` |
| `/jobemote brd` | `9` |
| `/jobemote rng` | `10` |
| `/jobemote sam` | `11` |
| `/jobemote nin` | `12` |
| `/jobemote drg` | `13` |
| `/jobemote smn` | `14` |
| `/jobemote blu` | `15` |
| `/jobemote cor` | `16` |
| `/jobemote pup` | `17` |
| `/jobemote dnc` | `18` |
| `/jobemote sch` | `19` |
| `/jobemote geo` | `20` |
| `/jobemote run` | `21` |

## Additional Information: `FaithUniqueNo` & `FaithActIndex`

The `FaithUniqueNo` and `FaithActIndex` values are used to allow the casters emote animation to be replicated by their trusts. This is done when the `/emotefaith` command is enabled on the client. Emotes that support this kind of interaction will have these values populated in the packet indicating that the emote should be replicated by the casters trusts. The actual `/emotefaith` command check is performed on the client side, the server will always populate these values when the caster has trusts summoned and the emote is valid for replication.

If the emote does not support replication, then these values will be defaulted to `0`. _(For example, `/hurray` is not supported.)_
