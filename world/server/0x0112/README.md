# `Unknown - Packet: 0x0112`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0112` |
| **Size**                  | `0x0088` |

## Description

This packet is sent by the server to update the clients Records of Eminence quest log information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Data[128];
    uint16_t    Offset;
    uint16_t    padding00;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Data`

_The Records of Eminence quest data, represented in bits._

### `Offset`

_The starting offset that the `Data` is used for._

This value is used as an offset _(or index)_ into the Records of Eminence quest table where the `Data` information will be copied into. The client expects this to be a value of either 0, 1, 2 or 3. When used, the client will recalculate its value as follows: `offset = pkt->Offset << 7;`

### `padding00`

_Padding; unused._

## Additional Information

When populating the clients Records of Eminence quest log, the server will send multiple copies of this packet _(up to 4)_. Each packet will contain a different `Offset` value to index the given quest `Data` into the overall RoE quest information within the client. The information stored within the `Data` field holds the bits that represent the quests for the RoE system. If a given bit is set to 1, then the coorosponding quest that aligns to that bit is considered completed.
