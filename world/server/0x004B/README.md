# `GP_SERV_COMMAND_PBX_RESULT`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_PBX_RESULT` |
| **Client Handler**        | `RecvReqPostReplyCommon` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x004B` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the server when the client is interacting with the delivery box system.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_PBX_RESULT
struct GP_SERV_PBX_RESULT
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Command;        // PS2: Command
    int8_t      BoxNo;          // PS2: BoxNo
    int8_t      PostWorkNo;     // PS2: PostWorkNo
    int8_t      ItemWorkNo;     // PS2: ItemWorkNo
    int32_t     ItemStacks;     // PS2: ItemStacks
    int8_t      Result;         // PS2: Result
    int8_t      ResParam1;      // PS2: ResParam1
    int8_t      ResParam2;      // PS2: ResParam2
    int8_t      ResParam3;      // PS2: ResParam3
    uint32_t    Represent[1];   // PS2: Represent
};

// PS2: GC_PBOX_SEND
struct GC_PBOX_SEND
{
    uint8_t     To[16];         // PS2: To
    uint32_t    RequestID;      // PS2: RequestID
    uint32_t    RequestTime;    // PS2: RequestTime
    int32_t     ItemWorkNo;     // PS2: ItemWorkNo
};

// PS2: GC_PBOX_RECV
struct GC_PBOX_RECV
{
    uint8_t     From[16];       // PS2: From
    uint32_t    RequestID;      // PS2: RequestID
    uint32_t    RequestTime;    // PS2: RequestTime
    int32_t     OpponentPBoxNo; // PS2: OpponentPBoxNo
};

// PS2: (unknown)
struct GC_PBOX
{
    union
    {
        GC_PBOX_SEND Send;      // PS2: Send
        GC_PBOX_RECV Recv;      // PS2: Recv
    } pbox;
};

// PS2: GP_POST_BOX_STATE
struct GP_POST_BOX_STATE
{
    uint32_t    Stat;           // PS2: Stat
    GC_PBOX     box_state;      // PS2: box_state
    uint16_t    ItemNo;         // PS2: ItemNo
    uint16_t    padding00;      // PS2: (New; did not exist.)
    int32_t     Kind;           // PS2: Kind
    uint32_t    Stack;          // PS2: Stack
    uint8_t     Data[28];       // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_PBX_RESULT`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `Command`

_The packet command id._

This value represents the delivery box action that is being performed by this packet. The following command ids are handled by the client:

| Id | PBX Command Name | Purpose |
| --- | --- | --- |
| `0x00` | `COM_NONE`           | _None. (Invalid command id.)_ |
| `0x01` | `COM_WORK`           | _Work_ |
| `0x02` | `COM_SET`            | _Set_ |
| `0x03` | `COM_SEND`           | _Send_ |
| `0x04` | `COM_CANCEL`         | _Cancel_ |
| `0x05` | `COM_CHECK`          | _Check_ |
| `0x06` | `COM_RECV`           | _Receive_ |
| `0x07` | `COM_CONFIRM`        | _Confirm_ |
| `0x08` | `COM_ACCEPT`         | _Accept_ |
| `0x09` | `COM_REJECT`         | _Reject_ |
| `0x0A` | `COM_GET`            | _Get_ |
| `0x0B` | `COM_CLEAR`          | _Clear_ |
| `0x0C` | `COM_QUERY`          | _Query_ |
| `0x0D` | `COM_DELI_OPEN`      | _Request Enter Delivery Mode_ |
| `0x0E` | `COM_POST_OPEN`      | _Request Enter Post Mode_ |
| `0x0F` | `COM_POSTAL_CLOSE`   | _Request Exit Delivery/Post Mode_ |

### `BoxNo`

_The box number._

This value represents which postal system container is being used with the packet.

| BoxNo | Client Container Used |
| --- | --- |
| `0` | _None. (Invalid)_ |
| `1` | _zone->PostSys.PostBoxCli\_inc_ |
| `2` | _zone->PostSys.PostBoxCli\_out_ |

### `PostWorkNo`

_The post work number._

This value represents the index into the box container being used. It can have a value of 0 to 8 to represent the 8 available box slots.

### `ItemWorkNo`

_The item work number._

> [!NOTE]
> The client does not appear to use this value.

### `ItemStacks`

_The item stacks value._

> [!NOTE]
> The client does not appear to use this value.

### `Result`

_The command result._

This value is used to inform the client if the previous action that caused this packet was successful or not. It is also used to tell the client different status values of the delivery system. Negative values indicate an error has occurred and will generally have an additional message printed to the chat log. _(Note: This is a signed value, the table will represent the negative values in their hex format!)_

| Value | State | Message |
| --- | --- | --- |
| `0xFF` | `ON_REQUEST`                 | _No message printed._ |
| `0xFE` | `BOX_FULL`                   | _No message printed._ |
| `0xFD` | `BOX_EMPTY`                  | _No message printed._ |
| `0xFC` | `NOBOX`                      | _No message printed._ |
| `0xFB` | `NOADDRESS`                  | _No message printed._ |
| `0xFA` | `LIMITED`                    | `Please try again in a little while.` |
| `0xF9` | `(unknown)`                  | `(The delivery service is currently unavailable.)` |
| `0xF0` | `PBOX_IN_USE`                | _No message printed._ |
| `0xEF` | `PBOX_NOT_FOR_SEND`          | _No message printed._ |
| `0xEE` | `PBOX_WAS_NOT_SEND`          | _No message printed._ |
| `0xED` | `PBOX_NOT_FOR_ACPT`          | _No message printed._ |
| `0xEC` | `PBOX_NOT_FOR_RJCT`          | _No message printed._ |
| `0xEB` | `PBOX_NOT_FOR_GET`           | _No message printed._ |
| `0xEA` | `POST_EMPTY`                 | _No message printed._ |
| `0xE9` | `RCPT_EMPTY`                 | _No message printed._ |
| `0xE0` | `CANCEL_NOTFOUND`            | _No message printed._ |
| `0xDF` | `CANCEL_NOT_MATCH_NAMES`     | _No message printed._ |
| `0xDE` | `CANCEL_NOT_MATCH_ID`        | _No message printed._ |
| `0xDD` | `CANCEL_NOT_MATCH_TIME`      | _No message printed._ |
| `0xDC` | `CANCEL_NOT_MATCH_PWORK_NO`  | _No message printed._ |
| `0xDB` | `CANCEL_NOT_MATCH_CONTENT`   | _No message printed._ |
| `0xD8` | `CONFIRM_ILLIGAL_PWORK_NO`   | _No message printed._ |
| `0xD7` | `CONFIRM_ILLIGAL_PWORK_STATE`| _No message printed._ |
| `0xD6` | `CONFIRM_NOT_MATCH_NAMES`    | _No message printed._ |
| `0xD5` | `CONFIRM_NOT_MATCH_ID`       | _No message printed._ |
| `0xD4` | `CONFIRM_NOT_MATCH_TIME`     | _No message printed._ |
| `0xD3` | `CONFIRM_NOT_MATCH_CONTENT`  | _No message printed._ |
| `0xD2` | `CONFIRM_UNEXPECTED_SUBJECT` | _No message printed._ |
| `0xC0` | `ERROR_IN_ITEMSYS`           | _No message printed._ |
| `0xBF` | `NO_ITEM_IN_WORK`            | _No message printed._ |
| `0xBE` | `ITEMWORK_IN_USE`            | _No message printed._ |
| `0xBD` | `ITEMWORK_LOCKED`            | _No message printed._ |
| `0xBC` | `LESS_ITEM_STACKS`           | _No message printed._ |
| `0xBB` | `NOT_POSTABLE_ITEM`          | _No message printed._ |
| `0xBA` | `YOU_ALREADY_HAVE_LORE`      | _No message printed._ |
| `0xB9` | `PROB_INVENTRY_FULL`         | _No message printed._ |
| `---`  | `---`                        | `---` |
| `0x00` | `FAIL`                       | _No message printed._ |
| `0x01` | `SUCCESS`                    | _No message printed._ |
| `0x02` | `INTERIM`                    | _No message printed._ |

### `ResParam1`

_The result parameter. (1)_

This value and its usage will depend on the packet `Command`.

### `ResParam2`

_The result parameter. (2)_

This value and its usage will depend on the packet `Command`.

### `ResParam3`

_The result parameter. (3)_

This value and its usage will depend on the packet `Command`.

### `Represent`

_The data specific to the current packets command id._

This value is only used during certain `Command` and `Result` values are set. When used, this value will contain a sub-structure (`GP_POST_BOX_STATE`) which holds additional information about an item.

## Structure Fields (`GP_POST_BOX_STATE`)

### `Stat`

_The item status._

| Stat | Meaning |
| --- | --- |
| `0x00` | `NONE` |
| `0x01` | `SET` |
| `0x02` | `SEND_GOING` |
| `0x03` | `SEND_DONE` |
| `0x04` | `CANCEL_GOING` |
| `0x05` | `CANCEL_DONE` |
| `0x06` | `RECV_INC` |
| `0x07` | `RECV_DONE` |
| `0x08` | `ACCEPTED_BY` |
| `0x09` | `REJECTED_BY` |
| `0x0A` | `ACCEPTING` |
| `0x0B` | `ACCEPT` |
| `0x0C` | `REJECTING` |
| `0x0D` | `REJECT` |
| `0x0E` | `GET` |

### `box_state`

_The item box state structure._

This value will vary based on the kind of packet being sent. _(This is a union type value.)_

### `ItemNo`

_The item number._

### `padding00`

_Padding; unused._

### `Kind`

_The item kind._

### `Stack`

_The item stack / count._

### `Data`

_The item data._

This value holds the items additional / extended data used for things such as augments, charges, signatures, timing values, etc.

## Structure Fields (`GC_PBOX`)

_This structure is a union type that is determined based on the kind of command being handled._

### `Send`

_The item box information. (To)_

### `Recv`

_The item box information. (From)_

## Structure Fields (`GC_PBOX_SEND`)

### `To`

_The name of the player the item is being sent to._

### `RequestID`

_The item request id._

### `RequestTime`

_The item request timestamp._

### `ItemWorkNo`

_The item work number._

## Structure Fields (`GC_PBOX_RECV`)

### `From`

_The name of the player the item was sent from._

### `RequestID`

_The item request id._

### `RequestTime`

_The item request time._

### `OpponentPBoxNo`

_The item pbox number._

## Structure Fields (`GP_SERV_PBX_RESULT`)

## Additional Information: `Represent`

This packet does not always make use of the `Represent` value and will cause the size of the packet to differ depending on when its present or not. When it is not present, then `Represent` will be a single `uint32_t` value, causing the packet to be `0x14` bytes in size. When `Represent` is being used, it will be replaced with a `GP_POST_BOX_STATE` structure instance, causing the packet size to be increased to `0x58` bytes in size instead. Only certain `Command` and `Result` values will cause this field to be the full `GP_POST_BOX_STATE` value instead of its base `uint32_t`. When `Represent` is only its `uint32_t` value, it is not used and its value is considered junk. The client does not use it and the data it holds is likely left-over buffer junk from a previous packet.

The following `Command` and `Result` combinations will cause the `Represent` value to be used. _(**Please note**; although the below table shows when the `Represent` data is actually used by the client, it does not indicate when the `Represent` value is set to the full structure. The server will still send the full structure value for certain commands even if the client will not make use of or access the data.)_

| `Command` | `Result` |
| :---: | --- |
| `0x01` | `0x01` |
| `0x02` | `0x01` |
| `0x03` | `0x01` |
| `0x04` | `0x01` |
| `0x06` | `0x01` |
| `0x07` | `0x01` |
| `0x08` | `0x01` |
| `0x08` | `0x02` |
| `0x09` | `0x01` |
| `0x09` | `0x02` |
| `0x0A` | `0x01` |
| `0x0B` | `0x01` |

_**Note:** There are also special logging `Command` cases where certain commands will use the `Represent` value always. This includes the following commands:_

  - `0x09` - _Used when `g_pTkPost` is a valid pointer._
  - `0x0A` - _Used when `g_pTkPost` is a valid pointer. Used when `g_pTkDelivery` is a valid pointer._
  - `0x0B` - _Used when `g_pTkPost` is a valid pointer._
