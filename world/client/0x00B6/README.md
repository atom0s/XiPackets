# `GP_CLI_COMMAND_CHAT_NAME`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_CHAT_NAME` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00B6` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the client when sending tells to another player.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_CHAT_NAME
struct GP_CLI_CHAT_NAME
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     unknown04;  // PS2: Dammy
    uint8_t     unknown05;  // PS2: (New; did not exist.)
    uint8_t     sName[15];  // PS2: sName
    uint8_t     Mes[];      // PS2: Mes
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_CHAT_NAME`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `unknown04`

_Unknown._

This value is used as a flag, however the client always sets it to `3`.

### `unknown05`

_Unknown._

This value is used as a flag, however the client always sets it to `0`.

### `sName`

_The name of the recipient who should receive the message._

### `Mes`

_The message string._
