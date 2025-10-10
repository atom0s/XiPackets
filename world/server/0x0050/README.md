# `GP_SERV_COMMAND_EQUIP_LIST`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_EQUIP_LIST` |
| **Client Handler**        | `RecvEquipList` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0050` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the server to inform the client of how to equip a certain equipment slot.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: SAVE_EQUIP_KIND
enum SAVE_EQUIP_KIND : uint8_t
{
    SAVE_EQUIP_KIND_RIGHTHAND,
    SAVE_EQUIP_KIND_LEFTHAND,
    SAVE_EQUIP_KIND_BOW,
    SAVE_EQUIP_KIND_ARROW,
    SAVE_EQUIP_KIND_HEAD,
    SAVE_EQUIP_KIND_BODY,
    SAVE_EQUIP_KIND_ARM,
    SAVE_EQUIP_KIND_LEG,
    SAVE_EQUIP_KIND_FOOT,
    SAVE_EQUIP_KIND_NECK,
    SAVE_EQUIP_KIND_BELT,
    SAVE_EQUIP_KIND_RIGHTEAR,
    SAVE_EQUIP_KIND_LEFTEAR,
    SAVE_EQUIP_KIND_RIGHTFINGER,
    SAVE_EQUIP_KIND_LEFTFINGER,
    SAVE_EQUIP_KIND_BACKPACK,
    SAVE_EQUIP_KIND_END
};

// PS2: GP_SERV_EQUIP_LIST
struct GP_SERV_EQUIP_LIST
{
    uint16_t        id: 9;
    uint16_t        size: 7;
    uint16_t        sync;

    uint8_t         PropertyItemIndex;  // PS2: PropertyItemIndex
    SAVE_EQUIP_KIND EquipKind;          // PS2: EquipKind
    uint8_t         Category;           // PS2: (New; did not exist.)
    uint8_t         padding07;          // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_EQUIP_LIST`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `PropertyItemIndex`

_The index of the item within the container._

### `EquipKind`

_The equipment slot enumeration id._

**Note:** This is aligned as a single byte!

### `Category`

_The container holding the item being equipped._

### `padding07`

_Padding; unused._
