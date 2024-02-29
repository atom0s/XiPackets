# `GP_SERV_COMMAND_CONFIG`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_CONFIG` |
| **Client Handlers**       | `FsChatFilterServerCallback`, `RecvConf` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00B4` |
| **Size**                  | `0x0018` |

## Description

This packet is sent by the server to populate the clients configuration related system information. This includes various client state flags and chat filters.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (Non-structure usage; this is mapped out manually.)
struct filters1_t
{
    uint32_t    say                                 : 1;
    uint32_t    shout                               : 1;
    uint32_t    unused02                            : 1;
    uint32_t    emotes                              : 1;
    uint32_t    special_actions_started_on_by_you   : 1;
    uint32_t    special_action_effects_on_by_you    : 1;
    uint32_t    attacks_by_you                      : 1;
    uint32_t    missed_attacks_by_you               : 1;
    uint32_t    attacks_you_evade                   : 1;
    uint32_t    damage_you_take                     : 1;
    uint32_t    special_action_effects_on_by_npcs   : 1;
    uint32_t    attacks_by_npcs                     : 1;
    uint32_t    missed_attacks_by_npcs              : 1;
    uint32_t    special_action_effects_on_by_party  : 1;
    uint32_t    attacks_by_party                    : 1;
    uint32_t    missed_attacks_by_party             : 1;
    uint32_t    attacks_evaded_by_party             : 1;
    uint32_t    damage_taken_by_party               : 1;
    uint32_t    special_action_effects_on_by_allies : 1;
    uint32_t    attacks_by_allies                   : 1;
    uint32_t    missed_attacks_by_allies            : 1;
    uint32_t    attacks_evaded_by_allies            : 1;
    uint32_t    damage_taken_by_allies              : 1;
    uint32_t    special_actions_started_on_by_party : 1;
    uint32_t    special_actions_started_on_by_allies: 1;
    uint32_t    special_actions_started_on_by_npcs  : 1;
    uint32_t    others_synthesis_and_fishing_results: 1;
    uint32_t    lot_results                         : 1;
    uint32_t    attacks_by_others                   : 1;
    uint32_t    missed_attacks_by_others            : 1;
    uint32_t    unused30                            : 1;
    uint32_t    unused31                            : 1;
};

// PS2: (Non-structure usage; this is mapped out manually.)
struct filters2_t
{
    uint32_t    attacks_evaded_by_others            : 1;
    uint32_t    damage_taken_by_others              : 1;
    uint32_t    special_action_effects_on_by_others : 1;
    uint32_t    special_actions_started_on_by_others: 1;
    uint32_t    attacks_by_foes                     : 1;
    uint32_t    missed_attacks_by_foes              : 1;
    uint32_t    attacks_evaded_by_foes              : 1;
    uint32_t    damage_taken_by_foes                : 1;
    uint32_t    special_action_effects_on_by_foes   : 1;
    uint32_t    special_actions_started_on_by_foes  : 1;
    uint32_t    campaign_related_data               : 1;
    uint32_t    tell_messages_deemed_spam           : 1;
    uint32_t    shout_yell_messages_deemed_spam     : 1;
    uint32_t    unused13                            : 1;
    uint32_t    unused14                            : 1;
    uint32_t    job_specific_emote                  : 1;
    uint32_t    yell                                : 1;
    uint32_t    messages_from_alter_egos            : 1;
    uint32_t    unused18                            : 1;
    uint32_t    assist_j                            : 1;
    uint32_t    assist_e                            : 1;
    uint32_t    unused21                            : 1;
    uint32_t    unused22                            : 1;
    uint32_t    unused23                            : 1;
    uint32_t    unused24                            : 1;
    uint32_t    unused25                            : 1;
    uint32_t    unused26                            : 1;
    uint32_t    unused27                            : 1;
    uint32_t    unused28                            : 1;
    uint32_t    unused29                            : 1;
    uint32_t    unused30                            : 1;
    uint32_t    unused31                            : 1;
};

// PS2: SAVE_CONF
struct SAVE_CONF
{
    uint8_t     InviteFlg           : 1;    // PS2: InviteFlg
    uint8_t     AwayFlg             : 1;    // PS2: AwayFlg
    uint8_t     AnonymityFlg        : 1;    // PS2: AnonymityFlg
    uint8_t     Language            : 2;    // PS2: Language
    uint8_t     unknown05           : 3;    // PS2: GmLevel

    uint8_t     unknown08           : 1;    // PS2: InvisFlg
    uint8_t     unknown09           : 1;    // PS2: InvulFlg
    uint8_t     unknown10           : 1;    // PS2: IgnoreFlg
    uint8_t     SysMesFilterLevel   : 2;    // PS2: SysMesFilterLevel
    uint8_t     unknown13           : 1;    // PS2: GmNoPrintFlg
    uint8_t     AutoTargetOffFlg    : 1;    // PS2: AutoTargetOffFlg
    uint8_t     AutoPartyFlg        : 1;    // PS2: AutoPartyFlg

    uint8_t     unknown16           : 8;    // PS2: JailNo

    uint8_t     MentorUnlockedFlg   : 1;    // PS2: (New; previously padding byte.)
    uint8_t     MentorFlg           : 1;    // PS2: (New; previously padding byte.)
    uint8_t     NewAdventurerOffFlg : 1;    // PS2: (New; previously padding byte.)
    uint8_t     DisplayHeadOffFlg   : 1;    // PS2: (New; previously padding byte.)
    uint8_t     unknown28           : 1;    // PS2: (New; previously padding byte.)
    uint8_t     RecruitFlg          : 1;    // PS2: (New; previously padding byte.)
    uint8_t     unused              : 2;    // PS2: (New; previously padding byte.)

    filters1_t  MassageFilter;              // PS2: MassageFilter
    filters2_t  MassageFilter2              // PS2: MassageFilter2
    uint16_t    PvpFlg;                     // PS2: (New; did not exist.)
    uint8_t     AreaCode;                   // PS2: (New; did not exist.)
};

// PS2: (New; did not exist.)
struct languages_t
{
    uint8_t     Japanese    : 1;
    uint8_t     English     : 1;
    uint8_t     German      : 1;
    uint8_t     French      : 1;
    uint8_t     Other       : 1;
    uint8_t     unused      : 3;
};

// PS2: GP_SERV_CONFIG
struct GP_SERV_CONFIG
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    SAVE_CONF   ConfData;       // PS2: ConfData
    uint8_t     unknown00;      // PS2: GmLevel
    languages_t PartyLanguages; // PS2: (New; did not exist.)
    uint8_t     unknown01[3];   // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_CONFIG`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ConfData`

_The clients configuration data._

### `unknown00`

_Unknown._

This array of unknown data is not used by the client at all. However, it does not align to normal padding so it is assumed it may hold actual data sent by the server under certain conditions. According to the PS2 beta information, one of the bytes in this array may hold the GM level but this is not testable.

## Structure Fields (`SAVE_CONF`)

### `InviteFlg`

_Flag set if the client is looking for party._ (`/invite`)

### `AwayFlg`

_Flag set if the client is away._ (`/away`)

### `AnonymityFlg`

_Flag set if the client is anon._ (`/anon`)

### `Language`

_Unknown._

This value was originally used for the selected party language. This has been replaced with the newer `PartyLanguage` value instead.

This value does not appear to be used any longer, all tested characters have this set to `3`.

### `unknown05`

_Unknown._

This value was referred to as `GmLevel` on the PS2 beta. There is no way to test this on a normal client.


### `unknown08`

_Unknown._

This value was referred to as `InvisFlg` on the PS2 beta.

_Note: This is a GM related flag; the client does not use it any longer._

### `unknown09`

_Unknown._

This value was referred to as `InvulFlg` on the PS2 beta.

_Note: This is a GM related flag; the client does not use it any longer._

### `unknown10`

_Unknown._

This value was referred to as `IgnoreFlg` on the PS2 beta.

_Note: This is a GM related flag; the client does not use it any longer._

### `SysMesFilterLevel`

_The clients system level chat filters value._

  - `0` - _All system level filters are off._
  - `1` - _System Lv. 1 ("Check" notices) enabled._
  - `2` - _System Lv. 2 (Bazaar notices) enabled._
  - `3` - _System Lv. 3 (Countdowns) enabled._

_**Note:** These flags stack when enabled, meaning if you enable Lv. 3, then Lv. 1 and 2 are also enabled._

### `unknown13`

_Unknown._

This value was referred to as `GmNoPrintFlg` on the PS2 beta.

_Note: This is a GM related flag; the client does not use it any longer._

### `AutoTargetOffFlg`

_Flag set if the client has auto-target disabled._ (`/autotarget`)

### `AutoPartyFlg`

_Flag set if the client is looking for party using the auto-group system._ (`/autogroup`)

### `MentorUnlockedFlg`

_Flag set if the client has unlocked mentor status._

### `MentorFlg`

_Flag set if the client has enabled mentor status._ (`/mentor`)

### `NewAdventurerOffFlg`

_Flag set if the client has disabled the 'New Adventurer' status. (Red question mark.)_

### `DisplayHeadOffFlg`

_Flag set if the client has disabled displaying headgear._ (`/displayhead`)

### `unknown28`

_Unknown._

### `RecruitFlg`

_Flag set if the client has enabled recruitment requests._ (`/request`)

### `unused`

_Unused bits._

### `MassageFilter`

_The clients chat filters._

_See the `filters1_t` structure for more information._

### `MassageFilter2`

_The clients chat filters. (2)_

_See the `filters2_t` structure for more information._

### `PvpFlg`

_Flag that states if the client is participating in pvp related content. (ie. Ballista)_

This flag is set when the client is active in pvp related content. During Ballista, the client uses this flag to determine if the default linkshell chat should be overridden with the Ballista nation chat instead. This will remove the linkshell chat prefix _(`[1]`, `[2]`, `[3]`)_ as well as replace the linkshell name preview header with the clients registered Ballista nation name.

### `AreaCode`

_The client area code._

This value is used when making GM reports. It will be sent back to the server as part of the GM message.
