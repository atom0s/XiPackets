# `GP_SERV_COMMAND_WEATHER`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_WEATHER` |
| **Client Handler**        | `RecvWeather` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0057` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the server to populate the clients mission and quest information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_WEATHER
struct GP_SERV_WEATHER
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    StartTime;
    uint16_t    WeatherNumber;
    uint16_t    WeatherOffsetTime;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_WEATHER`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `StartTime`

_The weather start time._

The client uses this value by multiplying it by `60`.

### `WeatherNumber`

_The weather id._

The weather number corrosponds to the following table of weather tags:

| Id | Resource Tag | Weather Icon(s) |
| --- | --- | --- |
| `0x00` | `fine` | _None._ |
| `0x01` | `suny` | _None._ |
| `0x02` | `clod` | _None._ |
| `0x03` | `mist` | _None._ |
| `0x04` | `dryw` | `Fire x1` |
| `0x05` | `heat` | `Fire x2` |
| `0x06` | `rain` | `Water x1` |
| `0x07` | `squl` | `Water x2` |
| `0x08` | `dust` | `Earth x1` |
| `0x09` | `sand` | `Earth x2` |
| `0x0A` | `wind` | `Wind x1` |
| `0x0B` | `stom` | `Wind x2` |
| `0x0C` | `snow` | `Ice x1` |
| `0x0D` | `bliz` | `Ice x2` |
| `0x0E` | `thdr` | `Lightning x1` |
| `0x0F` | `bolt` | `Lightning x2` |
| `0x10` | `aura` | `Light x1` |
| `0x11` | `ligt` | `Light x2` |
| `0x12` | `fogd` | `Dark x1` |
| `0x13` | `dark` | `Dark x2` |
| `0x14` | `fin1` | _None_ |
| `0x15` | `sun1` | _None_ |
| `0x16` | `clo1` | _None_ |
| `0x17` | `mis1` | _None_ |
| `0x18` | `dry1` | _None_ |
| `0x19` | `hea1` | _None_ |
| `0x1A` | `rai1` | _None_ |
| `0x1B` | `squ1` | _None_ |
| `0x1C` | `dus1` | _None_ |
| `0x1D` | `san1` | _None_ |
| `0x1E` | `win1` | _None_ |
| `0x1F` | `sto1` | _None_ |
| `0x20` | `sno1` | _None_ |
| `0x21` | `bli1` | _None_ |
| `0x22` | `thd1` | _None_ |
| `0x23` | `bol1` | _None_ |
| `0x24` | `aur1` | _None_ |
| `0x25` | `lig1` | _None_ |
| `0x26` | `fog1` | _None_ |
| `0x27` | `dar1` | _None_ |

_**Note:** Weather effects do not work in every zone. Only some zones can play the full animation/effects of a given kind of weather._

### `WeatherOffsetTime`

_The weather time offset._

The client uses this value by multiplying it by `3600`.
