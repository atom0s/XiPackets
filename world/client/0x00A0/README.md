# `GP_CLI_COMMAND_SWITCH_PROPOSAL`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_SWITCH_PROPOSAL` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00A0` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the client when making a proposal. _(via `/nominate` or `/propose`)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_SWITCH_PROPOSAL
struct GP_CLI_SWITCH_PROPOSAL
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Kind;   // PS2: Kind
    uint8_t     Str[];  // PS2: Str
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_SWITCH_PROPOSAL`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Kind`

_The proposal kind._

This value is used as the chat mode parameter that is given when creating the proposal.

| Kind | Purpose |
| --- | --- |
| `0x01` | _Party_ |
| `0x02` | _Linkshell (1)_ |
| `0x03` | _Linkshell (2)_ |
| `0x05` | _Say_ |
| `0x06` | _Shout_ |

### `Str`

_The proposal string._

This value is the clients question and options that were given when using the `/nominate` or `/propose` command. If the client is cancelling their proposal, then this will be a null string.
