# `GP_SERV_COMMAND_ITEM_MAX`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_ITEM_MAX` |
| **Client Handler**        | `RecvItemMax` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x001C` |
| **Size**                  | `0x0064` |

## Description

This packet is sent by the server to inform the client of the characters general job information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_ITEM_MAX
struct GP_SERV_ITEM_MAX
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     ItemNum[18];    // PS2: ItemNum
    uint8_t     padding00[14];  // PS2: (New; did not exist.)
    uint16_t    ItemNum2[18];   // PS2: (New; did not exist.)
    uint8_t     padding01[28];  // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_ITEM_MAX`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `ItemNum`

_The characters various inventory container sizes._

This array holds the maximum space of a container. The client uses the values from this array to determine how large containers are and how large to loop over items within a container. This is handled by the game client function `gcItemMaxSpaceGet` which will return the value for the given container index that is stored in this array.

Containers hold `81` items total, however the first item of every container is hidden and used to represent the characters gil. Because of this, the client will expect a container to have `> 1` size in order to be valid to hold items and have space.

If a container is set to `0`, it will be considered locked.

### `padding00`

_Padding; unused._

### `ItemNum2`

_The characters various inventory container sizes._

This array holds the available space of a container, however the client does not use this array much at all. It is only used to check specific containers and with specific conditions. _(See additional notes below.)_

### `padding01`

_Padding; unused._

## Additional Notes

The manner in which the client uses both `ItemNum` and `ItemNum2` is a bit hacky and only for certain conditions. Not all containers rely on either value and some containers have special cases in how these values are used.

_**Only the Storage and Mog Locker containers make use of the `ItemNum2` values locally.**_

### Storage (Idx: 2)

The `ItemNum` value is used to determine if the container can have items moved into it or not as well as to display the total available item slots in the container. If the value is `<= 1`, then the container is not available for items and the inventory item selector will be greyed out.

The `ItemNum2` value is used when the player is manipulating their mog house furniture layout. When the player opens the `Layout` menu and then selects a piece of furniture, the client will calculate the space left available if they were to remove the item from placement. It does this calculation using the value within `ItemNum2`. If removing the item would cause the players storage to not have enough space to hold their current items, then the sub-menu `Remove` button will be greyed out and unselectable.

### Mog Locker (Idx: 4)

The `ItemNum` value is used to determine if the container can have items moved into it or not as well as to display the total available item slots in the container. If the value is `<= 1`, then the container is not available for items and the inventory item selector will be greyed out.

The `ItemNum2` value is used to determine if the locker is accessible. The client checks if this value is set to `0` which means the locker is not accessible. Otherwise, any value `>= 1` is considered accessible. When this value is `0` the locker contents will be greyed out, the side-by-side inventory selector will be hidden and the client will display an error message saying `You do not current have access to a Mog Locker.`.

## Container Access

When determining which containers the player has access to, multiple values are used. What is used to check these depends on the context, but in most cases follows the below table.

| Index | Container | Details |
| ---: | --- | --- |
| `0` | `Inventory`     | `Always accessible.` |
| `1` | `Mog Safe`      | `!MogCheck()` |
| `2` | `Storage`       | `Always accessible.` |
| `3` | `Temporary`     | `N/A (Zone Dependent)` |
| `4` | `Locker`        | `Always accessible.`<br>`Locked based on: pGlobalNowZone->ItemSys.ItemNum2[4] == 0` |
| `5` | `Satchel`       | `pGlobalNowZone->ItemSys.ItemNum[5] >= 30u` |
| `6` | `Sack`          | `!gcItemMaxSpaceGet(6)` |
| `7` | `Case`          | `Always accessible.` |
| `8` | `Wardrobe 1`    | `Always accessible.` |
| `9` | `Mog Safe 2`    | `pGlobalNowZone->mog_exit_flags & 1` |
| `10` | `Wardrobe 2`   | - `Always accessible.`<br>- `!gcItemMaxSpaceGet(10)` |
| `11` | `Wardrobe 3`   | - `(g_pNtSys->SrvExCode2 >> 2) & 1`<br>- `!gcItemMaxSpaceGet(11)` |
| `12` | `Wardrobe 4`   | - `(g_pNtSys->SrvExCode2 >> 3) & 1`<br>- `!gcItemMaxSpaceGet(12)` |
| `13` | `Wardrobe 5`   | - `(g_pNtSys->SrvExCode2 >> 4) & 1`<br>- `!gcItemMaxSpaceGet(13)` |
| `14` | `Wardrobe 6`   | - `(g_pNtSys->SrvExCode2 >> 5) & 1`<br>- `!gcItemMaxSpaceGet(14)` |
| `15` | `Wardrobe 7`   | - `(g_pNtSys->SrvExCode2 >> 6) & 1`<br>- `!gcItemMaxSpaceGet(15)` |
| `16` | `Wardrobe 8`   | - `(g_pNtSys->SrvExCode2 >> 7) & 1`<br>- `!gcItemMaxSpaceGet(16)` |
| `17` | `Recycle`      | `Always accessible.` |
