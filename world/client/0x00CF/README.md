# `GP_CLI_COMMAND_MYROOM_HARVEST`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_MYROOM_HARVEST` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00CF` |
| **Size**                  | `0x000A` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

The purpose of this packet is unknown.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    unknown00;
    uint16_t    unknown01;
    uint8_t     unknown02;
    uint8_t     unknown03;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `unknown00`

_Unknown._

### `unknown01`

_Unused._

The client does not use this value.

### `unknown02`

_Unknown._

### `unknown03`

_Unused._

The client does not use this value.

## Additional Information

This packet is deprecated and no longer used by the client. While it is connected to the client command `MYROOM_HARVEST`, there is no usage of the function that generates the packet. The original PS2 beta does not make use of this packet id at all either, thus the function was added sometime after the PS2 beta period.
