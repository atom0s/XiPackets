# `GP_SERV_COMMAND_RES`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_RES` |
| **Client Handler**        | `RecvServRes` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00F9` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the server to update the clients current death menu options. This is used for Raise, Reraise and Tractor.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_RES
struct GP_SERV_RES
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    UniqueNo;   // PS2: UniqueNo
    uint16_t    ActIndex;   // PS2: ActIndex
    uint16_t    type;       // PS2: type
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_RES`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The entity server id._

The client does not use this value.

### `ActIndex`

_The entity target index._

The client does not use this value.

### `type`

_The type of adjustment being made to the menu._

| Type | Purpose |
| --- | --- |
| `0` | _Resets the menu back to just the default homepoint menu._ |
| `1` | _Displays the raise sub-menu due to being Raised or having Reraise status._ |
| `2` | _Displays the tractor sub-menu._ |

_**Note:** The client will handle any other `Type` value as 0._

## Additional Information

When the client dies, it will automatically display the homepoint menu by default. The server can send this packet to the client to update the menu with additional options such as if the client has been raised or had reraise effect upon death or if they have been tractored by another player. In the event that the client died with reraise, this packet will not be sent immediately upon their death. There is a small delay before the reraise status is triggered.
