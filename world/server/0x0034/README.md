# `GP_SERV_COMMAND_EVENTNUM`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_EVENTNUM` |
| **Client Handler**        | `RecvEventCalcNum` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0034` |
| **Size**                  | `0x0034` |

## Description

This packet is sent by the server to begin an event on the client. _(With number based parameters.)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_EVENTNUM
struct GP_SERV_EVENTNUM
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    UniqueNo;   // PS2: UniqueNo
    int32_t     num[8];     // PS2: num
    uint16_t    ActIndex;   // PS2: ActIndex
    uint16_t    EventNum;   // PS2: EventNum
    uint16_t    EventPara;  // PS2: EventPara
    uint16_t    Mode;       // PS2: Mode
    uint16_t    EventNum2;  // PS2: (New; did not exist.)
    uint16_t    EventPara2; // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_EVENTNUM`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The event entity server id._

### `num`

_The event numeric parameters._

### `ActIndex`

_The event entity target index._

### `EventNum`

_The event number._

### `EventPara`

_The event parameter._

### `Mode`

_The event mode._

### `EventNum2`

_The event number. (2)_

### `EventPara2`

_The event parameter. (2)_
