# `GP_SERV_COMMAND_GROUP_ATTR`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_GROUP_ATTR` |
| **Client Handler**        | `RecvGroupAttr` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00DF` |
| **Size**                  | `0x0012`, `0x0014` |

## Description

This packet is sent by the server to update a party members information. This packet is similar to `0x00DD`, but is used for the local client player and Trust party members.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_GROUP_ATTR
struct GP_SERV_GROUP_ATTR
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    UniqueNo;           // PS2: UniqueNo
    uint32_t    Hp;                 // PS2: Hp
    uint32_t    Mp;                 // PS2: Mp
    uint32_t    Tp;                 // PS2: Tp
    uint16_t    ActIndex;           // PS2: ActIndex
    uint8_t     Hpp;                // PS2: (New; was HpMax)
    uint8_t     Mpp;                // PS2: (New; was MpMax)
    uint8_t     Kind;               // PS2: Kind
    uint8_t     MoghouseFlg;        // PS2: (New; did not exist.)
    uint16_t    ZoneNo;             // PS2: (New; did not exist.)
    uint16_t    MonstrosityFlag;    // PS2: (New; did not exist.)
    uint16_t    MonstrosityNameId;  // PS2: (New; did not exist.)
    uint8_t     mjob_no;            // PS2: (New; did not exist.)
    uint8_t     mjob_lv;            // PS2: (New; did not exist.)
    uint8_t     sjob_no;            // PS2: (New; did not exist.)
    uint8_t     sjob_lv;            // PS2: (New; did not exist.)
    uint8_t     masterjob_lv;       // PS2: (New; did not exist.)
    uint8_t     masterjob_flags;    // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_GROUP_ATTR`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The party members server id._

### `Hp`

_The party members current health._

### `Mp`

_The party members current mana._

### `Tp`

_The party members current TP._

### `ActIndex`

_The party members target index._

### `Hpp`

_The party members current health percent._

### `Mpp`

_The party members current mana percent._

### `Kind`

_The group kind._

| Kind | Usage |
| --- | --- |
| `0` | _Main Party (and Alliance)_ |
| `1` | _Unknown._ |
| `2` | _Unknown._ |
| `3` | _Unknown._ |
| `4` | _Unknown._ |
| `5` | _Resets `Kind` to `0`; uses Main Party table._ |
| `6` | _Unknown._ |

### `MoghouseFlg`

_Flag set if the party member is currently allowing guests into their mog house. (Open Mog House)_

### `ZoneNo`

_The party members zone id._

This value is only used with the `MoghouseFlg` when its enabled.

### `MonstrosityFlag`

_The party members Monstrosity flags._

### `MonstrosityNameId`

_The party members Monstrosity name ids._

_This value holds two bytes used to build the full Monstrosity name._

### `mjob_no`

_The party members main job id._

### `mjob_lv`

_The party members main job level._

### `sjob_no`

_The party members sub job id._

### `sjob_lv`

_The party members sub job level._

### `masterjob_lv`

_The party members master job level._

### `masterjob_flags`

_The party members master job flags._

This value holds two flags related to the party members job mastery:

  - `0x01` - Flag that states the job mastery system is unlocked.
  - `0x02` - Flag that states the current job is capped on exemplar points.

## Additional Information

The server will send this packet in varying sizes of either `0x0012` or `0x0014` bytes long. This appears to be done to ignore the Mastery information if the party member this packet is used to update has not unlocked that system. _(Or when updating non-player members.)_ However, the client will still try and read the information from the buffer as if that data is present. The client does not check to determine which variant of the packet is being sent or used.
