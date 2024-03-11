# `Unknown - Packet: 0x0110`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0110` |
| **Size**                  | `0x0014` |

## Description

This packet is sent by the server to update the clients Unity quest information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    Sparks      : 24;
    uint32_t    unused00    : 8;
    uint16_t    Deeds;
    uint16_t    padding00;
    uint8_t     RoEUnityShared;
    uint8_t     RoEUnityLeader;
    uint8_t     unknown0E[6];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Sparks`

_The local clients Sparks count._

### `unused00`

_Unused bits._

### `Deeds`

_The local clients Deeds count._

### `padding00`

_Padding; unused._

### `RoEUnityShared`

_The current available Records of Eminence under the `Unity -> Shared` menu._

This value is the index for the currently available Records of Eminence quests under the Unity (Shared) menu. _(ie. `Unity (Shared A)`)_

This will be a value of 0 to 5.

### `RoEUnityLeader`

_The current available Records of Eminence under the `Unity -> (LeaderName)` menu._

This value is the index for the currently available Records of Eminence quests under the local clients Unity Leader menu. _(ie. `Unity (Apururu)`)_

This will be a value of 0 to 3.

### `unknown0E`

_Unknown._

The purpose of these values are currently unknown. They are generally set to `0xFF`. The client does not appear to use them currently.
