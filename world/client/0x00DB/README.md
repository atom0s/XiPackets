# `Packet: 0x00DB`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00DB` |
| **Size**                  | `0x0028` |

## Description

This packet is sent by the client when updating certain configuration settings such as chat filters or language preferences.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     unknown00;
    uint8_t     unknown01;
    uint8_t     Kind;
    uint8_t     padding00;
    uint32_t    ConfigSys[3];
    uint32_t    padding01[4];
    uint32_t    Param;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `unknown00`

_Unknown._

The client always sets this value to `0`.

### `unknown01`

_Unknown._

The client always sets this value to `0`.

### `Kind`

_The packet kind._

This value is used to determine the value that is set in the packets `Param` value.

| Kind | Purpose |
| --- | --- |
| `0` | _The `Param` value holds the clients `PTR_pGlobalNowZone->ConfSys.SearchLanguage`._ |
| `1` | _The `Param` value holds the clients current party search languages. (`Main Menu > Party > Languages`)_ |
| `2` | _Unknown. (The client has functions to generate this packet `Kind`, but nothing calls the function.)_ |
| `3` | _Unknown. (The client has functions to generate this packet `Kind`, but nothing calls the function.)_ |

### `padding00`

_Padding; unused._

### `ConfigSys`

_The clients configuration system values._

This array of values is set to the clients current `PTR_pGlobalNowZone->ConfSys.Configs` information. The client directly copies the first three `uint32_t` values from this system into the packet.

```cpp
pkt->ConfigSys[0] = PTR_pGlobalNowZone->ConfSys.Configs[0];
pkt->ConfigSys[1] = PTR_pGlobalNowZone->ConfSys.Configs[1];
pkt->ConfigSys[2] = PTR_pGlobalNowZone->ConfSys.Configs[2];
```

### `padding01`

_Padding; unused._

### `Param`

_The packet parameter._

This value is set based on the packet `Kind`.

When the packet `Kind` is `0`, this value will be set to the clients `PTR_pGlobalNowZone->ConfSys.SearchLanguage` value.

When the packet `Kind` is `1`, this value will be set to the clients current party search languages. _(`Main Menu > Party > Languages`)_ This is a flag based value as more than one language can be choosen at once:

  - `0x01` - _Japanese_
  - `0x02` - _English_
  - `0x04` - _German_
  - `0x08` - _French_
  - `0x10` - _Other_
