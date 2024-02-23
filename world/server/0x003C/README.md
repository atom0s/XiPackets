# `GP_SERV_COMMAND_SHOP_LIST`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_SHOP_LIST` |
| **Client Handler**        | `RecvShopList` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x003C` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the server to inform the client of a shops items.

## Packet Layout

The layout of this packet is the following:

```cpp
struct GP_SHOP
{
    uint32_t    ItemPrice;  // PS2: ItemPrice
    uint16_t    ItemNo;     // PS2: ItemNo
    uint8_t     ShopIndex;  // PS2: ShopIndex
    uint8_t     padding00;  // PS2: Dammy
    uint16_t    Skill;      // PS2: (New; did not exist.)
    uint16_t    GuildInfo;  // PS2: (New; did not exist.)
};

// PS2: GP_SERV_SHOP_LIST
struct GP_SERV_SHOP_LIST
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    ShopItemOffsetIndex;// PS2: ShopItemOffsetIndex
    uint8_t     Flags;              // PS2: dammy
    uint8_t     padding00;          // PS2: Dammy
    GP_SHOP     ShopItemTbl[];      // PS2: ShopItemTbl
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_SHOP_LIST`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ShopItemOffsetIndex`

_The starting offset of where the items in this packet will be placed into the internal shop system table._

### `Flags`

_The shop flags._

The client currently only uses the first bit of this value. However, its usage only sets flags in the client that are then never used.

### `padding00`

_Padding; unused._

### `ShopItemTbl`

_The list of items._

## Structure Fields (`GP_SHOP`)

### `ItemPrice`

_The item price._

### `ItemNo`

_The item number._

### `ShopIndex`

_The shop index._

This value is not used by the client. Instead, it starts the base index from the main `ShopItemOffsetIndex` value and increments it for each item in the packet.

### `padding00`

_Padding; unused._

### `Skill`

_The item skill._

This value relates to the index of the client skill table. This will usually be set to the index of the craft skill the shop selling the item is for.

  - `48` - _Fishing_
  - `49` - _Woodworking_
  - `50` - _Smithing_
  - `51` - _Goldsmithing_
  - `52` - _Clothcraft_
  - `53` - _Leathercraft_
  - `54` - _Bonecraft_
  - `55` - _Alchemy_
  - `56` - _Cooking_
  - `57` - _Synergy_
  - `58` - _Chocobo Digging_

_The client also accepts values between `64` and `87` when using this value. When a value in this range is used, then the client will display a job name instead of the crafting rank required for the item. The `GuildInfo` value will also then be treated as a numerical value to display next to the job string._

### `GuildInfo`

_The item guild information._

This value is generally used for displaying the crafting skill rank required for the item. This value is calculated in the following manner: `pkt->GuildInfo = craft_rank * 100;`

This value can also be used as a job level requirement instead when `Skill` is set to a value between `64` and `87`.

_The following list is the base skill ranks. Their value is multiplied by 100 in the packet._

  - `0` - _Amateur_
  - `1` - _Recruit_
  - `2` - _Initiate_
  - `3` - _Novice_
  - `4` - _Apprentice_
  - `5` - _Journeyman_
  - `6` - _Craftsman_
  - `7` - _Artisan_
  - `8` - _Adept_
  - `9` - _Veteran_
  - `10` - _Expert_
  - `11` - _Authority_
  - `12` - _Luminary_
  - `13` - _Master_
  - `14` - _Grandmaster_
  - `15` - _Legend_

## Additional Information

The size of this packet will vary based on the number of items sent within the `ShopItemTbl` field. To determine the number of items that is stored in a packet, the client uses the following:

```cpp
int32_t __cdecl FUNC_enQueAddSizeGet(int32_t struct_size, int32_t packet_size)
{
    return 4 * packet_size - struct_size;
}

// Item count calculation..
const auto cnt = FUNC_enQueAddSizeGet(20, *(uint16_t*)pkt >> 0x09) / 0x0C + 1;
```
