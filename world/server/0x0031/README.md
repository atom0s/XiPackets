# `GP_SERV_COMMAND_RECIPE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_RECIPE` |
| **Client Handler**        | `RecvRecipe` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0031` |
| **Size**                  | `0x0034` |

## Description

This packet is sent by the server to inform the client of synthesis related recipes.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_RECIPE
struct GP_SERV_RECIPE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Data[40];
    uint16_t    Type;
    uint16_t    Unknown00;
};
```

_The original PS2 beta version of this packet only accounted for a single type. See the notes below for how this packet is used with the different types._

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_RECIPE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `Data`

_The recipe information._

The data stored within this value will change based on the packet `Type`.

### `Type`

_The packet type._

### `Unknown00`

_Unknown._

## Additional Information

This packet is used for multiple purposes when talking to a synthesis related NPC. Players can request both a list of available recipes for a given craft rank and individual recipes for the items listed. The following `Type` values are handled by the client:

  - `1`, `3` - _Used to return an individual recipes information._
  - `2` - _Used to return a recipe list._
  - `4` - _Unknown._

The `Data` information of the packet will change based on the `Type` of packet being sent.

## Mode 1 & 3 - Recipe Request

These modes are used when the player requests an individual recipes information. The response from the server will be a packet containing the recipes resulting item id, needed materials to craft the item, along with sub-craft requirements.

The packet layout when this mode is sent is as follows:

```cpp
struct GP_SERV_RECIPE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    productitem;    // PS2: productitem
    uint16_t    need_skill_1;   // PS2: need_skill_1
    uint16_t    need_skill_2;   // PS2: need_skill_2
    uint16_t    need_skill_3;   // PS2: need_skill_3
    uint16_t    need_item;      // PS2: need_item
    uint16_t    need_key_item;  // PS2: (New; did not exist.)
    uint16_t    itemnum[8];     // PS2: itemnum
    uint16_t    itemcount[8];   // PS2: itemcount
    uint16_t    Type;           // PS2: (New; did not exist.)
    uint16_t    Unknown00;      // PS2: (New; did not exist.)
};
```

### `productitem`

_The resulting item id that will be made with this recipe._

### `need_skill_1`

_The needed sub-craft to make this recipe. (1)_

This value will be 0 if this sub-craft is not needed.

### `need_skill_2`

_The needed sub-craft to make this recipe. (2)_

This value will be 0 if this sub-craft is not needed.

### `need_skill_3`

_The needed sub-craft to make this recipe. (3)_

This value will be 0 if this sub-craft is not needed.

### `need_item`

_The needed crystal item id to make this recipe._

### `need_key_item`

_The needed key item id to make this recipe._

This value will be 0 if there is no key item requirement.

### `itemnum`

_An array of item ids that represent the required ingrediants to make this recipe._

### `itemcount`

_An array of item counts that represent the required ingrediants to make this recipe._

### `Type`

_The packet type._

### `Unknown00`

_Unknown._

## Mode 2 - Recipe List Request

This mode is used when the player requests a list of recipes for a given crafting rank. The response from the server will be a packet containing a list of item ids that can be crafted. If an additional page of content is available, the last entry will be set to the first id of the item on the next page.

The packet layout when this mode is sent is as follows:

```cpp
struct GP_SERV_RECIPE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    junk00[6];      // PS2: (New; did not exist.)
    uint16_t    itemnum[16];    // PS2: (New; did not exist.)
    uint16_t    Type;           // PS2: (New; did not exist.)
    uint16_t    itemnum_next;   // PS2: (New; did not exist.)
};
```

### `junk00`

_Junk data; the client does not use this and the buffer may contain unrelated data._

### `itemnum`

_The item id of the crafted item of the recipe._

This array holds the item id of the item that can be crafted, which is displayed in the menu to select to see the full recipe for.

### `Type`

_The packet type._

### `itemnum_next`

_The item id of the next pages first item; if another page of items can be displayed._

This value is used to tell the client there are multiple pages of recipes to display. If this value is set, the client will display the `View more recipes.` menu option.

## Mode 4 - Unknown

_The purpose of this mode is unknown at this time. However, the handler appears to be implemented in the same manner as the original packet from the PS2 beta. This may be a left-over mode to allow restoring original functionality if it were ever needed._
