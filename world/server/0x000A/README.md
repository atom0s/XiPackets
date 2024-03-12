# `GP_SERV_COMMAND_LOGIN`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_LOGIN` |
| **Client Handler**        | `RecvLogIn` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x000A` |
| **Size**                  | `0x0104` |

## Description

This packet is sent by the server to respond to a client login request (`0x000A`).

The client uses the information from this response to initialize the current `GC_ZONE` instance, preparing it for usage when the zone is initialized and loaded.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_POS_HEAD
struct GP_SERV_POS_HEAD
{
    uint32_t            UniqueNo;           // PS2: UniqueNo
    uint16_t            ActIndex;           // PS2: ActIndex
    uint8_t             padding00;          // PS2: (Removed; was SendFlg.)
    int8_t              dir;                // PS2: dir
    float               x;                  // PS2: x
    float               z;                  // PS2: y
    float               y;                  // PS2: z
    uint32_t            flags1;             // PS2: (Multiple fields; bits.)
    uint8_t             Speed;              // PS2: Speed
    uint8_t             SpeedBase;          // PS2: SpeedBase
    uint8_t             HpMax;              // PS2: HpMax
    uint8_t             server_status;      // PS2: server_status
    uint32_t            flags2;             // PS2: (Multiple fields; bits.)
    uint32_t            flags3;             // PS2: (Multiple fields; bits.)
    uint32_t            flags4;             // PS2: (Multiple fields; bits.)
    uint32_t            BtTargetID;         // PS2: BtTargetID
};

// PS2: SAVE_LOGIN_STATE
enum class SAVE_LOGIN_STATE : uint32_t
{
    SAVE_LOGIN_STATE_NONE           = 0,
    SAVE_LOGIN_STATE_MYROOM         = 1,
    SAVE_LOGIN_STATE_GAME           = 2,
    SAVE_LOGIN_STATE_POLEXIT        = 3,
    SAVE_LOGIN_STATE_JOBEXIT        = 4,
    SAVE_LOGIN_STATE_POLEXIT_MYROOM = 5,
    SAVE_LOGIN_STATE_END            = 6
};

// PS2: GP_MYROOM_DANCER
struct GP_MYROOM_DANCER_PKT
{
    uint16_t            mon_no;             // PS2: mon_no
    uint16_t            face_no;            // PS2: face_no
    uint8_t             mjob_no;            // PS2: mjob_no
    uint8_t             hair_no;            // PS2: hair_no
    uint8_t             size;               // PS2: size
    uint8_t             sjob_no;            // PS2: sjob_no
    uint32_t            get_job_flag;       // PS2: get_job_flag
    int8_t              job_lev[16];        // PS2: job_lev
    uint16_t            bp_base[7];         // PS2: bp_base
    int16_t             bp_adj[7];          // PS2: bp_adj
    int32_t             hpmax;              // PS2: hpmax
    int32_t             mpmax;              // PS2: mpmax
    uint8_t             sjobflg;            // PS2: sjobflg
    uint8_t             unknown00[3];       // Unknown
};

// PS2: SAVE_CONF
struct SAVE_CONF_PKT
{
    uint32_t            unknown00[3];       // PS2: (Multiple fields; bits.)
};

// PS2: GP_SERV_LOGIN
struct GP_SERV_LOGIN
{
    uint16_t                id: 9;
    uint16_t                size: 7;
    uint16_t                sync;
    GP_SERV_POS_HEAD        PosHead;            // PS2: PosHead
    uint32_t                ZoneNo;             // PS2: ZoneNo
    uint32_t                ntTime;             // PS2: ntTime
    uint32_t                ntTimeSec;          // PS2: ntTimeSec
    uint32_t                GameTime;           // PS2: GameTime
    uint16_t                EventNo;            // PS2: EventNo
    uint16_t                MapNumber;          // PS2: MapNumber
    uint16_t                GrapIDTbl[9];       // PS2: GrapIDTbl
    uint16_t                MusicNum[5];        // PS2: MusicNum
    uint16_t                SubMapNumber;       // PS2: SubMapNumber
    uint16_t                EventNum;           // PS2: EventNum
    uint16_t                EventPara;          // PS2: EventPara
    uint16_t                EventMode;          // PS2: EventMode
    uint16_t                WeatherNumber;      // PS2: WeatherNumber
    uint16_t                WeatherNumber2;     // PS2: WeatherNumber2
    uint32_t                WeatherTime;        // PS2: WeatherTime
    uint32_t                WeatherTime2;       // PS2: WeatherTime2
    uint32_t                WeatherOffsetTime;  // PS2: WeatherOffsetTime
    uint32_t                ShipStart;          // PS2: ShipStart
    uint16_t                ShipEnd;            // PS2: ShipEnd
    uint16_t                IsMonstrosity;      // PS2: (New; did not exist.)
    SAVE_LOGIN_STATE        LoginState;         // PS2: LoginState
    char                    name[16];           // PS2: name
    int32_t                 certificate[2];     // PS2: certificate
    uint16_t                unknown00;          // Unknown
    uint16_t                ZoneSubNo;          // PS2: (New; did not exist.)
    uint32_t                PlayTime;           // PS2: PlayTime
    uint32_t                DeadCounter;        // PS2: DeadCounter
    uint8_t                 MyroomSubMapNumber; // PS2: (New; did not exist.)
    uint8_t                 unknown01;          // Unknown
    uint16_t                MyroomMapNumber;    // PS2: MyroomMapNumber
    uint16_t                SendCount;          // PS2: SendCount
    uint8_t                 MyRoomExitBit;      // PS2: MyRoomExitBit
    uint8_t                 MogZoneFlag;        // PS2: MogZoneFlag
    GP_MYROOM_DANCER_PKT    Dancer;             // PS2: Dancer
    SAVE_CONF_PKT           ConfData;           // PS2: ConfData
    uint32_t                Ex;                 // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_LOGIN`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `PosHead`

_Sub-structure containing general update information about the local client._

_See structure field breakdown below for more information._

### `ZoneNo`

_The zone number._

### `ntTime`

_The time system time value in seconds._

### `ntTimeSec`

_The time system count value._

### `GameTime`

_The game time value._

### `EventNo`

_The event no value._

This value is used with `EventNum` to determine which event data to be loaded.

### `MapNumber`

_The map number._

This value will generally match the `ZoneNo` value for most zones.

### `GrapIDTbl`

_The clients equipment model visual ids._

  - `GrapIDTbl[0]` - _The clients race and hair ids._
  - `GrapIDTbl[1]` - _The clients head equipment visual model id._
  - `GrapIDTbl[2]` - _The clients body equipment visual model id._
  - `GrapIDTbl[3]` - _The clients hands equipment visual model id._
  - `GrapIDTbl[4]` - _The clients legs equipment visual model id._
  - `GrapIDTbl[5]` - _The clients feet equipment visual model id._
  - `GrapIDTbl[6]` - _The clients main equipment visual model id._
  - `GrapIDTbl[7]` - _The clients sub equipment visual model id._
  - `GrapIDTbl[8]` - _The clients ranged equipment visual model id._

### `MusicNum`

_The array of music values to be used for the various purposes within the zone._

  - `MusicNum[0]` - _Zone Music: Day_
  - `MusicNum[1]` - _Zone Music: Night_
  - `MusicNum[2]` - _Battle Music: Solo_
  - `MusicNum[3]` - _Battle Music: Party_
  - `MusicNum[4]` - _Mount Music_

### `SubMapNumber`

_The sub map number._

This value represents the inner-region within a zone that the player is located within. If the zone does not have sub-regions, then this value will generally be 0. These regions can be used to separate the client from within common reused events such as airships. _(For example, the airship counters/rooms/docks in Port Jeuno are each separate regions.)_

### `EventNum`

_The event num value._

This value is used with `EvetNo` to determine which event data to be loaded.

### `EventPara`

_The event para value._

This value is used as the event id to determine which event opcode block to execute.

### `EventMode`

_The event mode._

This value is used as a set of flags for the event system.

### `WeatherNumber`, `WeatherNumber2`, `WeatherTime`, `WeatherTime2`, `WeatherOffsetTime`

_The zone weather values._

When the client is initializing the zone (`FUNC_ZoneSetUp`) it will make use of these values to prepare the zones time and weather related functionality. The client will initialize the zone in the following manner with these values:

```cpp
XiDateTime dt_real_frame{}; // Initializes time values to 0..
XiDateTime dt_frame{};      // Initializes time values to 0..

PTR_WeatherStartTime    = PTR_pGlobalNowZone->WeatherTime2;
PTR_WeatherNumber       = PTR_pGlobalNowZone->WeatherNumber2;
PTR_WeatherOffsetTime   = HIWORD(PTR_pGlobalNowZone->WeatherOffsetTime);

FUNC_XiDateTime_SetTickRealFrame(&dt_real_frame, 60 * PTR_WeatherStartTime);
FUNC_XiDateTime_SetTickFrame(&dt_frame, 3600 * PTR_WeatherOffsetTime);

auto wid = FUNC_Helper_GetWeatherResId(PTR_Zone, PTR_WeatherNumber);
FUNC_XiArea_SetWeather(PTR_Zone, wid, dt_real_frame, dt_frame);

dt_real_frame   = XiDateTime(0); // Initializes time values to 0..
dt_frame        = XiDateTime(0); // Initializes time values to 0..

PTR_WeatherStartTime    = PTR_pGlobalNowZone->WeatherTime;
PTR_WeatherNumber       = PTR_pGlobalNowZone->WeatherNumber;
PTR_WeatherOffsetTime   = PTR_pGlobalNowZone->WeatherOffsetTime;

auto wid = FUNC_Helper_GetWeatherResId(PTR_Zone, PTR_WeatherNumber);
FUNC_XiArea_SetWeather(PTR_Zone, wid, dt_real_frame, dt_frame);
```

### `ShipStart`, `ShipEnd`

_The ship start and end time values to initialize the ship system with._

When the client is initializing the zone (`FUNC_ZoneSetUp`) it will make use of these values to prepare the zones ship system. The values for this are used as follows:

```cpp
if (PTR_pGlobalNowZone->ShipStart)
{
    if (PTR_pGlobalNowZone->ShipEnd)
    {
        if (PTR_pGlobalNowZone->ZoneFlags & 0x20)
            FUNC_ShipInitReverse(PTR_pGlobalNowZone->ShipStart, PTR_pGlobalNowZone->ShipEnd, HIBYTE(PTR_pGlobalNowZone->ZoneFlags4));
        else
            FUNC_ShipInit(PTR_pGlobalNowZone->ShipStart, PTR_pGlobalNowZone->ShipEnd, HIBYTE(PTR_pGlobalNowZone->ZoneFlags4));
    }
}
```

The `FUNC_ShipInit` and `FUNC_ShipInitReverse` functions take the given timing values and prepares the ship time in the following manner:

```cpp
void __cdecl FUNC_ShipInit(int32_t ship_start, int32_t ship_end, int32_t path_num)
{
    PTR_ship_path_number    = path_num;
    PTR_reverse_flag        = 0; // Set to 1 if calling `FUNC_ShipInitReverse`.

    const auto val1 = 60.0 * (ship_end - ship_start);
    const auto val2 = FUNC_ntGameTimeGet() - ship_start;

    PTR_ship_v = 1.0 / val1;
    PTR_ship_t = val2 * 60.0 / val1;
}
```

### `IsMonstrosity`

_Flag that states if Monstrosity is active._

### `LoginState`

_The clients login state._

### `name`

_The clients character name._

### `certificate`

_The clients certificate values._

These values are unique to the current client. The client sends these values back to the server with every packet. _(Part of the `0x001C` bytes of header data with each packet.)_ These values **MUST** match what the client was last sent from the server, otherwise it will be disconnected from the server. The server checks this value constantly and will begin to R0 the client as soon as its invalid.

> [!NOTE]
> The manner in which these values are generated or are connected to the client are unknown at this time. It may just be completely random, or it may be connected to other values in some manner.

### `unknown00`

_The purpose of this value is unknown._

The client stores this value in the main `GC_ZONE` instance, but never uses it afterward. It has been observed to match the value sent as `ZoneSubNo` or usually set to `0`.

### `ZoneSubNo`

_The zone sub number value._

This value is used for zones that are instanced or have multiple inner maps in the same area. For example, the battle content Sortie makes use of several zones that contain sub-areas using this value. To load the client into the area `Outer Ra'Kaznar [U2]` for Sortie, the client would set:

  - `ZoneNo` to `133`
  - `MapNumber` to `133`
  - `ZoneSubNo` to `1031`

_The client uses this value when calculating what DAT files to be loaded for the zone._

### `PlayTime`

_The clients play time offset value._

The client calculates the value to be used when displaying the playtime as:

```cpp
bool __cdecl FUNC_gcZonePlayTimeGet(uint32_t *ptime)
{
    if (!PTR_pGlobalNowZone)
        return false;
    *ptime = PTR_pGlobalNowZone->PlaySys.PlayTime - PTR_pGlobalNowZone->PlaySys.LoginTime + FUNC_ntGameTimeGet();
    return true;
}
```

### `DeadCounter`

_The client death counter value._

The client calculates and stores this value as:

```cpp
zone->DeadCounter = pkt->DeadCounter / 60 + FUNC_ntGameTimeGet();
```

### `MyroomSubMapNumber`

_The clients mog house sub map number._

The client calculates and stores this value as:

```cpp
zone->MyroomSubMapNumber = (pkt->MyroomSubMapNumber >> 1) & 3;
```

_The client uses this value in various checks to determine if you are allowed to place/move furniture, which kind of exit menu is displayed, etc._

### `unknown01`

_The purpose of this value is unknown._

The client stores this value in the main `GC_ZONE` instance, but never uses it afterward.

### `MyroomMapNumber`

_The clients mog house map number._

This value represents the model id of the area to be loaded for the players mog house. The value will vary based on some additional circumstances. For example, if the player has their mog house registered to one of the main three nations, then the value will change based on if they are aligned to that nation or if they are a guest.

| Nation | Home | Guest |
| ---: | --- | --- |
| `San d'Oria`  | `0x0121` | `0x0101` |
| `Bastok`      | `0x0122` | `0x0102` |
| `Windurst`    | `0x0123` | `0x0120` |

Further, the value will differ if the player is accessing the second floor of their house.

| Nation | Value |
| ---: | --- |
| `San d'Oria`  | `0x0267` |
| `Bastok`      | `0x0268` |
| `Windurst`    | `0x0269` |
| `Patio`       | `0x026A` |

Additional locations for residence include:

| Zone | Value |
| ---: | --- |
| `Aht Urhgan (Whitegate)`  | `0x00D6` |
| `Ronfaure Front`          | `0x00BD` |
| `Gustaberg Front`         | `0x00C7` |
| `Sarutabaruta Front`      | `0x00DB` |
| `Jeuno`                   | `0x0100` |
| `Adoulin`                 | `0x0124` |
| `Mog Garden`              | `0x02D4` |
| `Feretory`                | `0x02D9` |

### `SendCount`

_Entity (NPC) load count limiter value._

The server can use this value to lock the client in place and prevent movement upon first entering a zone. This value represents the number of flagged NPC entities _(referred to as `king`)_ that must load _(via `0x000E` packets sent from the server)_ before the client is allowed to move. When the client is put into this state, rendering additional entities is paused and allows for all needed entities to populate. _(This is generally used to allow on-zone-in cutscenes to load their needed entities data first before playing the cutscene.)_

In the event the client fails to load the expected `SendCount` number of entities, the client has a built-in timeout as a failsafe when put into this mode. If the client does not reach the expected count within `6` seconds, it will be released and the event will play regardless.

### `MyRoomExitBit`

_Value that controls the exit menu type when leaving the mog house._

The naming of this variable has likely changed since PS2 beta as it no longer is treated as a single bit flag. Instead, it is now a value _(ranging from `0` to `9`)_ which determines the kind of menu that will be displayed when leaving each of the various residental areas of the game.

| Value | Description |
| --- | --- |
| `0` | _Default; display normal exit menu._ |
| `1` | _Enables extended exit menu when leaving `San d'Oria` residence._ |
| `2` | _Enables extended exit menu when leaving `Bastok` residence._ |
| `3` | _Enables extended exit menu when leaving `Windurst` residence._ |
| `4` | _Enables extended exit menu when leaving `Jeuno` residence._ |
| `5` | _Enables extended exit menu when leaving `Aht Urhgan (Whitegate)` residence_ |
| `6` | _Enables extended exit menu when leaving `Ronfaure Front` residence._ |
| `7` | _Enables extended exit menu when leaving `Gustaberg Front` residence._ |
| `8` | _Enables extended exit menu when leaving `Sarutabaruta Front` residence._ |
| `9` | _Enables extended exit menu when leaving `Adoulin` residence._ |

### `MogZoneFlag`

_Flag that states if the current zone has access to the mog menu._

This flag is set in areas that have nomad Moogles.

### `Dancer`

_Sub-structure containing general update information about the local clients character._

_See structure field breakdown below for more information._

### `ConfData`

_Sub-structure containing client configuration data._

_See structure field breakdown below for more information._

### `Ex`

_The purpose of this value is unknown._

This is a debug related value that is only used in a disabled/hidden debug function in the client. It is simply printed to the screen and is not used otherwise.

## Structure Fields (`GP_SERV_POS_HEAD`)

### `UniqueNo`

_The local players server id._

### `ActIndex`

_The local players target index._

### `padding00`

_Padding; unused._

This value originally was used as the `SendFlg`. However, this packet no longer uses this value and is now considered padding in this case.

### `dir`

_The local players rotation direction._

This value is packed when being sent in packets and is convertable to its proper FFXI based radian as follows:

```cpp
double __cdecl FUNC_enDirNetToCli(uint8_t dir)
{
    return dir * 6.283185 * 0.00390625;
}
```

### `x`

_The local players X position._

### `z`

_The local players Z position._

### `y`

_The local players Y position._

### `flags1`

_Bit flags holding different purposes about the local player._

TODO: Finish breaking down the bits stored in this value.

### `Speed`

_The local players speed._

### `SpeedBase`

_The local players speed base._

### `HpMax`

_The local players health percentage._

### `server_status`

_The local players server status._

### `flags2`

_Bit flags holding different purposes about the local player._

TODO: Finish breaking down the bits stored in this value.

### `flags3`

_Bit flags holding different purposes about the local player._

TODO: Finish breaking down the bits stored in this value.

### `flags4`

_Bit flags holding different purposes about the local player._

TODO: Finish breaking down the bits stored in this value.

### `BtTargetID`

_The local players battle target information._

## Enumeration Fields (`SAVE_LOGIN_STATE`)

### `SAVE_LOGIN_STATE_NONE`

_None._

### `SAVE_LOGIN_STATE_MYROOM`

_The local client is within their residence._

### `SAVE_LOGIN_STATE_GAME`

_The local client is within the normal game areas._

### `SAVE_LOGIN_STATE_POLEXIT`

_The local client is performing a POL exit._

This feature has been deprecated and no longer functions.

### `SAVE_LOGIN_STATE_JOBEXIT`

_The local client is exiting the job menu._

This state is no longer used as the old job menu has been removed.

### `SAVE_LOGIN_STATE_POLEXIT_MYROOM`

_The local client is performing a POL exit from within their residence._

This feature has been deprecated and no longer functions.

### `SAVE_LOGIN_STATE_END`

_N/A._

## Structure Fields (`GP_MYROOM_DANCER_PKT`)

### `mon_no`

_The local players race id._

### `face_no`

_The local players face id._

### `mjob_no`

_The local players main job id._

### `hair_no`

_The local players hair id._

### `size`

_The local players model size._

### `sjob_no`

_The local players sub job id._

### `get_job_flag`

_The local players unlocked job flags._

### `job_lev`

_The array of the local players job levels._

### `bp_base`

_The local players base stats._

### `bp_adj`

_The local players stat adjustments._

### `hpmax`

_The local players max health._

### `mpmax`

_The local players max mana._

### `sjobflg`

_The local players sub job flag._

This value is used to determine if the player has unlocked and can change their sub job.

### `unknown00`

_Unknown._

It is assumed this array is padding and unused.

## Structure Fields (`SAVE_CONF_PKT`)

### `unknown00`

_Unknown. (TODO later.)_

This array of data holds the clients server-side savable configurations. This includes things like:

  - Current party seeking flag.
  - Current away status.
  - Current language.
  - Current auto-target setting.
  - Current message filter settings.
  - etc.

Based on the PS2 exported data:

  - `unknown00[0]` - _Holds various bit flags and bit data for invite flag, away flag, anon flag, language, GM level, auto-target, etc.._
  - `unknown00[1]` - _Chat filter bits._
  - `unknown00[2]` - _Chat filter bits._

## Additional Information

The `GP_MYROOM_DANCER_PKT` and `SAVE_CONF_PKT` structures are postfixed with the `_PKT` naming as these do not align to the actual internal structures that share the same name. This packet has changed since the PS2 beta and does not directly align to the non-packet variants of the same named structures. Instead, they are only partial copies from the packet into memory.
