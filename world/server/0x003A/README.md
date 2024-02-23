# `GP_SERV_COMMAND_MAGICSCHEDULOR`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_MAGICSCHEDULOR` |
| **Client Handler**        | `RecvMagicSchedulor` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x003A` |
| **Size**                  | `0x0014` |

## Description

This packet is sent by the server to inform the client of a magic scheduler animation/event that should play.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_MAGICSCHEDULOR
struct GP_SERV_MAGICSCHEDULOR
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    UniqueNoCas;    // PS2: UniqueNoCas
    uint32_t    UniqueNoTar;    // PS2: UniqueNoTar
    uint16_t    ActIndexCast;   // PS2: ActIndexCast
    uint16_t    ActIndexTar;    // PS2: ActIndexTar
    uint16_t    fileNum;        // PS2: fileNum
    uint8_t     type;           // PS2: (New; did not exist.)
    uint8_t     padding00;      // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_MAGICSCHEDULOR`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNoCas`

_The caster entity server id._

### `UniqueNoTar`

_The target entity server id._

### `ActIndexCast`

_The caster entity target index._

### `ActIndexTar`

_The target entity target index._

### `fileNum`

_The scheduler file id to be loaded._

### `type`

_The scheduler type._

The client supports type values of `0x00` to `0x0C` _(inclusive)_. However, the `type` value does not appear to be hard-locked into only playing certain kinds of animations related to that given type. It is possible for the scheduler to play different animation kinds regardless of the `type` given. For example, it is generally seen that the `type` value of `1` is used for item usage animations. However, if the `fileNum` value is `228` or above, it will instead play an overlay animation related to Chocobo racing.

_The following list is not guaranteed to be accurate due to the above note._

  - `0x00` - _Cast Spell_
  - `0x01` - _Use Item_
  - `0x02` - _Ability_
  - `0x03` - _Event Related (Misc event animations, banner announcements, etc.)_
  - `0x04` - _Event Related (Misc event animations, NPC weaponskills, etc.)_
  - `0x05` - _Unknown (Has misc ability animations, banner announcements, etc.)_
  - `0x06` - _Weapon Skills_
  - `0x07` - _Unknown_
  - `0x08` - _Unknown (Has misc ability animations, banner announcements, etc.)_
  - `0x09` - _Monster Skills_
  - `0x0A` - _Unknown (Has misc warping animations.)_
  - `0x0B` - _Unknown (Has misc banner announcements.)_
  - `0x0C` - _Unknown (Has misc casting animations.)_

### `padding00`

_Padding; unused._
