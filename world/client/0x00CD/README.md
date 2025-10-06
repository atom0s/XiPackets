# `GP_CLI_COMMAND_MYROOM_PLANT`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_MYROOM_PLANT` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00CD` |
| **Size**                  | `0x000A` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

> [!NOTE]
> This is possibly a special GM-related packet!

The purpose of this packet is unknown.

It is possible this packet was once used for a set of GM commands:

  - `//plantadd`
  - `//plantcheck`
  - `//plantcrop`
  - `//plantstop`

_**Note:** These commands have since been changed to use a newer set of packets._

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    unknown00;
    uint16_t    unknown01;
    uint8_t     unknown02;
    uint8_t     unknown03;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `unknown00`

_Unknown._

### `unknown01`

_Unknown._

### `unknown02`

_Unknown._

### `unknown03`

_Unknown._

## Additional Information

This packet is deprecated and no longer used by the client. While it is connected to the client command `MYROOM_PLANT`, there is no usage of the function that generates the packet. The original PS2 beta does not make use of this packet id at all either, thus the function was added sometime after the PS2 beta period.

This packet had additional sub-packets that are used with it that map to the same base structure layout:

```cpp
// PS2: GP_CLI_MYROOM_PLANT_ADD
struct GP_CLI_MYROOM_PLANT_ADD
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    MyroomPlantItemNo;      // PS2: MyroomPlantItemNo
    uint16_t    MyroomAddItemNo;        // PS2: MyroomAddItemNo
    uint8_t     MyroomPlantItemIndex;   // PS2: MyroomPlantItemIndex
    uint8_t     MyroomAddItemIndex;     // PS2: MyroomAddItemIndex
};

// PS2: GP_CLI_MYROOM_PLANT_CHECK
struct GP_CLI_MYROOM_PLANT_CHECK
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    MyroomPlantItemNo;      // PS2: MyroomPlantItemNo
    uint8_t     MyroomPlantItemIndex;   // PS2: MyroomPlantItemIndex
};

// PS2: GP_CLI_MYROOM_PLANT_CROP
struct GP_CLI_MYROOM_PLANT_CROP
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    MyroomPlantItemNo;      // PS2: MyroomPlantItemNo
    uint8_t     MyroomPlantItemIndex;   // PS2: MyroomPlantItemIndex
    uint8_t     CancellFlg;             // PS2: CancellFlg
};

// PS2: GP_CLI_MYROOM_PLANT_STOP
struct GP_CLI_MYROOM_PLANT_STOP
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    MyroomPlantItemNo;      // PS2: MyroomPlantItemNo
    uint8_t     MyroomPlantItemIndex;   // PS2: MyroomPlantItemIndex
};
```
