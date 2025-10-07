# `Packet: 0x003B`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x003B` |
| **Size**                  | `0x0020` |

## Description

This packet is sent by the client when interacting with a sub-container item. _(ie. Mannequins)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    Kind;           // PS2: (New; did not exist.)
    uint32_t    Category1;      // PS2: (New; did not exist.)
    uint8_t     ItemIndex1;     // PS2: (New; did not exist.)
    uint8_t     ContainerIndex; // PS2: (New; did not exist.)
    uint16_t    padding0E;      // PS2: (New; did not exist.)
    uint32_t    Category2;      // PS2: (New; did not exist.)
    uint8_t     ItemIndex2;     // PS2: (New; did not exist.)
    uint8_t     padding15[3];   // PS2: (New; did not exist.)
    uint32_t    Category3;      // PS2: (New; did not exist.)
    uint8_t     ItemIndex3;     // PS2: (New; did not exist.)
    uint8_t     padding1D[3];   // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Kind`

_The mode of the packet._

This value is used to tell the server what action is being requested. This will be based on the sub-container being interacted with.

For mannequins, the values used are:

| Kind | Purpose |
| --- | --- |
| `1` | _Equip item._ |
| `2` | _Unequip item._ |
| `5` | _Unequip all items._ |

### `Category1`

_The container that holds the sub-container item being interacted with._

### `ItemIndex1`

_The index within the container that holds the sub-container item._

### `ContainerIndex`

_The index within the sub-container that is being interacted with._

This value will be dependent on the sub-container that is interacted with.

For mannequins, this value will represent the equipment slot being modified.

| Index | Name |
| --- | --- |
| `0x00` | _M.Weapon_ |
| `0x01` | _S.Weapon_ |
| `0x02` | _R.Weapon_ |
| `0x03` | _Head_ |
| `0x04` | _Body_ |
| `0x05` | _Hands_ |
| `0x06` | _Legs_ |
| `0x07` | _Feet_ |

When unequipping all items on a mannequin, this will be set to `0`.

### `padding0E`

_Padding; unused._

### `Category2`

_The container that holds the item that will be used with the sub-container._

When unequipping items _(individually and all)_ from a mannequin, this value will be `18`.

### `ItemIndex2`

_The index within the container that holds the item._

When unequipping items _(individually and all)_ from a mannequin, this value will be `0`.

### `padding15`

_Padding; unused._

### `Category3`

_Unknown._

This value is treated like a container. It is always set to `18`.

### `ItemIndex3`

_Unknown._

This value is treated like an item index within a container. It is always set to `0`.

### `padding1D`

_Padding; unused._
