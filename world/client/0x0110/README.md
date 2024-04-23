# `Packet: 0x0110`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0110` |
| **Size**                  | `0x0014` |

## Description

This packet is sent by the client when interacting with the fishing mini-game system.

_**Please Note:** This packet is meant for use with the newer fishing mini-game system. The fishing system has undergone several changes and 2 major overhauls over the course of the games history. The older revision of the system is still in the client, although not directly used. This packet is used with the newer system. The older system makes use of the packet `0x0066` instead._

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    UniqueNo;
    int32_t     para;
    uint16_t    ActIndex;
    int8_t      mode;
    uint8_t     padding00;
    int32_t     para2;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The local clients server id._

### `para`

_The fishing parameter._

This value is set depending on the `mode` value.

### `ActIndex`

_The local clients target index._

### `mode`

_The fishing mode._

This value is used to determine the current state the client is in while playing out the fishing mini-game.

| Mode | Purpose |
| --- | --- |
| `0` | _Unused._ |
| `1` | _Unused._ |
| `2` | _Request: Check Hook_ |
| `3` | _Request: End Mini-Game_ |
| `4` | _Request: Release_ |
| `5` | _Request: Potential Timeout_ |

### `padding00`

_Padding; unused._

### `para2`

_The fishing parameter (2)._

This value is set depending on the `mode` value.

## Additional Information

This packet is used by the client throughout the old Fishing mini-game. While most of the mini-game happens on the client, there are some parts that still take place on the server side which the client uses this packet to communicate with the server to be updated what various factors or to inform the server of different updates that have happened on the client.

The `mode` value is used to tell the server what kind of request action is being made by the client. This value will depend on the current state of the mini-game on the client side. Some `mode` values have multiple purposes based on the current state of the mini-game.

## Additional Information - `Mode`: `2`

This `mode` is used by the client to request that the server inform it if anything has bitten the fishing bait/hook. When the client first performs a `/fish` action, it will request from the server if it is valid to fish in its current location and state. If the request is valid, then the client will begin the fishing animation of pulling out their rod and casting their line. While this happens, an internal counter is ticked down each frame in the client, simulating the 'wait' of a fish to bite the line. Once this timer has reached 0, the client will send this packet with the `mode` value of `2`, requesting that the server inform it if anything bit the hook.

When this `mode` is used, the following values are set in the packet:

| Field | Value |
| --- | --- |
| `para`    | _Set to: `0`_ |
| `mode`    | _Set to: `2`_ |
| `para2`   | _Set to: `0`_ |

## Additional Information - `Mode`: `3`

This `mode` is used by the client to request that the server complete the fishing mini-game with the client. There are multiple usages of this `mode` by the client depending on the different ways the mini-game can be completed or exited. This includes things such as successfully fighting the fish and catching it, failing to fight the fish and losing it, force-exiting the fishing mini-game _(ie. pressing escape or being interrupted)_, attempting to catch the fish too early/soon, running out of time etc. Either way, this `mode` notes that the client has completed the mini-game and wishes to exit it.

When this `mode` is used, the packets values will depend on the manner in which the client is exiting the mini-game:

### Client Fails To Catch The Fish (ie. Lack of Skill or Timeout)

| Field | Value |
| --- | --- |
| `para`    | _Set to: `300`_ |
| `mode`    | _Set to: `3`_ |
| `para2`   | _Set to: `0`_ |

### Client Exits Mini-Game By Force (Pressing Escape or Interrupted)

| Field | Value |
| --- | --- |
| `para`    | _Set to: `200`_ |
| `mode`    | _Set to: `3`_ |
| `para2`   | _Set to: `0`_ |

### Client Attempts To Catch Fish Too Early (ie. Pressing Enter Before Fish Stamina Is Low Enough or 0)

| Field | Value |
| --- | --- |
| `para`    | _Set to: `(Fishes remaining stamina.)`_ |
| `mode`    | _Set to: `3`_ |
| `para2`   | _Set to: `(Intuition/special value reflected from server.)`_ |

### Client Catches The Fish Successfully

| Field | Value |
| --- | --- |
| `para`    | _Set to: `0`_ |
| `mode`    | _Set to: `3`_ |
| `para2`   | _Set to: `(Intuition/special value reflected from server.)`_ |

## Additional Information - `Mode`: `4`

This `mode` is used by the client to request that it be released from the fishing mini-game event. The client uses this `mode` after it has finished playing the resulting animation from the previous `mode` value `3` packet. That previous packet will cause the client to play the finishing animation related to what action occurred during their fishing mini-game. _(ie. playing the animation to catch the fish, break their line, or simply put away their rod.)_ While this happens, an internal counter is ticked down each frame in the client, simulating the 'wait' of letting the animation play out in full. Once this timer has reached 0, the client will send this packet with the `mode` value of `4`, requesting that the server release the client from the mini-game fully.

| Field | Value |
| --- | --- |
| `para`    | _Set to: `0`_ |
| `mode`    | _Set to: `4`_ |
| `para2`   | _Set to: `0`_ |

## Additional Information - `Mode`: `5`

This `mode` is used by the client while fighting a fish on their line when they are getting close to timing out and losing their catch. This allows the server to react by sending a message regarding the clients current status, generally on the lines of _"You don't know how much longer you can hold on!"_ and other additional information related to the current fishing state.

| Field | Value |
| --- | --- |
| `para`    | _Set to: `(Remaining time to fight the fish.)`_ |
| `mode`    | _Set to: `5`_ |
| `para2`   | _Set to: `0`_ |
