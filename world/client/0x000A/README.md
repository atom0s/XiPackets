# `GP_CLI_COMMAND_LOGIN`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_LOGIN` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x000A` |
| **Size**                  | `0x005C` |

## Description

This packet is sent by the client when requesting to log into a zone. This is the first packet sent by the client to the world server when beginning its communications.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_LOGIN
struct GP_CLI_LOGIN
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     LoginPacketCheck;   // PS2: LoginPacketCheck
    uint8_t     padding00;          // PS2: dam__
    uint16_t    unknown00;          // PS2: MyPort
    uint32_t    unknown01;          // PS2: MyIP
    uint32_t    UniqueNo;           // PS2: UniqueNo
    uint16_t    GrapIDTbl[9];       // PS2: GrapIDTbl
    char        sName[15];          // PS2: sName
    char        sAccunt[15];        // PS2: sAccunt
    uint8_t     Ticket[16];         // PS2: Ticket
    uint32_t    Ver;                // PS2: Ver
    uint8_t     sPlatform[4];       // PS2: sPlatform
    uint16_t    uCliLang;           // PS2: uCliLang
    uint16_t    dammyArea;          // PS2: dammyArea
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_LOGIN`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `LoginPacketCheck`

_The packet data checksum._

This value is a basic byte-sum value used as a checksum for the packet data to check for tampering. The client add the values of each byte in the packet _(starting at the `unknown01` field)_ and store the total in this value.

### `padding00`

_Padding; unused._

### `unknown00`

_Unknown._

This value is used as a state related value within the client. It will range between `0` and `4` depending on what current state the client is in.

  - When the client first initializes the zone _(via `gcZoneInit`)_ it will reset the value used for this to `0`.
  - When the client sends this login packet to the server, it will set this value to `1`. _(Value is set after it is already read and populated into the packet.)_
  - When the client sends this login packet to the server under other special conditions, it will be instead be set to `2`.
  - When the client receives packets from the server, this value will be set to `3`.
  - When the client receives the servers `0x000A` packet response, this value will be set to `4`.

### `unknown01`

_Unknown._

This value is used as an error state related value within the client. It contains two values as the client rotates between values _(last-two states)_ that will be stored. Its values are set during error conditions within the client related to packet traffic. The values that can be set here are between `0` and `32`.

The following is some examples of when the client will set different value types:

  - `0x00` - _Unused._
  - `0x01` - The client has an invalid zone IP address.
  - `0x02` - The client failed to receive the incoming packet buffer. _(`enAcvGet` call failed, returned 0.)_
  - `0x03` - The client failed to receive the incoming packet buffer. _(`enAcvGet` call failed, returned <= -1.)_
  - `0x04` - _Unused._
  - `0x05` - The client hit a specific packet loss percentage.
  - `0x06` - The client is in an unexpected state or the current expected sync count is invalid.
  - `0x07` - The client has received an incomplete packet. _(Not enough data to process.)_
  - `0x08` - The client has received a packet with an invalid server sync count.
  - `0x09` - _Unused._
  - `0x0A` - The client has received a packet that it does not have a handler for.
  - `0x0B` - The client has received a packet it has failed to handle. _(No valid handler, or invalid state.)_
  - `0x10` - The client has unsuccessfully received the servers `0x000A` packet response. _(Client in unexpected state.)_
  - `0x20` - The client has successfully received the servers `0x000A` packet response.

### `UniqueNo`

_The server id of the character the client is logging in with._

### `GrapIDTbl`

_Unused._

This value is no longer used.

### `sName`

_Unknown._

While the client will set this value into the packet, the value is always null (empty string).

### `sAccunt`

_Unknown._

While the client will set this value into the packet, the value is always null (empty string).

### `Ticket`

_The unique client ticket checksum value._

When the client is building this packet, it will generate a unique ticket value for the client based on several pieces of information. The client will generate an MD5 checksum using the clients account name string and its current blowfish key. The resulting MD5 hash value is stored into this array.

### `Ver`

_Unknown._

While the client does use and set this value, it is always set to 0.

### `sPlatform`

_The client platform tag._

This value is set to the clients current platform shorthand tag.

| Tag | Platform |
| --- | --- |
| `PS2` | _PlayStation 2_ |
| `WIN` | _Windows PC_ |

### `uCliLang`

_The client language id._

| Id | Language |
| --- | --- |
| `0` | _Japanese_ |
| `1` | _English_ |
| `2` | _French_ |
| `3` | _German_ |

### `dammyArea`

_Unknown._

This value is used as a kind of state related value.

The client has an internal system called `XiInfo` which holds state based or stepping based values throughout different operations within the client. While the client is executing certain functions, it will make use of this system to store information and generally a current 'step' in the process to execute the function. This system allows the client to read/write these values to later determine if it failed to properly execute a given function. The usage of this system is also backed by files on disk, which are the `info00.bin` and `info02.bin` files found in the `SYS` folder of the `FINAL FANTASY XI` directory.

_**Note:** While this parameter has a name similar to how SE names unused padding data, the value is actually used with this name._

The client will set the `dammyArea` value by doing the following checks:

```cpp
// Obtain the XiInfo read pointer for table index 0.. (info00.bin)
const auto ptr1 = XiInfoRead0();
if (ptr1)
{
    // XiInfoRead returns -2 if the file does not have proper access..
    if (ptr1 == -2)
        pkt->dammyArea = -2;
    else
        pkt->dammyArea = *ptr1; // Read the uint16_t value from the file pointer..
}
else
{
    pkt->dammyArea = -1;
}

// Obtain the XiInfo read pointer for table index 2.. (info02.bin)
const auto ptr2 = XiInfoRead2();
if (ptr2 && ptr2 != -2)
    pkt->dammyArea = *ptr2; // Read the uint16_t value from the file pointer..
```

The client makes use of index `0` within the `XiInfo` system in the following functions:

  - `ZoneSetup`
    - Writes the values `0x09` and `0x00` when the function starts, after some basic checks.
    - Writes the values `0x09` and `0x01` when certain zone based conditions are met.
    - Writes the values `0x0A` and `pGlobalZoneNow->MyroomMapNumber` when certain zone based conditions are met.
    - Writes the values `0x09` and `0x02` when certain zone based conditions are met, after opening the zone via `XiZone::Open`.
    - Writes the values `0x09` and `0x03` when certain zone based conditions are met.
    - Writes the values `0x09` and `0x04` when certain zone based conditions are met.
    - Writes the values `0x0A` and `pGlobalZoneNow->MyroomMapNumber` when certain zone based conditions are met.
    - Writes the values `0x09` and `0x05` when certain zone based conditions are met, after opening the zone via `XiZone::Open`.
    - Writes the values `0x0B` and `pGlobalZoneNow->SubMapNumber` when certain zone based conditions are met.
    - Writes the values `0x09` and `0x06` before registering packet handler callbacks.
    - Writes the values `0x09` and `0x07` after registering packet handler callbacks.
    - Writes the values `0x09` and `0x08` after calling several initialization functions.
    - Writes the values `0x09` and `0x09` after calibrating the local client position.
    - Writes the values `0x09` and `0x0A` after initializing the local client entity object.
    - Writes the values `0x09` and `0x0B` after adjusting the camera based on the login state.
    - Writes the values `0x09` and `0x0C` after initializing the zone specific DAT files and additional DAT info. _(Dialog strings, Monster Names, Item Information)_
    - Writes the values `0x09` and `0x0D` after initializing the zone specific DAT files and additional DAT info. _(Dialog strings, Monster Names, Item Information)_
    - Writes the values `0x09` and `0x0E` after initializing the zone weather system, time system, and other additional setup steps.
    - Writes the values `0x09` and `0x0F` when the function completes.
  - `NT_SYS::ProcBase`
    - Writes the values `0x02` and `g_pNtSys->m_localPhase` when the current `g_pNtSys->m_currentMode` is `6`.
      - _This state is used when the client is preparing the login handling._
    - Writes the values `0x07` and `g_pNtSys->m_localPhase` when the current `g_pNtSys->m_currentMode` is `7`.
      - _This state is used when the client is loading the received login data._
  - `RecvLogin` _(S2C `0x000A` packet handler.)_
    - Writes the values `0x03` and `pkt->LoginState` when the function starts.
    - Writes the values `0x04` and `pkt->LoginState` when the function completes successfully.
    - Writes the values `0x05` and `pkt->LoginState` when the function completes unsuccessfully.
  - `FsConfig::loadConfig`
    - Writes the values `0x08` and `0x00` when the function starts.
    - Writes the values `0x08` and `0x01` when the function completes.

The client makes use of index `2` within the `XiInfo` system in the following functions:

  - `MainIdle`
    - Writes the values `0x00` and `0x01` when the client has selected a character to login with, before additional handling. _(`USER` folder handling.)_
    - Writes the values `0x01` and `0x01` when the client has selected a character to login with, after additional handling. _(`USER` folder handling.)_
  - `XiFileManager::getFileStat`
    - Writes the file id _(broken into two bytes)_ when the call has failed.
  - `XiFileManager::readFileNow`
    - Writes the file id _(broken into two bytes)_ when the call has failed.
  - `XiFileManager::getFileSize`
    - Writes the file id _(broken into two bytes)_ when the call has failed.

## Additional Information

This is the first packet that the client will send to the world server that it will be connecting to when beginning to play. _(It will also send this packet each time the client has been handed off to another world server between zoning.)_ The client has two means of sending this packet, one is used when the client is setup in a debug mode, while the other is the normal usage of the client.

The first method is used when the client has a debug name set, which is set by default to a single space character. This is the typical method the client will use when sending this packet. The second method is when the debug name is not set. Both of the methods used to send this packet are fairly similar with limited differences, the overall effect is the same though in regards to just requesting to log into the server.

The main difference is how the client will make use of the value used to set the `pkt->unknown00` value. The main handler that is normally used sets this value to `1` when the call has completed, while the other will set it to `2` instead.
