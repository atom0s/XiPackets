# `Packet: 0x0116`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0116` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when opening the Unity menu. _(`Main Menu -> Status -> Unity`)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    Kind;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Kind`

_The packet kind._

This value is treated as a boolean.

## Additional Information

This packet is used by the client to request the current total Unity information on the server. This will be sent every time the client opens the `Main Menu -> Status -> Unity` menu. When this menu is open, the client will send two copies of this packet together. The first packet sent will have the `Kind` value set to `0` while the second has it set to `1`. This value is used to tell the server which block of Unity data is being requested.

The server responds to these packets by sending back `0x0063` packets, `32` packets for each of this packet to be exact. _(`64` packets in total each time the menu is opened.)_ The packets sent back from the server will be indexed based on the `Kind` value that was sent in this packet.

The first set of packets sent back from the server will have its block offset set to `0` and each packet will increment its block index value from 0 to 31. The second packet will have its block offset set to `1` and repeat the same kind of block index incrementing. These two values in the response packets are used to build out the total `PTR_UnityData` block of data.

More information about how this works can be found in the [0x0063](/world/server/0x0063/README.md) packet documentation.
