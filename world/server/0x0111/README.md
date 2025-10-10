# `Unknown - Packet: 0x0111`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0111` |
| **Size**                  | `0x0104` |

## Description

This packet is sent by the server to update the clients current set Records of Eminence quest information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct record_t
{
    uint32_t    Id    : 12; // PS2: (New; did not exist.)
    uint32_t    Count : 20; // PS2: (New; did not exist.)
};

// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    records[64]; // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `records`

_The quest records for the clients current Records of Eminence quest log._

## Structure Fields (`record_t`)

### `Id`

_The quest id._

### `Count`

_The quest progress._

This value is the number of points towards the quests total progress.

## Additional Information

This packet is sent by the server each time there is any update to the local clients current set Records of Eminence quest log. This is for the quests the client has currently set and active. This is also sent any time the client requests the current quest log when opening the Quests menu.
