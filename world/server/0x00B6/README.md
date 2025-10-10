# `GP_SERV_COMMAND_SET_GMMSG`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_SET_GMMSG` |
| **Client Handler**        | `FDtRecvGmNotice` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00B6` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the server in response to the client GM report/call. This packet contains a GMs response message.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_SET_GMMSG
struct GP_SERV_SET_GMMSG
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    msgId;  // PS2: msgId
    uint16_t    seqId;  // PS2: seqId
    uint16_t    pktNum; // PS2: pktNum
    uint8_t     Msg[];  // PS2: Msg
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_SET_GMMSG`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `msgId`

_The message id._

This value is the message id that this packet belongs to. Messages can be broken into several packets allowing for the message to fill an entire 1024 characters of data. Each message packet will contain up to a total of 244 printable characters, followed by 4 bytes of zeros for padding alignment. Instead of a traditional id for data _(such as a database record id, unique hash/checksum, or some other unique value)_ this value is actually the Unix timestamp of when the message was created.

### `seqId`

_The message sequence id._

This value is used as the current message packet in a sequence of packets to populate the full message. This value will start at `1` and increment for each packet that is needed to complete the full message string.

### `pktNum`

_The message packet number._

This value is used as the overall packet number. It will match the `seqId` until the last packet of the message. When the last packet needed to populate the message is received, it will have the `pktNum` value set to `0`, noting the final message packet the sequence.

### `Msg`

_The message buffer._

## Additional Information

When the client makes a GM report, it will send a request to the server to notify the GM staff that the client wishes to speak with a GM or receive support in some manner. When possible, a GM will directly contact the player either by `/tell` or physically appearing before them. When the clients report or request can be simply responded to with a single message, then the GM system will use this packet to send the client a special GM message.

This system allows for a message of up to 1024 characters long. However, packets cannot hold that many bytes of data thus the message must be broken into parts. Each response using this packet can contain a maximum of 244 characters within the `Msg` buffer. This means that in order to populate a full 1024 character long message, a total of 5 packets would be needed.

When the client first begins to receive this packet, it will check to see if the `msgId` matches its local last-seen `msgId` value. _(This will be `0` if the client has not received a previous GM message since zoning.)_ If the ids do not match, then the client will treat this packet as a brand new message and reset the current local buffer state that holds the message information. If the ids do match, then the client assumes that the packet is part of a larger message that requires multiple packets. When this happens, it will reuse its local `recvOffset` value to know where to begin writing the new packets `Msg` value into its local buffer.

Next, the client will check and see if any of the following checks fail. _(If they do, the handler will exit ignoring the remaining data. When this happens, the client will attempt to receive the message again at a later time.)_

  - `pkt->msgId != PTR_FDtGmNotice.noticeId` _(Ensures the incoming message id matches the current known id.)_
  - `pkt->seqId <= PTR_FDtGmNotice.recvSeq` _(Ensures the sequence count is valid and above the previously seen sequence id. When handling a new message, `recvSeq` will be set to `0`.)_
  - `pkt->seqId != PTR_FDtGmNotice.recvSeq + 1` _(Ensures the packets are received in the expected proper order to properly rebuild the message.)_

Once these checks validate, the client will then ensure the current `recvOffset` does not exceed 1024. If not, then it will copy the current `Msg` data into the local `PTR_FDtGmNotice.message` buffer, starting at the current `recvOffset`. Once this is copied, the client will increment the `recvOffset` value by the length of the message preparing it for the next chunk in the message, if there is one.

Last, the client will increment the `recvSeq` count. It will then check if `pkt->pktNum` is set to `0`. If it is, then `recvOffset` is reset to `0`, marking the message as completely received and ready for the client to read.

With the full message received, the client will then process the data, saving it locally to disk. This is done to prompt the client that they have a pending GM message in case they zone. The client saves the information into the following files:

  - `gmNotice.siz` - Contains the current pending message size. _(Raw uint16\_t size written to the file as binary data.)_
  - `gmNotice.fdt` - Contains the current pending message.

The file format of the `gmNotice.fdt` file is a generic dataset storage format. The following lines are expected in this file:

```
DEF DATASET <date>
DEF SELECTNODE 0
LABEL "NOTICE"
HEADER "<string>"
END NODE
END DATASET
```

The first line is build using the following:
  - Format String: `DEF DATASET %04u%02u%02u%02u\n`
  - Format Values: `2003, 1, 17, 1` _(These are hard coded in the client!)_

The `HEADER` string value is the message buffer.
