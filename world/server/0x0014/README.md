# `GP_SERV_COMMAND_TELL`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_TELL` |
| **Client Handler**        | `RecvMessageTell` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0014` |
| **Size**                  | `(varies)` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

This packet is believed to be used originally for `/tell` messages due to its name. However, this packet is now deprecated and is no longer used.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_TELL
struct GP_SERV_TELL
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    UniqueNo;   // PS2: UniqueNo
    uint16_t    ActIndex;   // PS2: ActIndex
    uint8_t     Attr;       // PS2: Attr
    uint8_t     padding00;  // PS2: dammy2
    uint8_t     Str[];      // PS2: Str
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_TELL`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `UniqueNo`

_The server id of the entity that sent the tell._

### `ActIndex`

_The target index of the entity that sent the tell._

### `Attr`

_The message attributes._

### `padding00`

_Padding; unused._

### `Str`

_The message string._

> [!WARNING]
> This string value is not guaranteed to be null-terminated!

## Additional Information

While this packet is no longer used for its original assumed purpose of displaying `/tell` messages, the client still has the handler code for it. The only part of the packet that is used within the handler is the `Str` message string. The client parses this raw string for auto-translate phrases and converts them then displays the parsed string.

_It is believed that this packet was deprecated during the time when packet exploits were being abused to crash zones and individual players by crafting invalid chat packets that used incomplete auto-translate tags, causing the client to crash when trying to parse them. Tell packets are now combined into the main chat packet (`0x17`) as a specific mode instead._
