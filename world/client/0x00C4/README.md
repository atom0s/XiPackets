# `GP_CLI_COMMAND_GROUP_COMLINK_ACTIVE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_GROUP_COMLINK_ACTIVE` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00C4` |
| **Size**                  | `0x001C` |

## Description

This packet is sent by the client when requesting to create a linkshell or when equipping/unequipping a linkshell item.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_GROUP_COMLINK_ACTIVE
struct GP_CLI_GROUP_COMLINK_ACTIVE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    r : 4;              // PS2: r
    uint16_t    g : 4;              // PS2: g
    uint16_t    b : 4;              // PS2: b
    uint16_t    a : 4;              // PS2: a
    uint8_t     ItemIndex;          // PS2: ItemIndex
    uint8_t     Category;           // PS2: (New; did not exist.)
    uint8_t     ActiveFlg;          // PS2: ActiveFlg
    uint8_t     padding09[3];       // PS2: (New; did not exist.)
    uint8_t     sComLinkName[15];   // PS2: sComLinkName
    uint8_t     LinkshellId;        // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_GROUP_COMLINK_ACTIVE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `r`

_The linkshell color. (r)_

This value is used for the linkshell color, clamped to 4 bits per color. _(Values 0 to 15.)_

### `g`

_The linkshell color. (g)_

This value is used for the linkshell color, clamped to 4 bits per color. _(Values 0 to 15.)_

### `b`

_The linkshell color. (b)_

This value is used for the linkshell color, clamped to 4 bits per color. _(Values 0 to 15.)_

### `a`

_The linkshell color. (a)_

The client will always set this value to `0x15`.

### `ItemIndex`

_The location within the container that the linkshell item is located._

### `Category`

_The inventory container that holds the linkshell item._

### `ActiveFlg`

_Flag state thats if the linkshell is being equipped or unequipped or if a new linkshell is being created._

  - `0` - _Unequip_
  - `1` - _Equip (or Create)_

### `padding09`

_Padding; unused._

### `sComLinkName`

_The linkshell name._

This value is the linkshell name, however it has special conditions on how its used. When the client is equipping or unequipping an already existing linkshell, or swapping to another linkshell, this value will be set to the string `dummy`.

When the client is creating a brand new linkshell, then this value will be set to the linkshell name, but compressed using a custom 6-bit compression setup.

### `LinkshellId`

_The linkshell id being modified._

  - `1` - _Linkshell 1_
  - `2` - _Linkshell 2_
