# `GP_CLI_COMMAND_ITEM_USE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_ITEM_USE` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0037` |
| **Size**                  | `0x0014` |

## Description

This packet is sent by the client when completing a trade with an NPC.

```cpp
// PS2: GP_CLI_ITEM_USE
struct GP_CLI_ITEM_USE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    UniqueNo;           // PS2: UniqueNo
    uint32_t    ItemNum;            // PS2: ItemNum
    uint16_t    ActIndex;           // PS2: ActIndex
    uint8_t     PropertyItemIndex;  // PS2: PropertyItemIndex
    uint8_t     padding0F;          // PS2: dammy2
    uint32_t    Category;           // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_ITEM_USE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The server id of the entity the item is being used on._

### `ItemNum`

_The count of items being used._

This value is always set to `0` by the client. The server is responsible for the amount of items that should be used/consumed.

### `ActIndex`

_The target index of the entity the item is being used on._

### `PropertyItemIndex`

_The index within the container the item is located._

### `padding0F`

_Padding; unused._

### `Category`

_The container holding the item._
