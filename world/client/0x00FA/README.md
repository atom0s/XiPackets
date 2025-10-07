# `GP_CLI_COMMAND_MYROOM_LAYOUT`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_MYROOM_LAYOUT` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00FA` |
| **Size**                  | `0x0010` |

## Description

This packet is sent by the client when rearranging furniture in their mog house. _(The client will also send this packet when exiting the Layout menu.)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_MYROOM_LAYOUT
struct GP_CLI_MYROOM_LAYOUT
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    MyroomItemNo;   // PS2: MyroomItemNo
    uint8_t     MyroomItemIndex;// PS2: MyroomItemIndex
    uint8_t     MyroomCategory; // PS2: (New; did not exist.)
    uint8_t     MyroomFloorFlg; // PS2: (New; did not exist.)
    uint8_t     x;              // PS2: x
    uint8_t     y;              // PS2: y
    uint8_t     z;              // PS2: z
    uint8_t     v;              // PS2: v
    uint8_t     padding0D[3];   // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_MYROOM_LAYOUT`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `MyroomItemNo`

_The item id of the furnishing being modified._

### `MyroomItemIndex`

_The index within the container that holds the furnishing being modified._

### `MyroomCategory`

_The item container that holds the furnishing being modified._

### `MyroomFloorFlg`

_Flag set if the furnishing is being placed on the 2nd floor._

### `x`

_The X grid position where the furnishing is being placed._

### `y`

_The Y grid position where the furnishing is being placed._

### `z`

_The Z grid position where the furnishing is being placed._

### `v`

_The rotation of the furnishing is being placed._

### `padding00`

_Padding; unused._

## Additional Information

This packet is sent by the client when rearranging the furnishings in their mog house. It will also be sent when exiting the Layout menu. When the client exits the Layout menu, informing the server they have completed their rearranging, all of the fields in this packet will be set to `0`.

The manner in which the mog house handles placing items will depend on the kind of item being placed. The client can place items in several different manners:

  - Placed directly on the floor.
  - Placed on top of another item. _(ie. tables)_
  - Hung on the wall.

Along with these placement options, there is also two separate floors that can be accessed within the mog house once unlocked. Each floor of the mog house offers varying placement space that the client can use to furnish their mog house. The first floor of the mog house is more restrictive in that there is less floor and wall space when compared to the second floor.

The grid layout system is also rotated off-axis by -90 degrees, much like most of FFXI's rotation handling. This means that the grids starting cell of `(0, 0)` is actually in the bottom-left corner of the mog house when facing north. The ending cell is then in the top-right corner. The wall hanging layout however differs based on the floor the player is placing items. See the below floor breakdowns for more information.

## Additional Information - Mog House Floor 1

The first floor of the mog house offers the least amount of usable space for both the floors and walls.

  - **Floor:** `19 x 23` grid with an `8 x 18` unusable rectangle from the door to the center of the room.
  - **Walls:** `5` usable wall slots.

The first floor has a large `8 x 18` rectangle in front of the door, including the area where the permanent rug is located, which is completely unusable. The walls on the first floor of the mog house are also restricted in that you can only store `1` item per wall with a total of `5` walls being usable. _(There are four other wall spaces on the first floor that are not usable at all.)_

The first floor wall spaces are offset differently than the second floors as well. Please view the below diagram for an example of how the wall placements work.

```
Mog House: First Floor Wall Space

      (X)      (X)     (X)
    +------+--------+------+
    |                      |
  4 |                      | 0
    |                      |
    +          (M)         +
    |                      |
  3 |                      | (X)
    |                      |
    +                      +
    |                      |
  2 |                      | 1
    |                      |
    +------+--]  [--+------+
      (X)    (Door)    (X)
```

  - _**Note:** Numbers indicate the index of the wall when placing an item on it. (Each available wall can only hold 1 item.)_
  - _**Note:** `(Door)` indicates the door to exit the mog house. (This space is also unusable.)_
  - _**Note:** `(M)` indicates the moogle location._
  - _**Note:** `(X)` indicates an unusable wall surface._

## Additional Information - Mog House Floor 2

The second floor of the mog house, when unlocked, offers much more space than the first floor.

  - **Floor:** `19 x 25` grid with no restrictions.
  - **Walls:** `92` usable wall slots.

Unlike the first floor, the second floor of the mog house does not have any kind of restrictions on the available floor space. The client has full access to the entire available grid layout on this floor. Additionally, the second floor offers a significant amount of extra wall space. Of the `12` wall sections, `11` of them are usable on the second floor. Furthermore, each wall section on the second floor offers either 4 to 12 slots individually.

The second floor wall spaces are offset differently than the first floors as well. Please view the below diagram for an example of how the wall placements work.

```
Mog House: Second Floor Wall Space

       4 D     4 E     4 F
    +-------+------+-------+
    |                      |
 12 |                      | 12
  C |                      | I
    +                      +
    |                      |
 12 |                      | 12
  B |                      | H
    +                      +
    |                      |
 12 |                      | 12
 *A |                      | G
    +-------+-]  [-+-------+
       4 J   (Door)    4 K


Mog House: Second Floor Wall Space Slots

+---+---+---+
| 0 | 4 | 8 |
+---+---+---+
============= (Wood Divider)
+---+---+---+
| 1 | 5 | 9 |
+---+---+---+
| 2 | 6 | 10|
+---+---+---+
| 3 | 7 | 11|
+---+---+---+
```

  - _**Note:** Numbers indicate the available slots that items can be placed on the given wall._
  - _**Note:** `(Door)` indicates the door to exit the mog house. (This space is also unusable.)_
  - _**Note:** `(X)` indicates an unusable wall surface._
  - _**Note:** `*A` indicates the surface where the client begins indexing the wall items._

Using the two diagrams for the second floor above, we can better understand how the layout is indexed when placing items on the walls. First, the indexing for the second floor begins on the bottom-left hand wall. _(See the `*A` in the above diagram for which wall this refers to.)_ This wall is where the index `0` for wall hanging furnishings begins on the second floor. Using the second diagram above, we can see the order in how the wall slots are indexed. The top-left corner is treated as slot `0` with the index incrementing downward. Once the last slot in a column is reached, the index will continue at the top of the next column and repeat downwards again. The first diagram is also marked with letters going in order of how each wall is used when indexing.

**Please Note:** When using the right-hand side walls, the order of the indexing is flipped for walls `G`, `H`, and `I`. It mirrors the left-hand walls. See the below pictures to better understand what this means:

  - **Left Side:** [Click Me](/world/client/0x00FA/left.png)
  - **Right Side:** [Click Me](/world/client/0x00FA/right.png)
