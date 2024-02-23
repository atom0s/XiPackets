# `GP_SERV_COMMAND_PACKETCONTROL`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_PACKETCONTROL` |
| **Client Handler**        | `RecvPacketControl` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0005` |
| **Size**                  | `0x001C` |

## Description

This packet is sent by the server to configure the clients rate limiting.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_PACKETCONTROL
struct GP_SERV_PACKETCONTROL
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    PacketCnt;
    uint32_t    padding00[5];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_PACKETCONTROL`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `PacketCnt`

_The new packet count value to set._

### `padding00`

_Padding; unused._

## Additional Information

This packet is used to configure and adjust the clients current packet rate limiting. The client and server both send packets in chunks to each other rather than sending data the moment it is ready. Instead, local queues are built up with packet data to be sent then those packets are chunked together into single larger packets to be separated and processed once received on the other end.

To limit how often the queue is processed, the client has values and timing data that is used to handle that rate limiting.

Every frame, the client will check to see if the current rate limit amount of time has passed since the last queued chunk was sent. If it hasn't, the client skips processing the current send queue and does not process any queued packet data to be sent. Once the time delay has passed, it will then call `FUNC_SendProc` which is the clients local function used to process the mentioned queue.

The first thing that `FUNC_SendProc` does is additional timing adjustments on this rate limiter. This is done to allow the client to adapt to lag and adjust its rate limit time accordingly by either increasing or decreasing the queue processing delay. The better the connection, the quicker the client will equalize the delay.

The two main values the client is using to perform this limiting are:

  - `PTR_pZoneSys->PacketCnt`
  - `PTR_pZoneSys->ZonePtr->SendTimer`

When the client zones, it will reinitialize these values to predefine defaults that are set within the client.

  - `PacketCnt` is set to `400`. _(This can be configured via the command line of the client.)_
  - `SendTimer` is set to `PacketCnt * 5`. _(Thus, `2000` by default.)_

Next, each frame tick will result in the client doing the timing check and calling `FUNC_SendProc` each time the delay has passed. Each successful call will then process the timing values as follows:

```cpp
// Calculate the current packet count difference..
cdiff = zone->ClientCnt - zone->CheckCnt;

if (cdiff < -1 || cdiff > 1)
{
    // Increase the timer if the client is lagging..

    const auto stimer = zone->SendTimer + 50;
    const auto pcount = PTR_pZoneSys->PacketCnt + 3000;

    zone->SendTimer = stimer;

    if (pcount < stimer)
        zone->SendTimer = pcount;
}
else
{
    // Decrease the timer if the client is not lagging..

    const auto stimer = zone->SendTimer;
    const auto pcount = PTR_pZoneSys->PacketCnt;

    if (stimer > pcount)
    {
        zone->SendTimer = stimer - pcount;

        if ((stimer - pcount) < pcount)
            zone->SendTimer = pcount;
    }
}
```

Under normal conditions where the client is not lagging, this would result in `SendTimer` being decreased by `400` each time `FUNC_SendProc` was actually called until it reaches `400` itself and evens out.
