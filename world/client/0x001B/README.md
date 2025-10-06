# `GP_CLI_COMMAND_FRIENDPASS`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_FRIENDPASS` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x001B` |
| **Size**                  | `0x001C` |

## Description

This packet is sent by the client when it is interacting with, and ultimately purchasing from, a world pass vendor NPC.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_FRIENDPASS
struct GP_CLI_FRIENDPASS
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    Para;       // PS2: Para
    uint16_t    padding00;  // PS2: Dammy
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_FRIENDPASS`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Para`

_The parameter of the packet._

| Id | Purpose |
| --- | --- |
| `0` | _Client has requested to begin the purchase of a world pass._ |
| `1` | _Client has confirmed the purchase of a world pass._ |
| `2` | _Client has requested to begin the purchase of a gold world pass._ |
| `3` | _Client has confirmed the purchase of a gold world pass._ |

_**Note:** If the client rejects the confirmation for either world pass type, it will simply exit the event. It does not send a packet to the server._

### `padding00`

_Padding; unused._
