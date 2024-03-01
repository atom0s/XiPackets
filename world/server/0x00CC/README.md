# `GP_SERV_COMMAND_LINKSHELL_MESSAGE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_LINKSHELL_MESSAGE` |
| **Client Handler**        | `RecvComlinkMessage` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00CC` |
| **Size**                  | `0x00B0` |

## Description

This packet is sent by the server in response to the clients linkshell command request usage.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_LINKSHELL_MESSAGE
struct GP_SERV_LINKSHELL_MESSAGE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     stat            : 4;    // PS2: stat
    uint8_t     attr            : 4;    // PS2: attr
    uint8_t     readLevel       : 2;    // PS2: readLevel
    uint8_t     writeLevel      : 2;    // PS2: writeLevel
    uint8_t     pubEditLevel    : 2;    // PS2: pubEditLevel
    uint8_t     linkshell_index : 2;    // PS2: dummyBits
    uint16_t    seqId;                  // PS2: seqId
    uint8_t     sMessage[128];          // PS2: sMessage
    uint32_t    updateTime;             // PS2: updateTime
    uint8_t     modifier[16];           // PS2: modifier
    uint16_t    opType;                 // PS2: opType
    uint16_t    padding00;              // PS2: padding
    uint8_t     encodedLsName[16];      // PS2: encodedLsName
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_LINKSHELL_MESSAGE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `stat`

_The packet status._

### `attr`

_The packet attributes._

### `readLevel`

_The authorization level required to read the linkshell message._

### `writeLevel`

_The authorization level required to edit the linkshell message._

### `pubEditLevel`

_The authorization level required to edit the linkshell message._

### `linkshell_index`

_The linkshell index._

This value is only used during certain `opType` modes.

### `seqId`

_The packet sequence id._

### `sMessage`

_The linkshell message._

### `updateTime`

_The linkshell message timestamp._

### `modifier`

_The name of the player who last edited the linkshel message._

### `opType`

_The packet mode._

### `padding00`

_Padding; unused._

### `encodedLsName`

_The encoded linkshell name._

## Additional Information

This packet is used to respond to the various usages of the `/lsmes` and `/linkshell2mes` commands. The client can use these commands to do various actions with the linkshell message including:

  - Request the current linkshell message. _(ie. `/lsmes`)_
  - Set the current linkshell message. _(ie. `/lsmes Hello world!` or `/lsmes set Hello world!`)_
  - Clear the current linkshell message. _(ie. `/lsmes clear`)_
  - Set the linkshell authorization level required to edit the linkshell message. _(ie. `/lsmes level all`)_

The server will make use of different parts of this packet depending on the response type being sent to the client. The main values of interest and use are the `stat`, `attr` and `opType` values. Below you can find more information regarding the various supported `opType` values and how the client handles them. If the `opType` value does not equal one of the listed below values, then the client will simply exit the handler; ignoring the packet.

### `opType` Mode: `1` - General Error Response

This mode is used when the server is sending the client an error response regarding their previous linkshell message request. When this mode is used, the client makes use of the `stat` value to determine the kind of error that has occurred and how the rest of the information should be processed.

If the client has requested to do something they are not authorized for, then the server will respond with this mode and set `stat` to 3. This will cause the client to print the `You are not authorized to perform that action.` error message. Other errors will set this value to anything non-zero and not 5. This will cause the client to print the `An error occurred.` message.

If the `stat` value is 5 in this mode, it will attempt to print the linkshell message within the packet. _(The purpose of this mode is unknown as actual successful linkshell message requests and prints use `opType` mode 3 instead.)_

The following pseudo code block is how the client will handle this mode:

```cpp
case 1:
{
    if (pkt->stat != 0)
    {
        if (pkt->stat == 3)
        {
            ComlinkCallback1();
            return 1;
        }

        if (pkt->stat != 5)
        {
            ComlinkCallback2(7, 0, 0, 0, 0);
            return 1;
        }
    }
    else if ((pkt->attr & 0x04) == 0)
    {
        return 1;
    }

    FromCompressStr(buffer, 20, pkt->encodedLsName);
    ComlinkCallback2(pkt->linkshell_index + 1, pkt->updateTime, buffer, pkt->modifier, pkt->sMessage);
    return 1;
}
```

### `opType` Mode: `2` - Request Linkshell Message Response

This mode is used when the server is sending the client the current linkshell message. When this mode is used, the client makes use of the `stat` value to determine how the rest of the information should be processed.

  - When the client requests the current linkshell message, and one is currently set, the server will respond with this mode and set `stat` to 0. `attr` will also be set to 4.
  - When the client requests the current linkshell message, and one is not currently set, the server will respond with this mode and set `stat` to 5 and `attr` to 7. _(This is also true when the client first equips a linkshell item and automatically requests the linkshell message.)_

The client will make use of the `linkshell_index` value to determine how to prefix and color the message. _(The client adds 1 to the below values before using it!)_

  - `0` - _The message is printed for linkshell 1._
  - `1` - _The message is printed for linkshell 2._
  - `2` - _The message is printed for linkshell 3._
  - `6` - _Invalid index; used for error handling._

The following pseudo code block is how the client will handle this mode:

```cpp
case 2:
{
    if (pkt->stat != 0)
    {
        if (pkt->stat == 5)
        {
            FromCompressStr(buffer, 20, pkt->encodedLsName);
            if (pkt->linkshell_index + 1 == 7)
                ComlinkCallback2(7, 0, 0, 0, 0);
            else
                ComlinkCallback2(pkt->linkshell_index + 1, pkt->updateTime, buffer, pkt->modifier, 0);
        }
        else
        {
            ComlinkCallback2(7, 0, 0, 0, 0);
        }
    }
    else
    {
        if ((pkt->attr & 0x04) == 0)
            return 1;

        FromCompressStr(buffer, 20, pkt->encodedLsName);
        ComlinkCallback2(pkt->linkshell_index + 1, pkt->updateTime, buffer, pkt->modifier, ptr->sMessage);
        return 1;
    }

    return 1;
}
```

### `opType` Mode: `3` - Set Authorization Level Response

This mode is used when the server is responding to the clients authorization level setting request. When this mode is used, the client makes use of the `stat` value to determine how the rest of the information should be processed. The auth level fields are also used to inform the sub-handler how it should print the status message, if any.

When the linkshell holder successfully changes the auth level, the server will respond with `stat` 0 and `attr` 3. The `linkshell_index` value will be set to the index of the linkshell that was modified. _(See above notes in Mode 2.)_ The server will also set the `readLevel` and `writeLevel` values in this mode informing the client which mode has been set. _(**Note:** `readLevel` will always be set to 2. This is mainly to update the `writeLevel` value.)_

The following values are used for the `readLevel` and `writeLevel` fields:

  - `0` - `ls` - _Only linkshell holder can edit the linkshell message._
  - `1` - `ps` - _Only linkshell holder and sack holders can edit the linkshell message._
  - `2` - `all` - _All members can edit the linkshell message._

If a client attempts to set the linkshell message auth level and does not have access, the server will respond with `stat` 3 and `attr` 0.

The following pseudo code block is how the client will handle this mode:

```cpp
case 3:
{
    if (pkt->stat != 0)
    {
        if (pkt->stat == 3)
        {
            ComlinkCallback1();
            return 1;
        }

        ComlinkCallback3(7, -1, -1);
        return 1;
    }

    ComlinkCallback3(pkt->linkshell_index + 1, pkt->readLevel, pkt->writeLevel);
    return 1;
}
```

### `opType` Mode: `6` - Request Authorization Level Response

This mode is used when the server is responding to the client requesting the current authorization level. If an error occurs during this request, the `stat` value will be non-zero. Otherwise, the server will respond with `stat` 0 and `attr` 3.

On successful requests, the server will set the `readLevel` and `writeLevel` to the appropriate authorization level numbers. _(See notes above.)_

The following pseudo code block is how the client will handle this mode:

```cpp
case 6:
{
    if (pkt->stat != 0)
    {
        ComlinkCallback3(7, -1, -1);
        return 1;
    }

    ComlinkCallback3(pkt->linkshell_index + 1, pkt->readLevel, pkt->writeLevel);
    return 1;
}
```

## Additional Inforamtion - Populated Packet Data

When the server responds with this packet, its content will not always be fully populated. Instead, the server will only make use of the fields required to allow the client to handle the response correctly. The only fields that are always populated in this packet are `stat`, `attr` and `encodedLsName`. The rest of the fields are optional and depend on the mode and current status of the packet.

The `readLevel` and `writeLevel` values are only populated when the client has requested them or the server is responding to an auth level related request. Generally, only the `readLevel` value will be set in most cases unless specifically requesting the full auth level of the linkshell. The `pubEditLevel` is not used by the client and can be ignored entirely.

The `linkshell_index` value is used when needed to properly display the linkshells related message or error message. _(See the pseudo code handlers above to see when its used.)_

The `seqId` value is used when handling the `/lsmes level` command. Any time the client requests current linkshell access level, it will increment its own internal sequence counter. This counter value is then sent with the request the client sends to the server asking for the current auth level. The server will respond to the client with the same `seqId` it sent. When the client sends an attempt to change the auth level, such as `/lsmes level all`, it will send the last known `seqId` it had incremented previously. _(Only getting the current auth level increases this counter on the client.)_

The `sMessage` value is only set when requesting the current linkshell message, and one is currently set.

The `updateTime` value is only set when requesting the current linkshell message. This value is set both when the message was successfully obtained, or if the previous message was cleared. Otherwise, this value will be 0.

The `modifier` value is only set when requesting the current linkshell message, and one is currently set. This will not be set if the linkshell message was previously cleared.

The `encodedLsName` appears to be set in every packet, regardless if it will be used.
