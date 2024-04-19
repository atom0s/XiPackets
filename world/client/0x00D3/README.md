# `GP_CLI_COMMAND_FAQ_GMCALL`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_FAQ_GMCALL` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00D3` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the client when interacting with the Help Desk system. More specifically, when selecting an option that will send a request to the server.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_FAQ_GMCALL
struct GP_CLI_FAQ_GMCALL
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    type    : 4;    // PS2: type
    uint16_t    vers    : 4;    // PS2: vers
    uint16_t    pktId   : 8;    // PS2: pktId
    uint16_t    seq     : 7;    // PS2: seq
    uint16_t    eos     : 1;    // PS2: eos
    uint16_t    blkNum  : 8;    // PS2: blkNum
    uint8_t     Data[];         // PS2: (Non-specific structured data.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_FAQ_GMCALL`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `type`

_The packet type._

This value is used to determine the type of GM packet is being sent.

| Type | Purpose |
| --- | --- |
| `1` | _Add History_ |
| `2` | _GM Call_ |
| `3` | _GM Notice_ |

### `vers`

_The packet version._

This value appears to always be set to `0`.

### `pktId`

_The packet id._

This value is the clients current GM call count for its current session. This value auto-increments each time the client begins sending a GM ticket using this packet.

### `seq`

_The packet sequence count._

This value is the sequence counter used for this packet when multiple packets of this type will be sent together to fully populate the needed information for the GM ticket. When the client makes use of an option in the Help Desk menu that will result in a GM ticket being filed, the client will send at least 2 packets together that are part of the same `pktId`. This value is used to mark which packet is which in the same sequence.

### `eos`

_The packet end of sequence flag._

This value is a flag that states if the packet is the final packet in the same sequence. Once this flag is set to `1`, the server can tell when the client has completed their ticket and the overall data can be processed.

### `blkNum`

_The packet block count._

This value is the number of sub-blocks that are contained within the packet.

### `Data`

_The packet data._

This array holds the remaining packet information. This holds the sub-blocks of data that populate the information needed to file the GM ticket with the server. There are multiple kinds of sub-blocks, each separated by a block header, that can hold various information needed for the overall ticket. The `blkNum` value holds the count of blocks that are held in the individual packet, while multiple packets can be present in the same sequence. _(Each packet in the same sequence will have its own `blkNum` value for the blocks contained in the individual packet.)_

## Additional Information

This packet is used by the client when interacting with the Help Desk system, under certain conditions and menu operations. The client will make use of this packet along with packets `0x00D4` and `0x00D5` when making use of the Help Desk system, depending on what action(s) the client is performing.

When the client first opens the Help Desk menu, it will send a `0x00D4` packet. This packet will contain the clients current local `FDtGmParam.Id` value. This value is incremented automatically by the client each time the client makes a param request via the `FDtXiRequestGmParam` function. The server will respond to this packet by sending its own `0x00B5` response. The client will use the response information to populate its `FDtGmParam` structure information by directly copying the packets data into its buffer. _(This information is not actually used by the client, but was potentially for use in telling the client how many tickets ahead of them are in the server queue.)_

Once the menu is open, the client is presented with various options that can lead to different sub-menus. Most of these sub-menus are 'dead ends' and just explain some generic information about the game to the client or offer more detailed player help information. However, some of the sub-menus can lead to allowing the client to request to contact the GM team to help assist with an in-game problem.

The following menu options will result in attempting to contact the GM team:

  - Open the `Main Menu`.
  - Select the `Help Desk` menu.
  - Select the `Help Desk` sub-menu. _(Client will send `0x00D4` packet here!)_
    - Select `I want to report a violation of the PlayOnline user agreement.`.
      - Select `Yes. I have.`.
        - Select `Yes, this is a violation of the user agreement. I would like a GM to investigate.`.
          - Enter a message (optional) and select `Execute`.
      - Select `No. Please elaborate.`.
        - Select `Yes, this is a violation of the user agreement. I would like a GM to mediate.`.
          - Enter a message (optional) and select `Execute`.
    - Select `I can't move my character or log out of the game.`.
      - Select `My character is stuck.`
        - Select `I am stuck and need to get out. My movement is not being restricted by a GM.`.
          - Enter a message (optional) and select `Execute`.
      - Select `Someone is blocking my way.`.
        - Select `Multiple players are conspiring to block my way. I would like a GM to mediate.`.
          - Enter a message (optional) and select `Execute`.
      - Select `Other.`.
        - Select `I would like to call a GM.`.
          - Enter a message (optional) and select `Execute`.

When the client selects one of these options, it will then send multiple copies of this packet that are needed to populate the full GM ticket information. The group of packets sent by the client will all share the same `pktId` letting the server know the data is part of the same ticket. The client will also set the `seq` value of each packet in the order they should be processed. The final packet in the ticket will set `eos` to `1`, letting the server know it should have received all packets needed for the ticket. At a minimum, the client will send at least 2 `0x00D3` packets when filing a ticket. However, more can be sent in the same group of packets if needed when populating the packets various blocks of data.

All of the above methods of filing a GM ticket will make use of the `type` value `2`. After the client has filed a ticket with the help system, exiting the Help Desk menu can result in an additional set of `0x00D3` packets being sent. This additional set of packets will make use of the `type` value `1`. This is used to further populate additional helpful information on the server about the client filing the ticket. Additional information will be included such as if the client is in-game, the clients platform, the clients full version information, and additional character data can be sent to the server. _(This type of packet does not attach to the previous reported ticket `pktId` either, it will have its own incremented id instead.)_

In the event that a GM response to the players ticket through the GM system, the server will send `0x00B6` packet(s) containing the ticket response. When this happens, the client will then start pulsing a notice icon where the client shows the current connection information in the top-right corner of the game screen with a Help Desk button and GM icon. When the client opens the Help Desk menu during this state, it will instead show the current pending response from the GM team instead of the normal menu options. The client can choose to either exit the menu or delete the message. Choosing to delete the message will cause the client to send an `0x00D5` packet.

## Additional Information - `Data` Field

The `Data` field of this packet holds the additional information needed to send to the GM ticket system. This information is broken into sub-blocks of data which can contain various kinds of information depending on the sub-blocks own type. Each of these blocks has a sub-block header which is as follows:

```cpp
struct FFGpGMReportBlockHdr
{
    uint8_t     bkType;     // PS2: bkType
    uint8_t     bkLength;   // PS2: bkLength
    uint16_t    bkOpt;      // PS2: bkOpt
};
```

  - `bkType` - _The sub-block type._
  - `bkLength` - _The sub-block total size. (This includes this header in the size!)_
  - `bkOpt` - _The sub-block options._

Following each sub-block header will be `bkLength - sizeof(FFGpGMReportBlockHdr)` bytes of data holding the sub-blocks information. This information is, itself, separate sub-structures of data depending on the sub-blocks `bkType` value, which is used to determine what kind of data is present in this sub-block.

There are currently `5` types of sub-blocks that are used with this system:

| bkType | Type | Purpose |
| --- | --- | --- |
| `0x00` | `FFGpGMReportCharBlock`      | _Contains the local clients zone, world, and position data._ |
| `0x01` | `(Unknown)`                  | _Unknown. (This appears to be the local notice file version information.)_ |
| `0x02` | `FFGpGMReportECodeStruct`    | _Contains the local clients ticket information._ |
| `0x03` | `(Unknown)`                  | _Contains a string parameter._ |
| `0x04` | `FFGpGMReportLobbyStruct`    | _Contains the local clients past lobby login information._ |

The below information breaks down each sub-block type.

_**Please Note:** Each sub-block starts with its own `FFGpGMReportBlockHdr` structure instance! They are not included in the breakdowns below to save space and not have to be repetitive._

## Additional Information - Sub-Block Type: `0x00`

This sub-block holds the following information:

```cpp
// PS2: FFGpGMReportCharBlock
struct sub_block_00_t
{
    uint16_t    zone;       // PS2: zone
    uint16_t    world;      // PS2: world
    float       cliPos[3];  // PS2: cliPos
    float       srvPos[3];  // PS2: srvPos
};
```

### `zone`

_The clients current zone id._

The client does not set this value.

### `world`

_The clients current world id._

The client does not set this value.

### `cliPos`

_The clients current position information. (Client)_

This is populated with the clients current `entity->Movement->LocalPosition` information.

### `srvPos`

_The clients current position information. (Server)_

This is populated with the clients current `entity->Movement->LastPosition` information.

## Additional Information - Sub-Block Type: `0x01`

This sub-block holds the following information:

```cpp
// PS2: (Unknown type.)
struct sub_block_01_t
{
    uint32_t unknown00;
    uint32_t unknown01[4];
};
```

### `unknown00`

_Unknown._

This value appears to be set to the same version string used within the clients GM notice dataset files, `2003022101`. _(gmNotice.fdt)_

### `unknown01`

_Unknown._

This array of data is unknown at this time. It appears to be some kind of date or version style information.

The values for this array seem to always be the same as well: `0`, `22`, `44`, `4`

## Additional Information - Sub-Block Type: `0x02`

This sub-block holds the following information:

```cpp
// PS2: FFGpGMReportECodeStruct
struct sub_block_02_t
{
    int16_t     code;       // PS2: code
    uint16_t    count;      // PS2: count
    uint32_t    timeCode;   // PS2: timeCode
};
```

### `code`

_Unknown._

This value appears to be some kind of error code. _(It does not appear to be specific to the type of option selected.)_

### `count`

_Unknown._

### `timeCode`

_Unknown._

This value is a Unix timestamp, but its origin is unknown at this time.

## Additional Information - Sub-Block Type: `0x03`

This sub-block holds the following information:

```cpp
// PS2: (Unknown type.)
struct sub_block_03_t
{
    uint8_t Str[];
};
```

### `Str`

_The parameter string._

This value will be the size of the headers `bkLength` value minus 4. The string is null-terminated and 4 byte aligned.

## Additional Information - Sub-Block Type: `0x04`

This sub-block holds the following information:

```cpp
// PS2: FFGpGMReportLobbyStruct
struct FFGpGMReportLobbyStruct
{
    uint16_t    cmd;        // PS2: cmd
    uint16_t    opt;        // PS2: opt
    uint32_t    timeCode;   // PS2: timeCode
    uint32_t    ident;      // PS2: ident
    uint8_t     name[16];   // PS2: name
};

// PS2: (Unknown type.)
struct sub_block_04_t
{
    FFGpGMReportLobbyStruct characters[];
};
```

### `characters`

_The array of last-known lobby logins on this client._

This array holds the list of last known character logins on this client. This can hold upto a maximum of `8` entries. This information is populated from the clients `/SYS/lb%03d.bin` files, which holds the previous lobby operation records.

The size of this array can be determined by the sub-block headers `bkLength` minus 4, then divided by the `sizeof(FFGpGMReportLobbyStruct)`.

### `cmd`

_The lobby operation command._

This value appears to be based on if the character was last created or deleted.

  - `1` - _Create/Created_
  - `2` - _Delete/Deleted_

### `opt`

_The lobby operation option._

This value is set to the characters world id. _(ie. `0x7F` is `Asura`)_

### `timeCode`

_The lobby operation timestamp._

This value is the Unix timestamp of the lobby record creation. _(Obtained via `xiGetTime` function.)_

### `ident`

_The lobby operation identity._

This value is set to the characters server id. _(Character specific server id.)_

### `name`

_The character name._

## Additional Information - Packet Building

When the client is ready to send GM ticket related information to the server, either by the client requesting to call for a GM or the client sending updated history information, it will do so using the function `FDtSendReportX`. This function is responsible for building the entire sequence of `0x00D3` packets that will be sent in the same `pktId` group.

The first thing this function will do is begin preparing the first packet in the sequence. This first packet will always start with a sub-block type `0x00` entry. This entry will have its `cliPos` and `srvPos` information populated with the clients local position information allowing the GM to know where the client is currently located. _(The zone and world information are not populated in this block!)_

Next, the client will then check if the first argument to this function is valid _(`FDtFFXiBrowser*`)_ and if its internal `_hist` object is set. If this is true, then the next sub-block entry in the packet is guaranteed to be a `0x01` type. This holds the version information for the file system of the entire FDt system. At this time, this information appears to always be the same set of values. The main version/timecode value is `2003022101`.

Next, the client will then check the current global system descriptor for the `FDtDataSet` object to see if there are any available string parameters to be added to the ticket. If there are, the client will walk these parameters, adding the needed `0x03` sub-block type entries for each one that is available. If the packet reaches a size that cannot hold the next parameter, then this packet will be sent as-is. When this happens the client will then step the current `seq` counter and continue building a new instance of the `0x00D3` packet for this same group. The client will repeat this until all string parameters have been populated into the packet as needed. _(If during this process the packet size will be at the maximum size or larger, the client will again send the packet as-is and begin another `0x00D3` packet for the group with the `seq` id incremented again.)_

Last, the client will check to see if the client has any previous lobby operations using the `FDtXiExtractLobbyOperationRecord` function. It will query for the last `8` operations that were executed with the lobby. These operations are for character creation/login and deletion which include each of the characters names, world and server ids, and the operation that was last performed. The client will then add a sub-block type `0x04` entry to the packet and populate it with the returned lobby operation information, up to a maximum of `8` entries.

If this is the last packet that will be sent from the client within this group, the `eos` flag will be set to `1` marking the group complete and that the server should process the total data.

## Additional Information - String Parameters

There are a handful of string parameters that the client may populate into this packet when filing a GM ticket, or if the client wishes to send history updates.

This includes the following:

| Category | Name | Value |
| --- | --- | --- |
| `GENERIC`     | `AREACODE`    | _Set to: The clients `PTR_pGlobalNowZone->ConfSys.AreaCode` value._ |
| `GENERIC`     | `CLILANG`     | _Set to: The client language id. (`0` - Japanese, `1` - English, European languages default to English.)_ |
| `GENERIC`     | `SELLANG`     | _Set to: `0`, `1`, or `2`. (Unknown purpose at this time.)_ |
| `GENERIC`     | `PLATFORM`    | _Set to: `win`_ |
| `GENERIC`     | `VERSION`     | _See notes below._ |
| `GAME`        | `INFIELD`     | _Set to: `on` or `off` if the client is in a non-safe area._ |
| `GAME`        | `GMTIME`      | _Set to: `1` or `0` if the client is in GM time or not._ |
| `EXEC`        | `LOGOUT`      | _Set to: `on` if present._ |
| `EXEC`        | `POLEXIT`     | _Set to: `on` if present._ |
| `EXEC`        | `RESCUE`      | _Set to: `on` if present._ |
| `GMREPORT`    | `NOTICE`      | _Set to: `on` if present._ |
| `GMREPORT`    | `GMCALL`      | _Set to: `on` if present._ |
| `GMREPORT`    | `HARASSMENT`  | _Set to: `on` if present._ |
| `GMREPORT`    | `STUCK`       | _Set to: `on` if present._ |
| `GMREPORT`    | `BLOCK`       | _Set to: `on` if present._ |

_**Note:** These strings are shown as their Category and Name in the above table. In the packets, these will be combined together with a single period separator between the two parts. For example, the category `GMREPORT` and name `STUCK` will be the following in the actual packet: `GMREPORT.STUCK` The value will then be appended to this string, separated with a colon. (`:`)_.


The `GENERIC.VERSION` string is comprised of several values formatted into a single string. It consists of 3 parts:

  - The clients language tag.
  - The clients expansion flags. _(What is paid for and what is available on disk.)_
    - _Pulled from: `PTR_g_papp->exist_expansion_id` and `PTR_g_papp->exist_expansion_disk`_
  - The clients actual version string.

An example of this would look like: `GENERIC.VERSION:EN/S00000fff/C00000fff/30220204_0`
