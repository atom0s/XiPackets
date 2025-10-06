# `GP_CLI_COMMAND_COMBINE_ASK`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_COMBINE_ASK` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0096` |
| **Size**                  | `0x0022` |

## Description

This packet is sent by the client when requesting to synthesize an item.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: _GP_COMBINE_ASK
struct _GP_COMBINE_ASK
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     HashNo;     // PS2: HashNo
    uint8_t     padding00;  // PS2: (New; did not exist.)
    uint16_t    Crystal;    // PS2: Crystal
    uint8_t     CrystalIdx; // PS2: CrystalIdx
    uint8_t     Items;      // PS2: Items
    uint16_t    ItemNo[8];  // PS2: ItemNo
    uint8_t     TableNo[8]; // PS2: TableNo
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`_GP_COMBINE_ASK`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `HashNo`

_The calculated hash of the packet information to prevent and detect tampering._

### `padding00`

_Padding; unused._

### `Crystal`

_The crystal item id being used for the synth._

### `CrystalIdx`

_The item index within the players inventory that the crystal is located at._

### `Items`

_The total count of items that are being used with the synthesis._

### `ItemNo`

_The array of item ids of the synth ingredients._

### `TableNo`

_The array of item indexes where the synth ingredients are located in the players inventory._

## Additional Information

The packet is sent by the client when requesting to synthesize an item. The data in this packet is populated in a special manner as the way items are used in this packet differ than others. The game limits the number of materials that can be included in a craft to 8. This includes when selecting multiple items from the same stack. Instead of a single slot being able to actually hold multiple items, the game flattens the selected materials into the 8 total slots available.

This means if the client selects 4 items from the same stack of items into a single slot of the craft window, when confirming the synth the client will spread the 4 items into separate slots, thus taking up 4 of the total 8 ingredients of the synth. _(The selected crystal does not count towards an ingredient slot.)_ If the client attempts to add a total of 9 or more items, the client will reject the synth attempt stating `Too many materials.` due to how this flattening works.

## Additional Information - Packet Hash (`HashNo`)

When the client sends this packet, it will first generate a hash value that is stored inside of `HashNo` field that is based on part of this packets contents. This hashing method is very 'light' and not any kind of real protection. Instead, it only uses part of the data of the packet after doing some basic operations to the data. First, the client will reorder the data that is stored inside of `ItemNo` and `TableNo`. The items will be reordered with the lowest item id first and to ensure all items are taking up the initial slots of the array instead of being in random slots.

Once the reordering happens, then the final part of the hashing will take place. The following is the remaining of the hashing:

```cpp
//
// Snipped code:
//
// Sort the packets ItemNo and TableNo information:
//  - Sort the item ids into the lowest array indexes.
//  - Sort the item ids into the lowest to highest order.
//

// Adjust the crystal value..
auto crystal = 0;
if (pData->Crystal < 6506 || pData->Crystal >= 6514)
{
    if (pData->Crystal < 4238 || pData->Crystal >= 4246)
        crystal = pData->Crystal;
    else
        crystal = pData->Crystal - 142;
}
else
{
    crystal = pData->Crystal - 2410;
}

// Calculate the hash..
pData->HashNo = (crystal + 3) * (pData->ItemNo[0] + 7) * (pData->Items + 5) % 0x7F;
```
