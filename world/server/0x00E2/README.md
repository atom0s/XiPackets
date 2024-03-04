# `GP_SERV_COMMAND_GROUP_LIST2`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_GROUP_LIST2` |
| **Client Handler**        | `RecvGroupList2` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00E2` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the server to initialize (and update) a party members information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_GROUP_ATTR
struct GP_GROUP_ATTR
{
	uint32_t        PartyNo             : 2;    // PS2: PartyNo
	uint32_t        PartyLeaderFlg      : 1;    // PS2: PartyLeaderFlg
	uint32_t        AllianceLeaderFlg   : 1;    // PS2: AllianceLeaderFlg
	uint32_t        PartyRFlg           : 1;    // PS2: PartyRFlg
	uint32_t        AllianceRFlg        : 1;    // PS2: AllianceRFlg
	uint32_t        unknown06           : 1;    // PS2: MasterComFlg
	uint32_t        unknown07           : 1;    // PS2: SubMasterComFlg
	uint32_t        padding00           : 24;   // PS2: dammy
};

// PS2: GP_SERV_GROUP_LIST2
struct GP_SERV_GROUP_LIST2
{
    uint16_t        id: 9;
    uint16_t        size: 7;
    uint16_t        sync;
    uint32_t        UniqueNo;       // PS2: UniqueNo
    uint32_t        Hp;             // PS2: Hp
    uint32_t        Mp;             // PS2: Mp
    uint32_t        Tp;             // PS2: Tp
    GP_GROUP_ATTR   GAttr;          // PS2: GAttr
    uint16_t        ActIndex;       // PS2: ActIndex
    uint8_t         MemberNumber;   // PS2: (New; was padding.)
    uint8_t         MoghouseFlg;    // PS2: (New; was padding.)
    uint8_t         Kind;           // PS2: Kind
    uint8_t         Hpp;            // PS2: (New; was HpMax)
    uint8_t         Mpp;            // PS2: (New; was MpMax)
    uint8_t         padding00;      // PS2: (New; did not exist.)
    uint16_t        ZoneNo;         // PS2: ZoneNo
    uint8_t         mjob_no;        // PS2: (New; did not exist.)
    uint8_t         mjob_lv;        // PS2: (New; did not exist.)
    uint8_t         sjob_no;        // PS2: (New; did not exist.)
    uint8_t         sjob_lv;        // PS2: (New; did not exist.)
    uint8_t         masterjob_lv;   // PS2: (New; did not exist.)
    uint8_t         masterjob_flags;// PS2: (New; did not exist.)
    uint8_t         Name[16];       // PS2: Name
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_GROUP_LIST2`)

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

### `GAttr`

_The party members flags._

### `ActIndex`

_The party members target index._

### `MemberNumber`

_The party members number within the party._

### `MoghouseFlg`

_Flag set if the party member is currently allowing guests into their mog house. (Open Mog House)_

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

### `Hpp`

_The party members current health percent._

### `Mpp`

_The party members current mana percent._

### `padding00`

_Padding; unused._

### `ZoneNo`

_The party members zone id._

When the party member is in the same zone as the local client, this will be set to 0. Otherwise, the client will treat the party member as in another zone and not show their entry information other than their name and abbreviated zone name.

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

### `Name`

_The party members name._

## Structure Fields (`GP_GROUP_ATTR`)

### `PartyNo`

_The party members party number._

### `PartyLeaderFlg`

_Flag set if the party member is a party leader._

### `AllianceLeaderFlg`

_Flag set if the party member is the alliance leader._

### `PartyRFlg`

_Flag set if the member is the party quartermaster._

### `AllianceRFlg`

_Flag set if the member is the alliance quartermaster._

### `unknown06`

_Unknown._

This value is still checked and used by the client. Its purpose is currently unknown.

### `unknown07`

_Unknown._

This value is still checked and used by the client. Its purpose is currently unknown.

### `padding00`

_Padding; unused._

## Additional Information

This packet functions similar to [**0x00DD**](/world/server/0x00DD/README.md)_ in almost every manner. The main difference is that if the client fails to find a valid entry for the member being updated, this packets handler will create a new party member entry and update it. _(`0x00DD` will simply exit the handler if no matching member was found.)_ The server will use this packet as the first/initial party member information update packet since the member has zoned, which allows the client to create the member entry if it is not missing before additional updates are sent later.

This packet does not follow the normal handling of string buffers for names. Instead, the size of the packet will change based on the length of the party members name. It will not adhere to the full `Name[16]` buffer that is expected like other packets that contain names. Due to this, the name should be determined based on the size of the packet and not assumed to be a full 16 byte buffer.
