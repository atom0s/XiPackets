# `GP_CLI_COMMAND_ITEM_MOVE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_ITEM_MOVE` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0029` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the client when moving items around containers.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_ITEM_MOVE
struct GP_CLI_ITEM_MOVE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    ItemNum;    // PS2: ItemNum
    uint8_t     Category1;  // PS2: Category1
    uint8_t     Category2;  // PS2: Category2
    uint8_t     ItemIndex1; // PS2: ItemIndex1
    uint8_t     ItemIndex2; // PS2: ItemIndex2
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_ITEM_MOVE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ItemNum`

_The quantity of the item to be moved._

### `Category1`

_The container holding the item._

### `Category2`

_The container to move the item to._

### `ItemIndex1`

_The index inside of the container (`Category1`) this item is currently located._

### `ItemIndex2`

_The index inside of the container (`Category2`) this item is being moved to._

This value will be set to `82` when the client is moving items to another container.
