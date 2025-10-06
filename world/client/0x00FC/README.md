# `GP_CLI_COMMAND_MYROOM_PLANT_ADD`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_MYROOM_PLANT_ADD` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00FC` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the client when interacting with a plant within their mog house. _(Sowing seeds or nourishing with crystals.)_

## Packet Layout

The layout of this packet is the following:

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
    uint8_t     MyroomPlantCategory;    // PS2: (New; did not exist.)
    uint8_t     MyroomAddCategory;      // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_MYROOM_PLANT_ADD`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `MyroomPlantItemNo`

_The item id of the plant/pot being modified._

### `MyroomAddItemNo`

_The item id of the item being applied to the plant/pot._

### `MyroomPlantItemIndex`

_The index within the container that the plant/pot is located._

### `MyroomAddItemIndex`

_The index within the container that the item being applied is located._

### `MyroomPlantCategory`

_The container that holds the plant/pot._

### `MyroomAddCategory`

_The container that holds the item being applied to the plant/pot._
