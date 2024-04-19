# `GP_CLI_COMMAND_CONFIG`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_CONFIG` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00DC` |
| **Size**                  | `0x0014` |

## Description

This packet is sent by the client when changing certain configuration related values or settings.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_CONFIG
struct GP_CLI_CONFIG
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     InviteFlg           : 1;    // PS2: InviteFlg
    uint8_t     AwayFlg             : 1;    // PS2: AwayFlg
    uint8_t     AnonymityFlg        : 1;    // PS2: AnonymityFlg
    uint8_t     Language            : 2;    // PS2: Language
    uint8_t     unused05            : 3;    // PS2: GmLevel
    uint8_t     unused08            : 1;    // PS2: InvisFlg
    uint8_t     unused09            : 1;    // PS2: InvulFlg
    uint8_t     unused10            : 1;    // PS2: IgnoreFlg
    uint8_t     unused11            : 2;    // PS2: SysMesFilterLevel
    uint8_t     unused13            : 1;    // PS2: GmNoPrintFlg
    uint8_t     AutoTargetOffFlg    : 1;    // PS2: AutoTargetOffFlg
    uint8_t     AutoPartyFlg        : 1;    // PS2: AutoPartyFlg
    uint8_t     unused16            : 8;    // PS2: JailNo
    uint8_t     unused24            : 1;    // PS2: (New; previously padding byte.)
    uint8_t     MentorFlg           : 1;    // PS2: (New; previously padding byte.)
    uint8_t     NewAdventurerOffFlg : 1;    // PS2: (New; previously padding byte.)
    uint8_t     DisplayHeadOffFlg   : 1;    // PS2: (New; previously padding byte.)
    uint8_t     unused28            : 1;    // PS2: (New; previously padding byte.)
    uint8_t     RecruitFlg          : 1;    // PS2: (New; previously padding byte.)
    uint8_t     unused30            : 2;    // PS2: (New; previously padding byte.)
    uint32_t    unused00;                   // PS2: (Other misc data.)
    uint32_t    unused01;                   // PS2: (Other misc data.)
    uint8_t     SetFlg;                     // Ps2: SetFlg
    uint8_t     padding00[3];               // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_CONFIG`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `InviteFlg`

_Flag set if the client is looking for party._ (`/invite`)

### `AwayFlg`

_Flag set if the client is away._ (`/away`)

### `AnonymityFlg`

_Flag set if the client is anon._ (`/anon`)

### `Language`

This value was originally used for the selected party language. This has been replaced with the newer `PartyLanguage` value instead.

This value does not appear to be used any longer, all tested characters have this set to `3`.

### `AutoTargetOffFlg`

_Flag set if the client has auto-target disabled._ (`/autotarget`)

### `AutoPartyFlg`

_Flag set if the client is looking for party using the auto-group system._ (`/autogroup`)

### `MentorFlg`

_Flag set if the client has enabled mentor status._ (`/mentor`)

### `NewAdventurerOffFlg`

_Flag set if the client has disabled the 'New Adventurer' status. (Red question mark.)_

### `DisplayHeadOffFlg`

_Flag set if the client has disabled displaying headgear._ (`/displayhead`)

### `RecruitFlg`

_Flag set if the client has enabled recruitment requests._ (`/request`)

### `SetFlg`

_Flag that states if the given setting has been turned on or off._

  - `1` - _The setting has been turned on._
  - `2` - _The setting has been turned off._

## Additional Information

This packet is used to turn an individual configuration setting on or off. While the packet makes use of the same `SAVE_CONF` structure layout as other packets, only some of its data is actually used and populated. The layout in this packet above is a slimmed down version with various bits of data renamed to 'unused' as it only makes use of the still-named bits.

Each time the client uses certain commands or toggles different configuration settings throughout the client, this packet will be set with the given flag set or removed and the `SetFlg` field set to the expected value for the settings current state on the client.
