# `GP_SERV_COMMAND_DEBUGPRINT`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_DEBUGPRINT` |
| **Client Handler**        | `RecvDebufPrint` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0054` |
| **Size**                  | `0x000C` |

## Description

The purpose of this packet is unknown.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_DEBUGPRINT
struct GP_SERV_DEBUGPRINT
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    int32_t     data;           // PS2: data
    uint8_t     port;           // PS2: port
    uint8_t     padding00[3];   // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_DEBUGPRINT`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `data`

_The data value._

### `port`

_The port value._

This value is limited to values `0` to `7`.

### `padding00`

_Padding; unused._

## Additional Information

The intended purpose of this packet is unknown. The client still handles the data and will populate the internal buffers this was used with, however those buffers are no longer used anywhere in the client code. Originally, this was used to print data during the `AtelIdle` function, but that functionality has since been stripped from this function in the current retail client.
