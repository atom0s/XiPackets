# `GP_CLI_COMMAND_GROUP_LEAVE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_GROUP_LEAVE` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x006F` |
| **Size**                  | `0x0006` |

## Description

This packet is sent by the client when leaving a party or alliance.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_COMMAND_GROUP_LEAVE
struct GP_CLI_COMMAND_GROUP_LEAVE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Kind;       // PS2: Kind
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_COMMAND_GROUP_LEAVE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Kind`

_The kind of party invite._

| Kind | Purpose |
| `0` | _Leave Party_ |
| `5` | _Leave Alliance_ |
