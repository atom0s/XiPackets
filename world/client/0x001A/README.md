# `GP_CLI_COMMAND_ACTION`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_ACTION` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x001A` |
| **Size**                  | `0x001C` |

## Description

This packet is sent by the client when it is requesting to perform an action.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_ACTION
struct GP_CLI_ACTION
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    UniqueNo;       // PS2: UniqueNo
    uint16_t    ActIndex;       // PS2: ActIndex
    uint16_t    ActionID;       // PS2: ActionID
    uint32_t    ActionBuf[4];   // PS2: ActionBuf
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_ACTION`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The server id of the entity the requested action is targeting._

### `ActIndex`

_The target index of the entity the requested action is targeting._

### `ActionID`

_The action id._

| Id | Purpose |
| --- | --- |
| `0x00` | _Talk (Interact With NPC)_ |
| `0x01` | _Unused._ |
| `0x02` | _Attack (`/attack`)_ |
| `0x03` | _Cast Magic (`/magic`)_ |
| `0x04` | _Attack Off (`/attackoff`)_ |
| `0x05` | _Help (`/help`)_ |
| `0x06` | _Unused._ |
| `0x07` | _Weapon Skill (`/weaponskill`)_ |
| `0x08` | _Unused._ |
| `0x09` | _Job Ability (`/jobability`, `/bstpet`)_ |
| `0x0A` | _Unused._ |
| `0x0B` | _Homepoint Menu Response_ |
| `0x0C` | _Assist (`/assist`)_ |
| `0x0D` | _Raise Menu Response_ |
| `0x0E` | _Fish (`/fish`)_ |
| `0x0F` | _Change Target_ |
| `0x10` | _Shoot (`/shoot`)_ |
| `0x11` | _Chocobo Dig_ |
| `0x12` | _Dismount (`/dismount`)_ |
| `0x13` | _Tractor Menu Response_ |
| `0x14` | _Set SendResRdy Flag_ |
| `0x15` | _Quarry (`/quarry`)_ |
| `0x16` | _Sprint (`/sprint`)_ |
| `0x17` | _Scout (`/scout`)_ |
| `0x18` | _Blockaid (`/blockaid`)_ |
| `0x19` | _Monster Skill (`/monsterskill`)_ |
| `0x1A` | _Mount (`/mount`)_ |

### `ActionBuf`

_The array of action parameters._

The values stored in this array of parameters will depend on the `ActionID` that is being performed.

## Additional Information

The client makes use of the `ActionBuf` array to set various needed extra bits of information regarding the action being peformed. Some actions do not require any additional parameters and will have all of these set to `0` while some actions may make use of all of the parameters. Below is a breakdown of what the parameter values are for each action type.

## Action Parameters - `0x00` (Talk)

When talking to or interacting with an npc:

| Index | Purpose |
| --- | --- |
| `0` | `0x00` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

When releasing a specific Trust or Fellow:

| Index | Purpose |
| --- | --- |
| `0` | `0x01` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

When releasing all Trusts:

| Index | Purpose |
| --- | --- |
| `0` | `Index` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

_The `Index` value is an incrementing value for each Trust that is being released, starting at 0._

When checking (`/check`) an entity under certain conditions:

| Index | Purpose |
| --- | --- |
| `0` | `2` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

## Action Parameters - `0x02` (Attack)

| Index | Purpose |
| --- | --- |
| `0` | `0x00` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

## Action Parameters - `0x03` (Cast Magic)

| Index | Purpose |
| --- | --- |
| `0` | `SpellId` |
| `1` | `PosX` |
| `2` | `PosZ` |
| `3` | `PosY` |

_The `PosX`, `PosZ`, and `PosY` values are used for special actions that are based on ground-targeting, such as certain Geomancer spells._

## Action Parameters - `0x04` (Attack Off)

| Index | Purpose |
| --- | --- |
| `0` | `0x00` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

## Action Parameters - `0x05` (Help)

| Index | Purpose |
| --- | --- |
| `0` | `0x00` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

## Action Parameters - `0x07` (Weapon Skill)

| Index | Purpose |
| --- | --- |
| `0` | `SkillId` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

_The `SkillId` value is set to the selected weapon skill being performed. If the weapon skill id is `>= 256`, the client adds `8192` to the value._

## Action Parameters - `0x09` (Job Ability)

| Index | Purpose |
| --- | --- |
| `0` | `SkillId` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

_The `SkillId` value is set to the selected job ability being performed. If the command category of the ability is either `43` (job ability) or `44` (pet), the client will subtract `512` from the value._

## Action Parameters - `0x0B` (Homepoint Menu Response)

| Index | Purpose |
| --- | --- |
| `0` | `StatusId` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

The `StatusId` value is set to one of the following:

  - `0` - _Homepoint accepted. (Used when not under Monstrosity.)_
  - `1` - _Monstrosity: Cancel_
  - `2` - _Monstrosity: Retry_

## Action Parameters - `0x0C` (Assist)

| Index | Purpose |
| --- | --- |
| `0` | `0x00` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

## Action Parameters - `0x0D` (Raise Menu Response)

| Index | Purpose |
| --- | --- |
| `0` | `StatusId` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

The `StatusId` value is set to one of the following:

  - `0` - _Raise accepted._
  - `1` - _Raise rejected._

## Action Parameters - `0x0E` (Fish)

| Index | Purpose |
| --- | --- |
| `0` | `0x00` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

## Action Parameters - `0x0F` (Change Target)

| Index | Purpose |
| --- | --- |
| `0` | `0x00` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

## Action Parameters - `0x10` (Shoot)

| Index | Purpose |
| --- | --- |
| `0` | `0x00` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

## Action Parameters - `0x11` (Chocobo Dig)

| Index | Purpose |
| --- | --- |
| `0` | `0x00` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

## Action Parameters - `0x12` (Dismount)

| Index | Purpose |
| --- | --- |
| `0` | `0x00` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

## Action Parameters - `0x13` (Tractor Menu Response)

| Index | Purpose |
| --- | --- |
| `0` | `StatusId` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

The `StatusId` value is set to one of the following:

  - `0` - _Tractor accepted._
  - `1` - _Tractor rejected._

## Action Parameters - `0x14` (Set SendResRdy Flag)

| Index | Purpose |
| --- | --- |
| `0` | `0x00` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

## Action Parameters - `0x15` (Quarry)

| Index | Purpose |
| --- | --- |
| `0` | `0x00` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

## Action Parameters - `0x16` (Sprint)

| Index | Purpose |
| --- | --- |
| `0` | `0x00` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

## Action Parameters - `0x17` (Scout)

| Index | Purpose |
| --- | --- |
| `0` | `0x00` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

## Action Parameters - `0x18` (Blockaid)

| Index | Purpose |
| --- | --- |
| `0` | `StatusId` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

The `StatusId` value is set to one of the following based on the command arguments:

  - `0` - _Disable blockaid._
  - `1` - _Enable blockaid._
  - `2` - _Toggle blockaid._

## Action Parameters - `0x19` (Monster Skill)

| Index | Purpose |
| --- | --- |
| `0` | `SkillId` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

_The `SkillId` value is set to the selected monster ability being performed. The client subtracts `1536` from this value._

## Action Parameters - `0x1A` (Mount)

| Index | Purpose |
| --- | --- |
| `0` | `MountId` |
| `1` | `0x00` _(Unused.)_ |
| `2` | `0x00` _(Unused.)_ |
| `3` | `0x00` |

_The `MountId` value is set to the selected mount._
