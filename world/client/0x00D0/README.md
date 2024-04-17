# `GP_CLI_COMMAND_MYROOM_DIARY`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_MYROOM_DIARY` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00D0` |
| **Size**                  | `0x0004` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

The purpose of this packet is unknown. The below information is based on the PS2 data.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: _GP_MYROOM_DIARY_REQ
struct _GP_MYROOM_DIARY_REQ
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`_GP_MYROOM_DIARY_REQ`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

## Additional Information

The naming of this packet and the response packet information from the server implies that this may have been used allow players to view another players lifetime statistics. _(Kill count, death count, other interaction stats, etc.)_ Or to have some kind of guest book system when visiting others' mog houses.
