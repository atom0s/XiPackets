# `Unknown - Packet: 0x0044`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0044` |
| **Size**                  | `0x00A0` |

## Description

This packet is sent by the server to populate the clients extended job information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     JobId;
    uint8_t     IsSubJob;
    uint8_t     Data[154];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `JobId`

_The job id that this packet is intended for._

### `IsSubJob`

_Flag that states if the `JobId` value should be treated as the players main job or sub job._

### `Data`

_The additional job data._

## Additional Information

This packet is used to populate additional job-specific information in the client. The client has two arrays available that hold this additional information, one for their main jobs information and one for their subjob. The packet will use the `JobId` and `IsSubJob` values to tell the client how it should validate and use this packets data.

The client will use the `IsSubJob` value to determine which job id should be checked against the `JobId` value. If it does not properly match which job (or subjob) the client is currently on, then the packet is ignored.

The client will then copy the entire contents of this packet _(starting at `JobId`)_ into one of two buffers depending on if the `IsSubJob` flag is set.

_Further reversing of the `Data` field will take place at a later time. This block of data will vary per-job._
