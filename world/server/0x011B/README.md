# `Unknown - Packet: 0x011B`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x011B` |
| **Size**                  | `(unknown)` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

This packet is sent by the server to toggle the clients style lock status. _(`/lockstyle`)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Status;
    uint8_t     padding00[3];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Status`

_The style lock status._

This value is treated as a boolean. The client will check the current style lock status and then compare its state against the desired `Status` value here.

```cpp
const auto cfg =  FUNC_GetConfig(191); // Get style lock status..
if (pkt->Status)
{
    if (pkt->Status == 1 && !cfg)
        FUNC_SetConfig(191, 1);
}
else if (cfg == 1)
{
    FUNC_SetConfig(191, 0);
}
```

### `padding00`

_Padding; unused._

## Additional Information

This packet is believed to be deprecated as the lock style system makes use of other packets now. While the client can still handle this packet properly, and the toggling works, it has not been observed on current retail.
