# `GP_CLI_COMMAND_PASSWARDS`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_PASSWARDS` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0060` |
| **Size**                  | `0x001C` |

## Description

This packet is sent by the client when interacting with an event that expects the client to give an input string. _(ie. Passwords)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_PASSWARDS
struct GP_CLI_PASSWARDS
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    UniqueNo;   // PS2: UniqueNo
    uint16_t    ActIndex;   // PS2: ActIndex
    uint16_t    padding00;  // PS2: dammy
    uint8_t     String[16]; // PS2: String
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_PASSWARDS`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The event server id._

This value is set to the clients local `PTR_CliEventUniqueNo` value. This will generally be set to the local clients server id, however it can also be updated by the server with the various event related packets.

### `ActIndex`

_The event target index._

This value is set to the clients local `PTR_CliEventIndex` value. This will generally be set to the local clients target index, however it can also be updated by the server with the various event related packets.

### `padding00`

_Padding; unused._

### `String`

_The string input value from the client._
