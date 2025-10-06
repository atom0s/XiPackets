# `GP_CLI_COMMAND_EQUIP_SET`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_EQUIP_SET` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0050` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when changing equipment.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_EQUIP_SET
struct GP_CLI_EQUIP_SET
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     PropertyItemIndex;  // PS2: PropertyItemIndex
    uint8_t     EquipKind;          // PS2: EquipKind
    uint8_t     Category;           // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_EQUIP_SET`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `PropertyItemIndex`

_The index within the container where the item is located to be equipped._

When the slot is being unequipped, this value is set to `0`.

### `EquipKind`

_The equipment slot being modified._

### `Category`

_The container that holds the item being equipped or unequipped._
