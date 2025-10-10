# `GP_SERV_COMMAND_CLISTATUS`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_CLISTATUS` |
| **Client Handler**        | `RecvCliStatus` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0061` |
| **Size**                  | `0x0070` |

## Description

This packet is sent by the server to update the clients general player information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: CLISTATUS
struct CLISTATUS
{
    int32_t     hpmax;                  // PS2: hpmax
    int32_t     mpmax;                  // PS2: mpmax
    uint8_t     mjob_no;                // PS2: mjob_no
    uint8_t     mjob_lv;                // PS2: mjob_lv
    uint8_t     sjob_no;                // PS2: sjob_no
    uint8_t     sjob_lv;                // PS2: sjob_lv
    int16_t     exp_now;                // PS2: exp_now
    int16_t     exp_next;               // PS2: exp_next
    uint16_t    bp_base[7];             // PS2: bp_base
    int16_t     bp_adj[7];              // PS2: bp_adj
    int16_t     atk;                    // PS2: atk
    int16_t     def;                    // PS2: def
    int16_t     def_elem[8];            // PS2: def_elem
    uint16_t    designation;            // PS2: designation
    uint16_t    rank;                   // PS2: rank
    uint16_t    rankbar;                // PS2: rankbar
    uint16_t    BindZoneNo;             // PS2: BindZoneNo
    uint32_t    MonsterBuster;          // PS2: MonsterBuster
    uint8_t     nation;                 // PS2: nation
    uint8_t     myroom;                 // PS2: myroom
    uint8_t     su_lv;                  // PS2: (New; did not exist.)
    uint8_t     padding4F;              // PS2: (New; did not exist.)
    uint8_t     highest_ilvl;           // PS2: (New; did not exist.)
    uint8_t     ilvl;                   // PS2: (New; did not exist.)
    uint8_t     ilvl_mhand;             // PS2: (New; did not exist.)
    uint8_t     ilvl_ranged;            // PS2: (New; did not exist.)
    uint32_t    unity_info;             // PS2: (New; did not exist.)
    uint16_t    unity_points1;          // PS2: (New; did not exist.)
    uint16_t    unity_points2;          // PS2: (New; did not exist.)
    uint32_t    unity_chat_color_flag;  // PS2: (New; did not exist.)
    uint32_t    mastery_info;           // PS2: (New; did not exist.)
    uint32_t    mastery_exp_now;        // PS2: (New; did not exist.)
    uint32_t    mastery_exp_next;       // PS2: (New; did not exist.)
};

// PS2: GP_SERV_CLISTATUS
struct GP_SERV_CLISTATUS
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    CLISTATUS   statusdata; // PS2: statusdata
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_CLISTATUS`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `statusdata`

_The clients status information._

## Structure Fields (`CLISTATUS`)

### `hpmax`

_The clients maximum health._

### `mpmax`

_The clients maximum mana._

### `mjob_no`

_The clients main job id._

### `mjob_lv`

_The clients main job level._

### `sjob_no`

_The clients sub job id._

### `sjob_lv`

_The clients sub job level._

### `exp_now`

_The clients current experience points._

### `exp_next`

_The clients needed experience points to level._

### `bp_base`

_The clients base stats._

This array of data holds the clients main stats in the following order:

  - `STR`, `DEX`, `VIT`, `AGI`, `INT`, `MND`, `CHR`

### `bp_adj`

_The clients base stat modifiers._

This array of data holds the clients main stat modifier values in the following order:

  - `STR`, `DEX`, `VIT`, `AGI`, `INT`, `MND`, `CHR`

### `atk`

_The clients attack value._

### `def`

_The clients defense value._

### `def_elem`

_The clients elemental resistances._

This array of data holds the clients elemental resistance values in the following order:

  - `Fire`, `Ice`, `Wind`, `Earth`, `Lightning`, `Water`, `Light`, `Dark`

### `designation`

_The clients title id._

### `rank`

_The clients rank._

### `rankbar`

_The clients rank bar points._

### `BindZoneNo`

_The clients homepoint zone id._

### `MonsterBuster`

_Unknown._

The client stores this value but does not reference or use it anywhere else.

### `nation`

_The clients home nation id._

  - `0` - `San d'Oria`
  - `1` - `Bastok`
  - `2` - `Windurst`

### `myroom`

_The clients home residence location._

The client stores this value but does not reference or use it anywhere else.

### `su_lv`

_The clients superior equipment level._

### `padding4F`

_Padding; unused._

### `highest_ilvl`

_The clients highest equipped item level._

The client stores this value but does not reference or use it anywhere else.

### `ilvl`

_The clients average item level._

The client will add `99` to this value when it is `>= 1`.

### `ilvl_mhand`

_The clients main hand weapon item level._

The client stores this value but does not reference or use it anywhere else.

### `ilvl_ranged`

_The clients ranged weapon item level._

The client stores this value but does not reference or use it anywhere else.

### `unity_info`

_The clients unity information._

This value is bitpacked and holds multiple pieces of information related to unity.

```cpp
struct unityinfo_t
{
    uint32_t Faction : 5;
    uint32_t Unknown : 5;
    uint32_t Points : 17;
    uint32_t unused : 5;
};
```

### `unity_points1`

_The clients unity points._

This value holds the clients partial unity personal evaluation points.

### `unity_points2`

_The clients unity points._

This value holds the clients personal unity evaluation points.

### `unity_chat_color_flag`

_The clients unity chat color flag._

The client only uses the lowest bit of this value.

This value is used to alter the name color of the Unity leader in the chat window when typing `/unity`.

  - If this value is set to `0`, then the Unity leaders name will be a dark grey.
  - If this value is set to `1`, then the Unity leaders name will be a light white/grey.

### `mastery_info`

_The clients mastery information._

This value holds multiple pieces of information.

```cpp
struct masteryinfo_t
{
    uint8_t job_no;     // The clients mastery job id. (The client does not use this value.)
    uint8_t job_lv;     // The clients mastery job level.
    uint8_t flags;      // The clients mastery job flags.
    uint8_t padding00;  // Unused.
};
```

The `masteryinfo_t::flags` value holds two flags currently:

  - `0x01` - Flag that states the job mastery system is unlocked. _(Enables `Master Levels` menu.)_
  - `0x02` - Flag that states the current job is capped on exemplar points.

### `mastery_exp_now`

_The clients current mastery experience points._

### `mastery_exp_next`

_The clients needed mastery experience points to level._
