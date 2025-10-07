# `GP_CLI_COMMAND_MYROOM_EXIT`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_MYROOM_EXIT` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00CA` |
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
    uint16_t    unknown06;  // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`_GP_MYROOM_GATE_REQ`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `No`

_Unused._

The client does not use this value with this packet.

### `unknown06`

_Unused._

The client does not use this value with this packet.

## Additional Information

This packet shares the same structure as the `0x00C9` packet. However, the client does not use the packets fields with this version of the packet. The client sends this packet if the GM is already within a mog house when using the `//myroom` command again, regardless of any parameters given.
