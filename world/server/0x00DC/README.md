# `GP_SERV_COMMAND_GROUP_SOLICIT_REQ`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_GROUP_SOLICIT_REQ` |
| **Client Handlers**       | `RecvGroupSolicitreq`, `YkWndPartyList::RecvGroupSolicitReq` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00DC` |
| **Size**                  | `0x0020` |

## Description

This packet is sent by the server when the client is being invited to a party or alliance.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_GROUP_SOLICIT_REQ
struct GP_SERV_GROUP_SOLICIT_REQ
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    UniqueNo;
    uint16_t    ActIndex;
    uint8_t     AnonFlag;
    uint8_t     Kind;
    uint8_t     sName[16];
    uint16_t    RaceNo;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_GROUP_SOLICIT_REQ`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The server id of the player that invited the client._

### `ActIndex`

_The target index of the player that invited the client._

### `AnonFlag`

_Flag set if the inviter is anon. (`/anon`)_

This value is used to alter (or fully hide) the invite message sent to the player.

  - `0x01` - Flag set if the player inviting the local client is anon. _(`/anon`)_
  - `0x02` - Flag set if the invite message should be hidden.

If the player inviting the local client is not anon, then the client will use their `RaceNo` value to determine their gender to be used with the party invite message.

  - `(anon)` - `PlayerName invites you to join a party.`
  - `Male` - `PlayerName invites you to his party.`
  - `Female` - `PlayerName invites you to her party.`

### `Kind`

_The type of invite._

  - `0` - _Normal party invite._
  - `5` - _Alliance invite._

_**Note:** The client will treat all `Kind` values other than 5 as a normal party invite._

### `sName`

_The name of the player who invited the client._

### `RaceNo`

_The race id of the player who invited the client._
