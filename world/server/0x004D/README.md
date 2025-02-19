# `GP_SERV_COMMAND_FRAGMENTS`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_FRAGMENTS` |
| **Client Handler**        | `RecvFragments` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x004D` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the server to respond to a client fragment request. The main usage of this packet is for sending the client fragments of the server message as the client requests them.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_FRAGMENTS
struct GP_SERV_FRAGMENTS
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Command;    // PS2: Command
    int8_t      Result;     // PS2: Result
    uint8_t     value1;     // PS2: fragmentsNo
    uint8_t     value2;     // PS2: fragmentsTotal
    int32_t     timestamp;  // PS2: signature
    int32_t     size_total; // PS2: timestamp
    int32_t     offset;     // PS2: offset
    int32_t     data_size;  // PS2: size
    uint8_t     data[];     // PS2: (unnamed)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_FRAGMENTS`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Command`

_The fragment command._

This value is used to tell the client how the fragments data should be handled. There are two handler sub-functions that are used based on the `Command` value. These are broken into the following:

| Id | Handler |
| --- | --- |
| `0x01` | `Handler: 1` |
| `0x02` | `Handler: 1` |
| `0x0A` | `Handler: 2` |
| `0x0B` | `Handler: 2` |
| `0x0C` | `Handler: 2` |
| `0x0D` | `Handler: 2` |

_The client will ignore all other `Command` values._

### `Result`

_The fragment result._

This value is used to tell the client the current result of the fragment command. However, unlike other packets that have similar `Command` and `Result` paired values, this one is used a bit differently. The `Result` value used with fragments can be a valid value even when negative. When a `Command` value of `0x0A`, `0x0B`, `0x0C`, or `0x0D` is being processed, the result value is checked against certain conditions to determine how the fragment should be processed.

If the fragment `value1` is set to `2` and the `Result` value is `if(pkt->Result >= -3 && (pkt->Result <= -2 || pkt->Result == 1))` then the fragment `data` is processed in a different manner. Another check is then performed afterward to test if the `Result` is `>= 0` and if additional conditions are met against other parts of the fragment information which will also cause the data to be handled differently.

### `value1`

_The fragment value. (1)_

The client uses this value to compare against an initializer value that is set on the requesting `YmFragmentsRequestTask` object. If the values do not match, then the fragment is ignored.

When the client creates the requesting `YmFragmentsRequestTask`, it will use the following values based on what created the task:

| Id | Request Cause |
| --- | --- |
| `0x00` | `Task Finish (Reset)` |
| `0x01` | `Server Message Request (/servmes)` |
| `0x02` | `Event VM, OpCode: 0xB3` |

  - When the task has been completed, the client will cleanup the task state and reset the value to `0x00`.
  - When the client is requesting the current server message, it will set this value to `0x01`.
  - When the client is requesting event related data, it will set this value to `0x02`. _(For example, the fishing rank NPC in Selbina, Chenon.)_

### `value2`

_The fragment value. (1)_

This value is used for multiple purposes.

When the client is requesting the current server message, the client will set the matching `YmFragmentsRequestTask` value this is compared against to the clients current language id.

| Id | Language |
| --- | --- |
| `0` | `Invalid` |
| `1` | `Japanese` |
| `2` | `English` |
| `3` | `French` |
| `4` | `German` |

When the client is requesting ranking board related data from the server, the client will set the matching `YmFragmentsRequestTask` value this is compared against to a value loaded from the given event script data. For example, when speaking to Chenon to request the Fishing ranking boards in Selbina, the event script that will cause the fragment request to be made is done with:

```
Event_Opcode_00B3 07 00 80
Event_Opcode_00B3 05 00 00 01 00 02 00 03 00 04 00 05 00 06 00 07 00
```

In this case the `B3 07 00 80` data is the opcode that will cause this task request to be created. The `0x8000` value here means to load the first value in the events reference data, which is `0x04`.

### `timestamp`

_The fragment timestamp._

This value holds the timestamp of the entire fragment content. For example, if the fragment is part of a server message response, then the timestamp is of the server message creation time. _(This is assumed to be the file time or database insert time of the system message.)_ If the content is broken into multiple fragments, all fragments will contain the same timestamp value.

### `size_total`

_The fragment total size._

This value holds the total size of the content that will be fragmented.

### `offset`

_The fragment data offset._

This value holds the current fragment data offset in which it should be copied into the overall data buffer being rebuilt on the client. The client rebuilds the total fragmented content using `memcpy`. This value is also used by the client to make the next fragment request, telling the server where to begin the next fragment chunk at.

### `data_size`

_The fragment data size._

### `data`

_The fragment data._

This value is used to determine to actual size of data held in the `data` field. _(See notes below for more information.)_

## Additional Information: Fragment Data & Data Size

The fragment `data_size` value is used to determine the length of data contained within the `data` field. The `data` field is a variable length block of data that will cause the overall size of this packet to change. It is important to note that `data_size` does not always guarantee or mean the actual true size of `data` and is dependent on certain packet conditions. This will differ based on the `Command` being sent on how other conditions are checked.

### Server Message Data

When this packet is used in response to a server message request, then the `data_size` value will be the actual true size of content stored within the `data` field. The packets `value1` value will also be set to `1` indicating that the content of the packet is considered a string and that the client should treat it as one block of data. The server message can still be fragmented across multiple packets, which will cause the client to rebuild the total string into a new buffer before processing it. It does this by rebuild the data making use of the `offset` value to determine where to copy the current fragments content into this new buffer.

If the server message exceeds the maximum size of `data` that a fragment can hold _(236 bytes)_ then the string will be fragmented into multiple parts. The client makes use of the `offset` value again when making requests back to the server for the next piece of the fragmented string. It is important to note that when this does happen, all fragments except the last one are **NOT** null-terminated. The `data` array will be completely full and have no actual null-termination until the last fragment. Because of this, it is important that the string being rebuilt on the client side is done so without making use of string APIs that expect a null-terminated string. The client does this rebuild using `memcpy` for this reason. The final message fragment will contain a proper null-terminated remainder of the string that is also byte aligned following the FFXI packet alignment rules.

### Ranking Data

When this packet is used in response to ranking information requests, then different parts of its data will change in functionality. First, the `Command` value is used to determine how the original request was made. This is done from the `0xB3` event VM opcode. It's sub-command value will determine the `Command` value used with the fragment:

  - `B3 00 ?? ??` - _Creates a fragment request with `Command` value of `0x01`._
  - `B3 03 ?? ??` - _Creates a fragment request with `Command` value of `0x0A`._
  - `B3 04 ?? ??` - _Creates a fragment request with `Command` value of `0x0B`._
  - `B3 06 ?? ??` - _Creates a fragment request with `Command` value of `0x0C`._
  - `B3 07 ?? ??` - _Creates a fragment request with `Command` value of `0x0D`._

When ranking data is being received, the response fragment packet will set `value1` to `2` which indicates the content within the `data` field is no longer a string and is instead holding ranking entries. This also causes the manner in which `data_size` and `data` are used depending on which packet in the group of fragments is being received.

For example, when first talking to a ranking NPC, the client will send a fragment request with a `Command` value of `0x0D`. The server will respond with a matching fragment response with `Command` set to `0x0D` as well. This initial fragment will also contain matching information from the original request.

  - `value1` will be set to `2` which indicates that the information within `data` contains ranking entries.
  - `value2` will be set to the requests original matching value. _(For example `0x04` when first talking to Chenon.)_
  - `size_total` will be set the to the total block size of the ranking data that the NPC can send.
  - `offset` will be set to `0`.
  - `size_data` will be set to `0`.

When this happens, the client will read the fragment `data` entry information in the following manner:

```cpp
// Check if the fragment is rank data..
if (pkt->value1 == 2)
{
    // Ensure the result is valid..
    if (pkt->Result >= -3 && (pkt->Result <= -2 || pkt->Result == 1) && task->buffer)
    {
        task->buffer->unknown18 = *reinterpret_cast<uint32_t*>(pkt->data);
        std::memcpy(task->buffer->unknown1C, reinterpret_cast<const uint8_t*>(&pkt->data[4]), 0x24);
    }
}
```

The first `uint32_t` value read from the `data` is the total entry count that can be expected within any additional fragment requests. The `memcpy` will then copy the initial ranking entry which is generally the local clients own entry into the rankings, if valid.

When the client makes a request within the NPCs menu for the full ranking list information, then it will make use of the `Command` values `0x01` and `0x02` which are used to signal a fragment begin and continue sequence. However, `value1` will still be set to `2` marking the `data` content as holding ranking entries.

The last important note about this ranking entry setup is that the first block in the packet will not always be set. This block is generally used for the local clients data and additional packets will not also include this data. For example, when requesting a ranking list that holds 22 entries, this will be broken into 5 separate fragment responses. The first response will contain the initial entry, however each response afterward will not. It will be an empty block. _(The initial `uint32_t` will still be set to the max entry count in all fragments.)_
