# `GP_CLI_COMMAND_MYROOM_PLANT_CHECK`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_MYROOM_PLANT_CHECK` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00FD` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when examining a plant within their mog house.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_MYROOM_PLANT_CHECK
struct GP_CLI_MYROOM_PLANT_CHECK
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    MyroomPlantItemNo;      // PS2: MyroomPlantItemNo
    uint8_t     MyroomPlantItemIndex;   // PS2: MyroomPlantItemIndex
    uint8_t     MyroomPlantCategory;    // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_MYROOM_PLANT_CHECK`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `MyroomPlantItemNo`

_The item id of the plant/pot being examined._

### `MyroomPlantItemIndex`

_The index within the container that the plant/pot is located._

### `MyroomPlantCategory`

_The container that holds the plant/pot._
