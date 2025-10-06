# `GP_CLI_COMMAND_RECIPE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_RECIPE` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0058` |
| **Size**                  | `0x0014` |

## Description

This packet is sent by the client when interacting with a crafting NPC that can suggest recipes to the client.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_RECIPE
struct GP_CLI_RECIPE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    skill;      // PS2: skill
    uint16_t    level;      // PS2: level
    uint16_t    Param0;     // PS2: itemnum
    uint16_t    Mode;       // PS2: dammy
    uint16_t    Param1;     // PS2: (New; did not exist.)
    uint16_t    Param2;     // PS2: (New; did not exist.)
    uint16_t    Param3;     // PS2: (New; did not exist.)
    uint16_t    Param4;     // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_RECIPE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `skill`

_The crafting skill related to the NPC._

| Skill | Purpose |
| --- | --- |
| `0x00` | `Fishing` |
| `0x01` | `Woodworking` |
| `0x02` | `Smithing` |
| `0x03` | `Goldsmithing` |
| `0x04` | `Clothcraft` |
| `0x05` | `Leathercraft` |
| `0x06` | `Bonecraft` |
| `0x07` | `Alchemy` |
| `0x08` | `Cooking` |
| `0x09` | `Synergy` |
| `0x0A` | `Digging` |

### `level`

_The players skill level for the given crafting skill._

This value is the players actual skill leve, **NOT** their packed skill level! This will be a value ranging between 0 and 110. _(**Note:** Equipment that increases the players crafting skill does not affect this value.)_

### `Param0`

_The packet parameter value. (0)_

This value is set depending on the current `Mode` value.

### `Mode`

_The packet mode._

This value determines the purpose of the packet.

| Mode | Purpose |
| --- | --- |
| `0` | _Invalid; unused._ |
| `1` | _Request available rank list._ |
| `2` | _Request available recipe list. (For the given rank.)_ |
| `3` | _Request recipe materials._ |
| `4` | _(Campaign Ops) Request recipe from Adjutant; after selecting an expertise._ |
| `5` | _(Campaign Ops) Complete material list successfully._ |

_**Note:** While these are the main modes used for this packet, some NPCs will respond more directly regardless of the `Mode` of the packet that is set. The information set for `skill` and `level` can be used to determine enough information when talking to certain NPCs that they will just respond with something more direct instead._

### `Param1`

_The packet parameter value. (1)_

This value is set depending on the current `Mode` value.

### `Param2`

_The packet parameter value. (2)_

This value is set depending on the current `Mode` value.

### `Param3`

_The packet parameter value. (3)_

This value is set depending on the current `Mode` value.

### `Param4`

_The packet parameter value. (4)_

This value is set depending on the current `Mode` value.

## Additional Information

The client sends this packet when interacting with crafting NPCs that can offer suggestions for recipes and give recipe material lists. There are multiple `Mode` values used with this packet for different purposes based on the menu the client is interacting with. The information populated in the rest of this packets `Param` based values are based on the `Mode`.

## Additional Information - `Mode` - `0x01`

  - `Param0` - _Parameter reflected back from server._
    - _This value is set from the event start packet from the server the client reuses the value._
  - `Param1` - _Value not set._
  - `Param2` - _Value not set._
  - `Param3` - _Value not set._
  - `Param4` - _Value not set._

## Additional Information - `Mode` - `0x02`

  - `Param0` - _Set to `0`._
  - `Param1` - _Recipe pagenation starting offset._
  - `Param2` - _Recipe pagenation ending offset._
  - `Param3` - _Value not set._
  - `Param4` - _The recipe rank._

The recipe pagenation values _(`Param1` & `Param2`)_ are used to walk the pages of recipes. The initial page will start with values `0` and `16`. Each page beyond that will step through the recipes, incrementing the values by `16`.

## Additional Information - `Mode` - `0x03`

  - `Param0` - _Set to `0`._
  - `Param1` - _Value not set._
  - `Param2` - _Value not set._
  - `Param3` - _The requested recipe index._
  - `Param4` - _The recipe rank._

## Additional Information - `Mode` - `0x04`

  - `Param0` - _Set to `0`._
  - `Param1` - _Value not set._
  - `Param2` - _Value not set._
  - `Param3` - _Unknown. (Set from event parameter.)_
  - `Param4` - _Unknown. (Set from event parameter.)_

These values are unknown at this time.

## Additional Information - `Mode` - `0x05`

  - `Param0` - _Unknown. (Set from event parameter.)_
  - `Param1` - _Unknown. (Set from event parameter.)_
  - `Param2` - _Value not set._
  - `Param3` - _Unknown. (Set from event parameter.)_
  - `Param4` - _Unknown. (Set from event parameter.)_

These values are unknown at this time.