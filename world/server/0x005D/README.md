# `Unknown - Packet: 0x005D`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x005D` |
| **Size**                  | `0x0068` |

## Description

This packet is sent by the server to update the clients event parameters.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; packet has been repurposed.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    int32_t     num[9];
    char        string1[16];
    char        string2[16];
    char        string3[16];
    char        string4[16];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `num`

_The event number parameters._

The client does not use the values of these at all. It's data is entirely ignored.

### `string1`, `string2`, `string3`, `string4`

_The event string parameters._

These values are copied into the clients `PTR_EventStrings` container in 256 byte blocks.
