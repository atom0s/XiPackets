# `GP_CLI_COMMAND_MYROOM_ENTER`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_MYROOM_ENTER` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00C9` |
| **Size**                  | `0x0008` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

> [!NOTE]
> This is a special GM-related packet!

This packet is used with the GM command `//myroom`.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: _GP_MYROOM_GATE_REQ
struct _GP_MYROOM_GATE_REQ
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    No;         // PS2: No
    uint16_t    unknown00;  // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`_GP_MYROOM_GATE_REQ`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `No`

_Unknown._

This value is assumed to be the server id of the characters mog house to enter.

### `unknown00`

_Unknown._

The purpose of this value is unknown.
