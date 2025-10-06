# `GP_CLI_COMMAND_INSPECT_MESSAGE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_INSPECT_MESSAGE` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00DE` |
| **Size**                  | `0x0080` |

## Description

This packet is sent by the client when changing their bazaar message.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_INSPECT_MESSAGE
struct GP_CLI_INSPECT_MESSAGE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     sInspectMessage[123];   // PS2: sInspectMessage
    uint8_t     padding00;              // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_INSPECT_MESSAGE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `sInspectMessage`

_The bazaar message._

This value holds the full bazaar message. While the client does show the bazaar message as three separate lines visually, the actual buffer that holds the lines is a single array. Each line takes up 40 characters within the array, with a remaining 3 characters at the end being unused. The manner in which this buffer works is also different than other normal strings. By default the client treats every unused character in this buffer as a space _(`0x20`)_ character.

### `padding00`

_Padding; unused._
