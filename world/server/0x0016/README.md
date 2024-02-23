# `GP_SERV_COMMAND_TALK`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_TALK` |
| **Client Handler**        | `RecvMessageTalk` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0016` |
| **Size**                  | `(varies)` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

This packet is believed to be used originally for general chat messages due to its name. However, this packet is now deprecated and is no longer used.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_TALK
struct GP_SERV_TALK
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Attr;       // PS2: Attr
    uint8_t     Channel;    // PS2: Channel
    uint8_t     Type;       // PS2: Type
    uint8_t     Str[];      // PS2: Str
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_TALK`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Attr`

_The message attributes._

### `Channel`

_The message channel id._

### `Type`

_The message type._

### `Str`

_The message string._

> [!WARNING]
> This string value is not guaranteed to be null-terminated!

## Additional Information

While this packet is no longer used for its original assumed purpose of displaying chat messages, the client still has the handler code for it. The only part of the packet that is used within the handler is the `Str` message string. The client parses this raw string for auto-translate phrases and converts them then displays the parsed string.

_It is believed that this packet was deprecated during the time when packet exploits were being abused to crash zones and individual players by crafting invalid chat packets that used incomplete auto-translate tags, causing the client to crash when trying to parse them. Chat messages are now handled via `0x17` packets instead._
