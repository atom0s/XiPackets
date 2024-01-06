# `GP_SERV_COMMAND_EVENTSTR`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_EVENTSTR` |
| **Client Handler**        | `RecvEventCalcStr` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0033` |
| **Size**                  | `0x0038` |

## Description

This packet is sent by the server to begin an event on the client. _(With string based parameters.)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_EVENTSTR
struct GP_SERV_EVENTSTR
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    UniqueNo;       // PS2: UniqueNo
    uint16_t    ActIndex;       // PS2: ActIndex
    uint16_t    EventNum;       // PS2: EventNum
    uint16_t    EventPara;      // PS2: EventPara
    uint16_t    Mode;           // PS2: Mode
    char        String[4][16];  // PS2: String
    uint32_t    Data[8];        // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_EVENTSTR`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `UniqueNo`

_The event entity server id._

### `ActIndex`

_The event entity target index._

### `EventNum`

_The event number._

### `EventPara`

_The event param._

### `Mode`

_The event mode._

### `String`

_The event string parameters._

### `Data`

_The event data._
