# `Packet: 0x00D8`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00D8` |
| **Size**                  | `0x0028` |

## Description

This packet is sent by the client when interacting with or changing parameters related to a private dungeon system, such as the `Moblin Maze Mongers` tablet item.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    ActIndex;
    uint16_t    Param1;
    uint8_t     Param2;
    uint8_t     padding00[3];
    uint32_t    UniqueNo;
    uint8_t     Data[24];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ActIndex`

_The clients target index._

### `Param1`

_The packet parameter. (1)_

This value is populated from an event VM script.

### `Param2`

_The packet parameter. (2)_

This value is populated from an event VM script.

### `padding00`

_Padding; unused._

### `UniqueNo`

_The clients server id._

### `Data`

_The packet bit data._

This array holds the various bit packed data related to the dungeon information being updated.

## Additional Information

This packet is used when the client is manipulating a private dungeon related item. This is mainly used with the `Moblin Maze Mongers` content.

_**Note:** There is much more work needed to fully reverse this packet. Due to the nature of content it is involved with; it is considered low priority currently._
