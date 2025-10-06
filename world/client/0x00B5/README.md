# `GP_CLI_COMMAND_CHAT_STD`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_CHAT_STD` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00B5` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the client when sending chat messages.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_CHAT_STD
struct GP_CLI_CHAT_STD
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     Kind;       // Kind
    uint8_t     unknown00;  // Dammy
    uint8_t     Str[];      // Str
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_CHAT_STD`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Kind`

_The message kind._

This value determines the kind of chat message that is being sent by the client. These are the valid message kinds:

| Mode | Kind | Command |
| --- | --- | --- |
| `0x00` | `Say`            | `/say`        |
| `0x01` | `Shout`          | `/shout`      |
| `0x04` | `Party`          | `/party`      |
| `0x05` | `Linkshell (1)`  | `/linkshell`  |
| `0x08` | `Emote Message`  | `/emote`      |
| `0x18` | `Linkshell (PvP)`| `/linkshell`  |
| `0x1A` | `Yell`           | `/yell`       |
| `0x1B` | `Linkshell (2)`  | `/linkshell2` |
| `0x21` | `Unity`          | `/unity`      |
| `0x22` | `Assist (J)`     | `/assistj`    |
| `0x23` | `Assist (E)`     | `/assiste`    |

_**Note:** When the client is participating in PvP, the linkshell chat modes will be overridden and make use of the `0x18` mode instead. All members of the same team will be placed into a team-based chat that uses the Linkshell mode to communicate._

### `unknown00`

_Unknown._

This value is used as a flag, however the client always sets it to `0`.

### `Str`

_The message string._
