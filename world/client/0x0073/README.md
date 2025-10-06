# `GP_CLI_COMMAND_GROUP_CHANGE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_GROUP_CHANGE` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0073` |
| **Size**                  | `0x000C` |

## Description

> [!NOTE]
> This is a special GM-related packet!

This packet was originally used with certain GM commands that are no longer available in the stock retail client. It was used to manipulate group systems as a GM. _(ie. Party, Alliance, Linkshell)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_GROUP_CHANGE
struct GP_CLI_GROUP_CHANGE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    UniqueNo;   // PS2: UniqueNo
    uint16_t    ActIndex;   // PS2: ActIndex
    uint8_t     Kind;       // PS2: Kind
    uint8_t     ChangeKind; // PS2: ChangeKind
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_GROUP_CHANGE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The server id of the player being changed._

### `ActIndex`

_The target index of the player being changed._

### `Kind`

_The packet kind._

This value is used to determine what kind of group is being modified.

| Kind | Purpose |
| --- | --- |
| `0` | _Party_ |
| `1` | _Linkshell_ |
| `2` | _Alliance_ |

### `ChangeKind`

_The packet change kind._

This value appears to always be set to `0`.
