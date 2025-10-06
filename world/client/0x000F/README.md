# `GP_CLI_COMMAND_CLSTAT`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_CLSTAT` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x000F` |
| **Size**                  | `0x0024` |

## Description

This packet is sent by the client when it has a zero-value state for its own equipment system.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_CLSTAT
struct GP_CLI_CLSTAT
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    stat[8]; // PS2: stat
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_CLSTAT`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `stat`

_The clients game status information._

The client will copy a block of data into this value when the packet is built. This block of data is generally always all zeros. The client currently only writes to the first `uint32_t` value during a very specific condition.

When the client is heavily lagging, it can put itself into a state of effectively 'speed hacking' the entire process causing it to try and catch up with itself to get back in sync between frame times. If the client is lagging so badly that it becomes 'stuck' in this state, it will begin incrementing an internal counter ~3 seconds. When this counter ticks up to over 45 times, the client will set the first value that `stat` is set to to `1`. This tells the server the client is basically stuck in a race trying to catch up. _(At this time, it is unknown what the server will do if the client sends this value.)_

_**Note:** Due to how this value becomes set, it is NOT advised to test this condition on an account that matters to you if you wish to inject this packet with the flag set! This could be considered a cheat condition the client monitors for!_
