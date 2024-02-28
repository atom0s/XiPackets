# `GP_SERV_COMMAND_GROUP_TBL`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_GROUP_TBL` |
| **Client Handler**        | `RecvGroupTbl` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00C8` |
| **Size**                  | `0x00F8` |

## Description

This packet is sent by the server to update the clients party list information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GROUP_TBL
struct GROUP_TBL
{
    uint32_t    UniqueNo;                   // PS2: UniqueNo
    uint16_t    ActIndex;                   // PS2: ActIndex
    uint8_t     PartyNo             : 2;    // PS2: PartyNo
    uint8_t     PartyLeaderFlg      : 1;    // PS2: PartyLeaderFlg
    uint8_t     AllianceLeaderFlg   : 1;    // PS2: AllianceLeaderFlg
    uint8_t     PartyRFlg           : 1;    // PS2: PartyRFlg
    uint8_t     AllianceRFlg        : 1;    // PS2: AllianceRFlg
    uint8_t     unknown06           : 1;    // PS2: MasterComFlg
    uint8_t     unknown07           : 1;    // PS2: SubMasterComFlg
    uint8_t     padding00;                  // PS2: (New; was ZoneNo originally.)
    uint16_t    ZoneNo;                     // PS2: (New; did not exist.)
    uint16_t    padding01;                  // PS2: (New; did not exist.)
};

// PS2: GP_SERV_GROUP_TBL
struct GP_SERV_GROUP_TBL
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Kind;           // PS2: Kind
    uint8_t     padding00[3];   // PS2: (New; did not exist.)
    GROUP_TBL   GroupTbl[20];   // PS2: GroupTbl
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_GROUP_TBL`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

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

### `padding00`

_Padding; unused._

### `GroupTbl`

_The array of group members._

## Structure Fields (`GROUP_TBL`)

### `UniqueNo`

_The group members server id._

### `ActIndex`

_The group members target index._

### `PartyNo`

_The members party number._

This value will be either 0, 1 or 2.

| PartyNo | Purpose |
| --- | --- |
| `0` | _Invalid._ |
| `1` | _The main local client party._ |
| `2` | _The top alliance party._ |
| `3` | _The bottom alliance party._ |

### `PartyLeaderFlg`

_Flag set if the member is the party leader of their given party._

### `AllianceLeaderFlg`

_Flag set if the member is the alliance leader._

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

### `ZoneNo`

_The group members zone id._

### `padding01`

_Padding; unused._

## Additional Information

This packet is used to update an entire group table block within the client. The client internally has space to hold a maximum of 7 `GroupTbl` blocks in its local memory. _(The client memory format differs from the format used with this packets `GroupTbl` block!)_ The packets `Kind` value is used to determine which of the clients `GroupTbl` blocks is updated with the information that is stored in the packets `GroupTbl` array.

The main party and alliance list used by the client is stored in the first (index 0) `GroupTbl` block in the clients memory. When the server is updating that block with this packet, then `Kind` will be set to `0`. _(At this time, the other `Kind` values are unknown and have not been observed.)_ Each `GroupTbl` can hold a total of 20 member entries, however for the main party system, the client only uses 18 of those.
