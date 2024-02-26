# `Unknown - Packet: 0x00BF`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00BF` |
| **Size**                  | `0x001C` |

## Description

This packet is sent by the server to update a clients event information when interacting with a battlefield registration NPC. _(ie. Dynamis, Moblin Maze Mongers, Salvage, etc.)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    unknown04;
    uint16_t    Result;
    uint32_t    unknown08;
    uint32_t    ActIndex;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `unknown04`

_Unknown._

The client does not use this value.

### `Result`

_The result value._

This value is used to populate data within the obtained entities event VM.

### `unknown08`

_Unknown._

The client does not use this value.

### `ActIndex`

_The entity target index._

This value is the target index of the NPC being interacted with.

## Additional Information

This packet is sent by the server in response to a client interacting with a battlefield registration NPC, when the client has requested to enter a battlefield. When the client requests to enter the battlefield, accepts any entry fees or requirements, and begins waiting for the server to confirm their entry, their event VM will be in a loop waiting for a response from the server. This is done using the event opcode: `0x00A7`.

The client will use the `ActIndex` to find the desired entity and update its event VM information to populate it with the value stored in `Result` when this packet is received. Once this packet is received, the event VM will update its local variable information using the value from `Result` and step to the next opcode.

Reviewing multiple event scripts shows that a `Result` value of `4` equals the entry request was confirmed and accepted. Other values will be treated as an error, or failure to enter.
