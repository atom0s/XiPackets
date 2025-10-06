# `GP_CLI_COMMAND_MYROOM_JOB`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_MYROOM_JOB` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0100` |
| **Size**                  | `0x0006` |

## Description

This packet is sent by the client when requesting to change jobs.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_MYROOM_JOB
struct GP_CLI_MYROOM_JOB
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     MainJobIndex;   // PS2: MainJobIndex
    uint8_t     SupportJobIndex;// PS2: SupportJobIndex
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_MYROOM_JOB`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `MainJobIndex`

_The index of the main job the client wishes to change to._

This value is set to `0` when the client is changing their support job.

### `SupportJobIndex`

_The index of the support job the client wishes to change to._

This value is set to `0` when the client is changing their main job.
