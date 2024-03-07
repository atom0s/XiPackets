# `GP_SERV_COMMAND_TRACKING_POS`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_TRACKING_POS` |
| **Client Handler**        | `RecvPos` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00F5` |
| **Size**                  | `0x0018` |

## Description

This packet is sent by the server to update the clients currently tracked entity. _(via Wide Scan)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_TRACKING_POS_STATE
enum GP_TRACKING_POS_STATE
{
    GP_TRACKING_POS_STATE_NONE,
    GP_TRACKING_POS_STATE_START,
    GP_TRACKING_POS_STATE_LOSE,
    GP_TRACKING_POS_STATE_END
};

// PS2: GP_SERV_TRACKING_POS
struct GP_SERV_TRACKING_POS
{
    uint16_t                id: 9;
    uint16_t                size: 7;
    uint16_t                sync;
    float                   x;          // PS2: x
    float                   y;          // PS2: y
    float                   z;          // PS2: z
    uint8_t                 Level;      // PS2: Level
    uint8_t                 unused;     // PS2: Dammy
    uint16_t                ActIndex;   // PS2: (New; did not exist.)
    GP_TRACKING_POS_STATE   State;      // PS2: State
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_TRACKING_POS`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `x`

_The tracked entities X position._

### `y`

_The tracked entities Y position._

### `z`

_The tracked entities Z position._

### `Level`

_The tracked entities level._

### `unused`

_Unused; junk._

### `ActIndex`

_The tracked entities target index._

### `State`

_The tracking state._

| State | Name | Purpose |
| --- | --- | --- |
| `0x00` | `GP_TRACKING_POS_STATE_NONE`  | _None._ |
| `0x01` | `GP_TRACKING_POS_STATE_START` | _Begin tracking and position updates._ |
| `0x02` | `GP_TRACKING_POS_STATE_LOSE`  | _Lost sight of the tracked entity._ |
| `0x03` | `GP_TRACKING_POS_STATE_END`   | _None._ |
