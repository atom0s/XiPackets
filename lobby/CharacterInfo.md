# Character Information Structure

The character information structure, _referred to by the PS2 beta as `TC_OPERATION_MAKE`_, is a structure that contains information related to an individual Final Fantasy XI character. This structure is used within multiple packets sent to and from the `lobby` server.

For example, when creating a new character, the client will send this block of data in the `RequestCreateChr` packet with some of the information filled in. _(The bare minimum required to create a character.)_ When the client requests the available character list via `RequestGetChr`, the server will respond with `ResponseGetChr` which contains `n` number of instances of this structure for each available character. _The servers' responses that use this structure will have most of the information populated._

## Structure

```cpp
struct TC_OPERATION_MAKE
{
    uint16_t    mon_no;
    uint8_t     mjob_no;
    uint8_t     sjob_no;
    uint16_t    face_no;
    uint8_t     town_no;
    uint8_t     gen_flag;
    uint8_t     hair_no;
    uint8_t     size;
    uint16_t    world_no;
    uint16_t    GrapIDTbl[8];
    uint8_t     zone_no;
    uint8_t     mjob_level;
    uint8_t     open_flag;
    uint8_t     GMCallCounter;
    uint16_t    version;
    uint8_t     skill1;
    uint8_t     zone_no2;
    uint8_t     TC_OPERATION_WORK_USER_RANK_LEVEL_SD_;
    uint8_t     TC_OPERATION_WORK_USER_RANK_LEVEL_BS_;
    uint8_t     TC_OPERATION_WORK_USER_RANK_LEVEL_WS_;
    uint8_t     ErrCounter;
    uint16_t    TC_OPERATION_WORK_USER_FAME_SD_COMMON_;
    uint16_t    TC_OPERATION_WORK_USER_FAME_BS_COMMON_;
    uint16_t    TC_OPERATION_WORK_USER_FAME_WS_COMMON_;
    uint16_t    TC_OPERATION_WORK_USER_FAME_DARK_GUILD_;
    uint32_t    PlayTime;
    uint32_t    get_job_flag;
    uint8_t     job_lev[16];
    uint32_t    FirstLoginDate;
    uint32_t    Gold;
    uint8_t     skill2;
    uint8_t     skill3;
    uint8_t     skill4;
    uint8_t     skill5;
    uint32_t    ChatCounter;
    uint32_t    PartyCounter;
    uint8_t     skill6;
    uint8_t     skill7;
    uint8_t     skill8;
    uint8_t     skill9;
};
```

## Structure Fields

### mon_no

_The characters race id._

  - `0` - `Invalid`
  - `1` - `Hume (Male)`
  - `2` - `Hume (Female)`
  - `3` - `Elvaan (Male)`
  - `4` - `Elvaan (Female)`
  - `5` - `Tarutaru (Male)`
  - `6` - `Tarutaru (Female)`
  - `7` - `Mithra`
  - `8` - `Galka`

### mjob_no

_The characters main job id._

  - `0` - `None`
  - `1` - `Warrior`
  - `2` - `Monk`
  - `3` - `White Mage`
  - `4` - `Black Mage`
  - `5` - `Red Mage`
  - `6` - `Thief`
  - `7` - `Paladin`
  - `8` - `Dark Knight`
  - `9` - `Beastmaster`
  - `10` - `Bard`
  - `11` - `Ranger`
  - `12` - `Samurai`
  - `13` - `Ninja`
  - `14` - `Dragoon`
  - `15` - `Summoner`
  - `16` - `Blue Mage`
  - `17` - `Corsair`
  - `18` - `Puppetmaster`
  - `19` - `Dancer`
  - `20` - `Scholar`
  - `21` - `Geomancer`
  - `22` - `Rune Fencer`

### sjob_no

_The characters sub job id._

### face_no

_The characters face model id._

### town_no

_The characters town id._

  - `0` - `San d'Oria`
  - `1` - `Bastok`
  - `2` - `Windurst`

### gen_flag

_Unknown._

_This value is not used within the client. Only observed to be 0._

### hair_no

_The characters hair model id._

 - `0` - `Hair 1A`
 - `1` - `Hair 1B`
 - `2` - `Hair 2A`
 - `3` - `Hair 2B`
 - `4` - `Hair 3A`
 - `5` - `Hair 3B`
 - `6` - `Hair 4A`
 - `7` - `Hair 4B`
 - `8` - `Hair 5A`
 - `9` - `Hair 5B`
 - `10` - `Hair 6A`
 - `11` - `Hair 6B`
 - `12` - `Hair 7A`
 - `13` - `Hair 7B`
 - `14` - `Hair 8A`
 - `15` - `Hair 8B`

### size

_The characters model size._

 - `0` - `Small`
 - `1` - `Medium`
 - `2` - `Large`

### world_no

_Unknown._

_This value is not used within the client. The server does set this value but unsure of its intended purpose. (It does not match the other world id values.)_

### GrapIDTbl

_The characters model id information._

The indexes into this table are as follows:

  - `0` - `Calculated model id for the characters race, face and hair.`
  - `1` - `Head model id.`
  - `2` - `Body model id.`
  - `3` - `Hands model id.`
  - `4` - `Legs model id.`
  - `5` - `Feet model id.`
  - `6` - `Main (weapon) model id.`
  - `7` - `Sub (weapon) model id.`

The first slot is a calculated model id using the characters choosen race, face and hair ids.\
That calculation is done as:

```cpp
uint16_t model_id = hair_no | (2 * (face_no | (mon_no << 7)));
```

### zone_no

_The characters current zone id._

_Note: This value is used with `zone_no2` in order to properly calculate the full zone id._

```cpp
uint16_t zone_id = zone_no + ((zone_no2 & 1) << 8);
```

### mjob_level

_The characters main job level._

### open_flag

_Unknown._

_This value is not used within the client. It appears to be a flag that states if the character slot is open for creation of a new character. It is observed to be 0 where a character already exists, but 1 otherwise on available slots._

### GMCallCounter

_Unknown._

_This value is not used within the client. The value of this does not appear to be actually connected to the characters real GM call count._

### version

_Unknown._

_This value is not used within the client. It appears to always be set to 2._

### skill1

_Unknown._

_This value is not used within the client. It appears to always be set to 0._

### zone_no2

_The characters current zone id. (hi-byte)_

_Note: This value is used with `zone_no` in order to properly calculate the full zone id._

```cpp
uint16_t zone_id = zone_no + ((zone_no2 & 1) << 8);
```

### TC_OPERATION_WORK_USER_RANK_LEVEL_SD_

_The characters San d'Oria rank value._

### TC_OPERATION_WORK_USER_RANK_LEVEL_BS_

_The characters Bastok rank value._

### TC_OPERATION_WORK_USER_RANK_LEVEL_WS_

_The characters Windurst rank value._

### ErrCounter

_Unknown._

_This value is not used within the client. It appears to always be set to 0._

### TC_OPERATION_WORK_USER_FAME_SD_COMMON_

_The characters San d'Oria fame amount._

_This value is not used within the client. It's value may not be what is expected based on the field name._

### TC_OPERATION_WORK_USER_FAME_BS_COMMON_

_The characters Bastok fame amount._

_This value is not used within the client. It's value may not be what is expected based on the field name._

### TC_OPERATION_WORK_USER_FAME_WS_COMMON_

_The characters Windurst fame amount._

_This value is not used within the client. It's value may not be what is expected based on the field name._

### TC_OPERATION_WORK_USER_FAME_DARK_GUILD_

_The characters fame amount with an unknown group._

_This value is not used within the client. It's value may not be what is expected based on the field name._

### PlayTime

_The characters playtime, in seconds._

### get_job_flag

_The characters unlocked jobs flag mask._

### job_lev

_An array holding the levels of the characters base jobs. (FFXI + RoZ job levels. First slot is invalid/unused and always 1.)_

### FirstLoginDate

_An unknown timestamp that represents when the character first logged in._

_This value appears to be an Epoch timestamp with some means of custom adjustment, potentially to defend against the 2038 bug that will eventually happen._

### Gold

_The characters current gil amount._

### skill2

_Unknown._

_This value is not used within the client._

### skill3

_Unknown._

_This value is not used within the client._

### skill4

_Unknown._

_This value is not used within the client._

### skill5

_Unknown._

_This value is not used within the client._

### ChatCounter

_Unknown._

_This value is not used within the client. The naming of this field does not relate to actual chat usage in-game._

### PartyCounter

_Unknown._

_This value is not used within the client. The naming of this field does not relate to the actual party usage in-game._

### skill6

_Unknown._

_This value is not used within the client._

### skill7

_Unknown._

_This value is not used within the client._

### skill8

_Unknown._

_This value is not used within the client._

### skill9

_Unknown._

_This value is not used within the client._

## General Notes

Because of when this structure is used, during `lobby` server transmissions, most of its data is otherwise useless and unused within the client. It is assumed that Square Enix makes use of this structure solely as a convenience when doing queries for character information on the server side. Rather than build out packets with the specific needed information, they just reuse this single structure. That means that it is likely the client is sent more information that is needed just because its auto-populated during the server-side query.

Several fields in this structure are also entirely unused by the normal client and do not appear to ever be populated on normal retail accounts. This is assumed to be due to potentially being used with the GM tooling instead.

_**Please note:** while the fields are named from their PS2 information, it is not guaranteed that any of these fields are solely used for that given purpose now, or at all. Some fields may have multiple purposes, or be entirely different now as these structures are literally from 2003. Some adjustments and naming have been made to align with the current PC version of the client._

## Usage: `RequestCreateChr`

When the client sends a `RequestCreateChr` request, only parts of the structure are populated.

The following fields are populated:

  - `mon_no`
  - `mjob_no`
  - `face_no`
  - `town_no`
  - `hair_no`
  - `size`
  - `GrapIDTbl` _(`GrapIDTbl[0]` is populated via the calculated information above.)_
  - `mjob_level`

_All other fields are nulled by default._

On retail, the other fields are ignored and not interpreted. Invalid values passed for the above fields will result in an error _(`321`)_ stating the character information is invalid.