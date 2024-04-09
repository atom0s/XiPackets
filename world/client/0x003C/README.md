# `GP_CLI_COMMAND_BLACK_LIST`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_BLACK_LIST` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x003C` |
| **Size**                  | `0x001C` |

## Description

This packet is sent by the client when it needs to request the clients blacklist.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (Unknown packet name.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    unknown00;      // PS2: (Unknown field name.)
    uint32_t    unknown01;      // PS2: (Unknown field name.)
    uint32_t    unknown02;      // PS2: (Unknown field name.)
    uint32_t    unknown03;      // PS2: (Unknown field name.)
    uint32_t    unknown04;      // PS2: (Unknown field name.)
    uint8_t     unknown05;      // PS2: (Unknown field name.)
    uint8_t     padding00[3];   // PS2: (Unknown field name.)
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

_Unknown._

### `unknown02`

_Unknown._

### `unknown03`

_Unknown._

### `unknown04`

_Unknown._

### `unknown05`

_Unknown._

### `padding00`

_Padding; unused._

## Additional Information

This packet is sent by the client when the internal blacklist system has not been initialized yet. Generally, this should not happen as the blacklist information is populated upon zoning. The client sets all fields in this packet to `0`.

_This packet was present on the PS2 beta, but it did not have an actual packet type or naming to any of its data/fields. The PS2 beta simply set the entire block of data in this packet to 0 using a single memset instead of setting individual fields._
