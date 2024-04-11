# `GP_CLI_COMMAND_PBX`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_PBX` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x004D` |
| **Size**                  | `0x0020` |

## Description

This packet is sent by the client when interacting with the delivery box system.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_PBX
struct GP_CLI_PBX
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
    uint8_t     TargetName[16]; // PS2: TargetName
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_PBX`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

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

### `ItemStacks`

_The item stacks value._

### `Result`

_The command result._

### `ResParam1`

_The result parameter. (1)_

### `ResParam2`

_The result parameter. (2)_

### `ResParam3`

_The result parameter. (3)_

### `TargetName`

_The target name._

## Additional Information

This packet is used by the client to handle the various operations when interacting with the delivery box system. The client uses this packet to request different actions be completed by the server for both the send and receive parts of the system. Different fields in this packet are populated depending on the `Command` being requested.

The client has a helper function that prepares the delivery packet for use by initializing the packets values to be in a proper default state. This function will always set the following fields to an actual value:

  - `Command`
  - `BoxNo`
  - `PostWorkNo`
  - `ItemWorkNo`

And will also default all the rest to the following:

  - `ItemStacks` - _Set to `-1`._
  - `Result` - _Set to `0`._
  - `ResParam1` - _Set to `0`._
  - `ResParam2` - _Set to `0`._
  - `ResParam3` - _Set to `0`._
  - `TargetName` - _Full string is set to null._

Afterward, the calling command function will then overwrite and populate any of the fields with additional information as needed for the given command to function properly.

## Additional Information - `Command` - `0x01` (WORK)

| Field | Value |
| --- | --- |
| `Command`     | _Set to `1`._ |
| `BoxNo`       | _Set to either `1` or `2` based on the box being interacted with._ |
| `PostWorkNo`  | _Set to the slot within the box being interacted with._ |
| `ItemWorkNo`  | _Set to `-1`._ |
| `ItemStacks`  | _Uses the default value._ |
| `Result`      | _Uses the default value._ |
| `ResParam1`   | _Uses the default value._ |
| `ResParam2`   | _Uses the default value._ |
| `ResParam3`   | _Uses the default value._ |
| `TargetName`  | _Uses the default value._ |

## Additional Information - `Command` - `0x02` (SET)

| Field | Value |
| --- | --- |
| `Command`     | _Set to `2`._ |
| `BoxNo`       | _Set to `2`._ |
| `PostWorkNo`  | _Set to the slot within the box being interacted with._ |
| `ItemWorkNo`  | _Set to the players inventory index where the item is located._ |
| `ItemStacks`  | _Set to the count of items being added._ |
| `Result`      | _Uses the default value._ |
| `ResParam1`   | _Uses the default value._ |
| `ResParam2`   | _Uses the default value._ |
| `ResParam3`   | _Uses the default value._ |
| `TargetName`  | _Set to the player name the item(s) are being sent to._ |

_**Note:** The client will enforce setting the first letter of the `TargetName` to uppercase!_

## Additional Information - `Command` - `0x03` (SEND)

| Field | Value |
| --- | --- |
| `Command`     | _Set to `3`._ |
| `BoxNo`       | _Set to `2`._ |
| `PostWorkNo`  | _Set to the slot within the box being interacted with._ |
| `ItemWorkNo`  | _Set to `-1`._ |
| `ItemStacks`  | _Uses the default value._ |
| `Result`      | _Uses the default value._ |
| `ResParam1`   | _Uses the default value._ |
| `ResParam2`   | _Uses the default value._ |
| `ResParam3`   | _Uses the default value._ |
| `TargetName`  | _Uses the default value._ |

## Additional Information - `Command` - `0x04` (CANCEL)

| Field | Value |
| --- | --- |
| `Command`     | _Set to `4`._ |
| `BoxNo`       | _Set to `2`._ |
| `PostWorkNo`  | _Set to the slot within the box being interacted with._ |
| `ItemWorkNo`  | _Set to `-1`._ |
| `ItemStacks`  | _Uses the default value._ |
| `Result`      | _Uses the default value._ |
| `ResParam1`   | _Uses the default value._ |
| `ResParam2`   | _Uses the default value._ |
| `ResParam3`   | _Uses the default value._ |
| `TargetName`  | _Uses the default value._ |

## Additional Information - `Command` - `0x05` (CHECK)

| Field | Value |
| --- | --- |
| `Command`     | _Set to `5`._ |
| `BoxNo`       | _Set to either `1` or `2` based on the box being interacted with._ |
| `PostWorkNo`  | _Set to `-1`._ |
| `ItemWorkNo`  | _Set to `-1`._ |
| `ItemStacks`  | _Uses the default value._ |
| `Result`      | _Uses the default value._ |
| `ResParam1`   | _Uses the default value._ |
| `ResParam2`   | _Uses the default value._ |
| `ResParam3`   | _Uses the default value._ |
| `TargetName`  | _Uses the default value._ |

## Additional Information - `Command` - `0x06` (RECV)

| Field | Value |
| --- | --- |
| `Command`     | _Set to `6`._ |
| `BoxNo`       | _Set to `1`._ |
| `PostWorkNo`  | _Set to the slot within the box being interacted with._ |
| `ItemWorkNo`  | _Set to `1`._ |
| `ItemStacks`  | _Uses the default value._ |
| `Result`      | _Uses the default value._ |
| `ResParam1`   | _Uses the default value._ |
| `ResParam2`   | _Uses the default value._ |
| `ResParam3`   | _Uses the default value._ |
| `TargetName`  | _Uses the default value._ |

## Additional Information - `Command` - `0x07` (CONFIRM)

| Field | Value |
| --- | --- |
| `Command`     | _Set to `7`._ |
| `BoxNo`       | _Set to `-1`._ |
| `PostWorkNo`  | _Set to `-1`._ |
| `ItemWorkNo`  | _Set to `-1`._ |
| `ItemStacks`  | _Uses the default value._ |
| `Result`      | _Uses the default value._ |
| `ResParam1`   | _Uses the default value._ |
| `ResParam2`   | _Uses the default value._ |
| `ResParam3`   | _Uses the default value._ |
| `TargetName`  | _Uses the default value._ |

## Additional Information - `Command` - `0x08` (ACCEPT)

| Field | Value |
| --- | --- |
| `Command`     | _Set to `8`._ |
| `BoxNo`       | _Set to `1`._ |
| `PostWorkNo`  | _Set to the slot within the box being interacted with._ |
| `ItemWorkNo`  | _Set to `-1`._ |
| `ItemStacks`  | _Uses the default value._ |
| `Result`      | _Uses the default value._ |
| `ResParam1`   | _Uses the default value._ |
| `ResParam2`   | _Uses the default value._ |
| `ResParam3`   | _Uses the default value._ |
| `TargetName`  | _Uses the default value._ |

## Additional Information - `Command` - `0x09` (REJECT)

| Field | Value |
| --- | --- |
| `Command`     | _Set to `9`._ |
| `BoxNo`       | _Set to `1`._ |
| `PostWorkNo`  | _Set to the slot within the box being interacted with._ |
| `ItemWorkNo`  | _Set to `-1`._ |
| `ItemStacks`  | _Uses the default value._ |
| `Result`      | _Uses the default value._ |
| `ResParam1`   | _Uses the default value._ |
| `ResParam2`   | _Uses the default value._ |
| `ResParam3`   | _Uses the default value._ |
| `TargetName`  | _Uses the default value._ |

## Additional Information - `Command` - `0x0A` (GET)

| Field | Value |
| --- | --- |
| `Command`     | _Set to `10`._ |
| `BoxNo`       | _Set to either `1` or `2` based on the box being interacted with._ |
| `PostWorkNo`  | _Set to the slot within the box being interacted with._ |
| `ItemWorkNo`  | _Set to `-1`._ |
| `ItemStacks`  | _Uses the default value._ |
| `Result`      | _Uses the default value._ |
| `ResParam1`   | _Uses the default value._ |
| `ResParam2`   | _Uses the default value._ |
| `ResParam3`   | _Uses the default value._ |
| `TargetName`  | _Uses the default value._ |

## Additional Information - `Command` - `0x0B` (CLEAR)

| Field | Value |
| --- | --- |
| `Command`     | _Set to `11`._ |
| `BoxNo`       | _Set to either `1` or `2` based on the box being interacted with._ |
| `PostWorkNo`  | _Set to the slot within the box being interacted with._ |
| `ItemWorkNo`  | _Set to `-1`._ |
| `ItemStacks`  | _Uses the default value._ |
| `Result`      | _Uses the default value._ |
| `ResParam1`   | _Uses the default value._ |
| `ResParam2`   | _Uses the default value._ |
| `ResParam3`   | _Uses the default value._ |
| `TargetName`  | _Uses the default value._ |

## Additional Information - `Command` - `0x0C` (QUERY)

| Field | Value |
| --- | --- |
| `Command`     | _Set to `12`._ |
| `BoxNo`       | _Set to `-1`._ |
| `PostWorkNo`  | _Set to `-1`._ |
| `ItemWorkNo`  | _Set to `-1`._ |
| `ItemStacks`  | _Uses the default value._ |
| `Result`      | _Uses the default value._ |
| `ResParam1`   | _Uses the default value._ |
| `ResParam2`   | _Uses the default value._ |
| `ResParam3`   | _Uses the default value._ |
| `TargetName`  | _Set to the player name the client wishes to send items to._ |

_**Note:** The `TargetName` in this packet is used to 'query' if the player name is valid on the server side._

## Additional Information - `Command` - `0x0D` (DELI_OPEN)

| Field | Value |
| --- | --- |
| `Command`     | _Set to `13`._ |
| `BoxNo`       | _Set to `-1`._ |
| `PostWorkNo`  | _Set to `-1`._ |
| `ItemWorkNo`  | _Set to `-1`._ |
| `ItemStacks`  | _Uses the default value._ |
| `Result`      | _Uses the default value._ |
| `ResParam1`   | _Uses the default value._ |
| `ResParam2`   | _Uses the default value._ |
| `ResParam3`   | _Uses the default value._ |
| `TargetName`  | _Uses the default value._ |

## Additional Information - `Command` - `0x0E` (POST_OPEN)

| Field | Value |
| --- | --- |
| `Command`     | _Set to `14`._ |
| `BoxNo`       | _Set to `-1`._ |
| `PostWorkNo`  | _Set to `-1`._ |
| `ItemWorkNo`  | _Set to `-1`._ |
| `ItemStacks`  | _Uses the default value._ |
| `Result`      | _Uses the default value._ |
| `ResParam1`   | _Uses the default value._ |
| `ResParam2`   | _Uses the default value._ |
| `ResParam3`   | _Uses the default value._ |
| `TargetName`  | _Uses the default value._ |

## Additional Information - `Command` - `0x0F` (POSTAL_CLOSE)

| Field | Value |
| --- | --- |
| `Command`     | _Set to `15`._ |
| `BoxNo`       | _Set to `-1`._ |
| `PostWorkNo`  | _Set to `-1`._ |
| `ItemWorkNo`  | _Set to `-1`._ |
| `ItemStacks`  | _Uses the default value._ |
| `Result`      | _Uses the default value._ |
| `ResParam1`   | _Uses the default value._ |
| `ResParam2`   | _Uses the default value._ |
| `ResParam3`   | _Uses the default value._ |
| `TargetName`  | _Uses the default value._ |
