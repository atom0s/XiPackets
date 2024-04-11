# `GP_CLI_COMMAND_FRAGMENTS`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_FRAGMENTS` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x004B` |
| **Size**                  | `0x0018` |

## Description

This packet is sent by the client to request information from the server that is expected to span over multiple packets. This includes things such as the server message and certain event dialogs such as the Fishing ranking system.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_FRAGMENTS
struct GP_CLI_FRAGMENTS
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
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_FRAGMENTS`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Command`

_The fragment command._

### `Result`

_The fragment result._

### `value1`

_The fragment value. (1)_

### `value2`

_The fragment value. (2)_

### `timestamp`

_The fragment timestamp._

### `size_total`

_The fragment total size._

### `offset`

_The fragment data offset._

### `data_size`

_The fragment data size._

## Additional Information

This packet is used to request fragmented data from the server. For more information on how this packet and its response from the server works, please check the server response packet documentation here: [**Header**](/world/server/0x004D/README.md)
