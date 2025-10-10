# `GP_SERV_COMMAND_EVENT`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_EVENT` |
| **Client Handler**        | `RecvEventCalc` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0032` |
| **Size**                  | `0x0014` |

## Description

This packet is sent by the server to begin an event on the client.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_EVENT
struct GP_SERV_EVENT
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    UniqueNo;   // PS2: UniqueNo
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

## Structure Fields (`GP_SERV_EVENT`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The entity server id._

### `ActIndex`

_The entity target index._

### `EventNum`

_The event number._

### `EventPara`

_The event param._

### `Mode`

_The event mode._

### `EventNum2`

_The event number. (2)_

### `EventPara2`

_The event param. (2)_

## Additional Information

This packet is used to initialize and prepare the client to begin playing an event. When this packet is received, the handler will first check if the client is already in an event. If an event is already started, then an error message will be printed and the handler will exit.

The client uses the `EventNum` and `EventNum2` values to determine which DAT file is loaded for the events data and messages. There is a specific check that happens which can cause the file id to be offset. This generally occurs when the client is in an area that has a sub-instance id. The check for this looks like:

```cpp

// zone_sub_no  = PTR_pGlobalNowZone->ZoneSubNo (The current zone sub number.)
// event_num    = pkt->EventNum
// event_no     = pkt->EventNum2

if (zone_sub_no >= 1000 && zone_sub_no <= 1299 || (evnum = event_num, zsno = event_no, event_num != event_no))
    evnum = zsno + 1000;
```

`EventNum` and `EventNum2` are generally set to the zone id for the event. With `EventNum2` being used for the sub-id during certain conditions / events.
