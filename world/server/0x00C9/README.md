# `GP_SERV_COMMAND_EQUIP_INSPECT`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_EQUIP_INSPECT` |
| **Client Handler**        | `RecvEquipInspect` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00C9` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the server in response to the client checking another player. This packet is used to populate the other players information. _(Jobs, level, equipment, etc.)_

## Packet Layout

This packet has 4 different responses which have different layouts. The layouts of this packet are the following:

```cpp
// PS2: SAVE_EQUIP_KIND
enum class SAVE_EQUIP_KIND : uint8_t
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

// PS2: GP_SERV_EQUIP_INSPECT_HDR
struct GP_SERV_EQUIP_INSPECT_HDR
{
    uint32_t    UniqNo;     // PS2: UniqNo
    uint16_t    ActIndex;   // PS2: ActIndex
    uint8_t     OptionFlag; // PS2: OptionFlag
};

// PS2: (Unknown Name)
struct GP_SERV_EQUIP_INSPECT_MODE0
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    GP_SERV_EQUIP_INSPECT_HDR eqHdr;    // PS2: eqHdr

    uint8_t         padding0F;          // PS2: (Unknown)
    uint16_t        ItemNo;             // PS2: (Unknown)
    SAVE_EQUIP_KIND EquipKind;          // PS2: (Unknown)
};

// PS2: GP_SERV_EQUIP_INSPECT_OPT
struct GP_SERV_EQUIP_INSPECT_MODE1
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    GP_SERV_EQUIP_INSPECT_HDR eqHdr;    // PS2: eqHdr

    uint8_t     padding0B[3];           // PS2: (New; did not exist.))
    uint16_t    ItemNo;                 // PS2: ItemNo
    uint8_t     sComLinkName[16];       // PS2: sComLinkName
    uint8_t     sComColor[2];           // PS2: (Unnamed struct of bits.)
    uint8_t     job[2];                 // PS2: job
    uint8_t     lvl[2];                 // PS2: lvl
    uint8_t     mjob;                   // PS2: mjob
    uint8_t     mlvl;                   // PS2: (New; did not exist.)
    uint8_t     mflags;                 // PS2: (New; did not exist.)
    uint8_t     padding29[3];           // PS2: (New; did not exist.)
    uint32_t    BallistaChevronCount;   // PS2: (New; did not exist.)
    uint8_t     BallistaChevronFlags;   // PS2: (New; did not exist.)
    uint8_t     padding31;              // PS2: (New; did not exist.)
    uint16_t    BallistaFlags;          // PS2: (New; did not exist.)
    uint32_t    MesNo;                  // PS2: (New; did not exist.)
    int32_t     Params[5];              // PS2: (New; did not exist.)
    uint8_t     padding4C[8];           // PS2: (New; did not exist.)
};

// PS2: GP_SERV_EQUIP_INSPECT2
struct GP_SERV_EQUIP_INSPECT_MODE2
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    GP_SERV_EQUIP_INSPECT_HDR eqHdr;    // PS2: eqHdr

    uint16_t    ItemNo[8];              // PS2: ItemNo
    uint8_t     EquipKind[8];           // PS2: EquipKind
};

// PS2: (New; did not exist.)
struct checkitem_t
{
    uint16_t        ItemNo;     // PS2: (New; did not exist.)
    SAVE_EQUIP_KIND EquipKind;  // PS2: (New; did not exist.)
    uint8_t         padding03;  // PS2: (New; did not exist.)
    uint8_t         Data[24];   // PS2: (New; did not exist.)
};

// PS2: (New; did not exist.)
struct GP_SERV_EQUIP_INSPECT_MODE3
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    GP_SERV_EQUIP_INSPECT_HDR eqHdr; // PS2: eqHdr

    uint8_t     EquipCount; // PS2: (New; did not exist.)
    checkitem_t Equip[];    // PS2: (New; did not exist.)
};
```

## Structure Fields (`GP_SERV_EQUIP_INSPECT_HDR`)

This structure is used as a sub-header within this packet. It holds general information that all packets of this type use. The `OptionFlag` value of this sub-structure is used to determine how this packet is handled.

### `UniqNo`

_The checked entity server id._

### `ActIndex`

_The checked entity target index._

### `OptionFlag`

_The packet mode._

This value is used to determine how the packet should be handled. The client has hardcoded value checks as follows:

| OptionFlag | Purpose |
| --- | --- |
| `1` | _Used to populate the general information about the checked entity._ |
| `2` | _Used to populate the checked entity equipment. (8 slots per packet.)_ |
| `3` | _Used to populate the checked entity equipment. (Count based on packet fields.)_ |
| `else` | _Used to populate an individual equipment item. (1 item per packet.)_ |

## Structure Fields (`GP_SERV_EQUIP_INSPECT_MODE0`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `eqHdr`

_The packet sub-header._

### `padding0F`

_Padding; unused._

### `ItemNo`

_The item id._

### `EquipKind`

_The slot this item is equipped into._

## Structure Fields (`GP_SERV_EQUIP_INSPECT_MODE1`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `eqHdr`

_The packet sub-header._

### `padding0B`

_Padding; unused._

### `ItemNo`

_The item id._

### `sComLinkName`

_The checked entities linkshell name._

This value holds the encoded linkshell name. The linkshell name is encoded using a 6-bit character encoder and can be rebuilt multiple ways. _(The manner in which the client does this is via its `FromCompressStr` method. This function does a handful of bit manipulation and math tricks to walk the bit values and rebuild the string.)_

### `sComColor`

_The checked entities linkshell color._

This value holds the linkshell color. The color is bitpacked into 4-bit color codes in the order of RGBA.

```cpp
struct color_t
{
    uint16_t r : 4;
    uint16_t g : 4;
    uint16_t b : 4;
    uint16_t a : 4;
};
```

While the client does read the alpha value from this packet, it does not use it when actually rendering the linkshell icon. It will always use an alpha of 128 instead. When rendering the icon, the actual color is reconstructed into a Direct3D ARGB value. The color bits are recalculated into a valid 0 to 255 color range in the following manner:

  - `r` - `0x000D0000 * r + 0x00380000`
  - `g` - `0x00000D00 * g + 0x00003800`
  - `b` - `0x0000000D * b + 0x00000038`
  - `a` - `0x80000000`

These values are all OR'd together _(`r | g | b | a`)_ to create the final color value.

### `job`

_The checked entities job ids._

These values will be 0 if the entity is anonymous. _(`/anon`)_

### `lvl`

_The checked entities job levels._

These values will be 0 if the entity is anonymous. _(`/anon`)_

### `mjob`

_The checked entities master job._

These values will be 0 if the entity is anonymous. _(`/anon`)_

### `mlvl`

_The checked entities master job level._

This value will be 0 if the entity is anonymous. _(`/anon`)_

### `mflags`

_The checked entities master job flags._

This value holds the master job flags for the checked entity. This states if the entity has unlocked mastery on their current job. When this is true, the level information displayed for the entity will be their Mastery job level (`mlvl`) value instead of their normal level (`lvl[0]`). This value currently holds two flags that are checked with this packet:

  - `0x01` - _The entity has unlocked Mastery Job leveling on their current job._
  - `0x02` - _The entity is currently capped on Mastery experience._

This value will be 0 if the entity is anonymous. _(`/anon`)_

### `padding29`

_Padding; unused._

### `BallistaChevronCount`

_The checked entities Ballista chevron count._

This value holds the entities current chevron count when in Ballista. When checking another player during Ballista, it will show their Chevron count instead of their linkshell name/icon.

This value is only used during Ballista.

### `BallistaChevronFlags`

_The checked entities Ballista chevron flags._

This value holds the entities current chevron flags. This value currently holds two flags that are checked with this packet:

  - `0x01` - _Displays the star icon next to the entities chevron count._
  - `0x02` - _If set and `MsgNo` is also set, then tells the packet handler to print the given system message._

Flag `0x02` is mainly used to print the checked entities Chevron count information to the chat log. When this is used, the `MsgNo` will be `0x03`. The `Params` values will be populated with the specific individual chevron counts to format the message.

This value is only used during Ballista.

### `padding31`

_Padding; unused._

### `BallistaFlags`

_The checked entities Ballista flags._

This value holds the entities current Ballista flags.

This value is only used during Ballista.

### `MesNo`

_The system message id to be used to print a message when checking this entity._

This value holds the system message id that will be printed if the `BallistaChevronFlags` flag `0x02` is also set. If this is true, then `Params` will be the values to populate the message formatted arguments.

### `Params`

_The system message parameters to use to format the message._

### `padding4C`

_Padding; unused._

## Structure Fields (`GP_SERV_EQUIP_INSPECT_MODE2`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ItemNo`

_The checked entities equipped item ids._

This array holds the ids to each of the items the checked entity has equipped. There is a maximum of 8 supported slots, so more than one packet may be used to fully populate the equipment slots. This array aligns to the `EquipKind` array to determine which slot an item is equipped into. If an item is not equipped in a given slot, then it will not be present in either of these arrays. _(This is only used for actually equipped/used slots.)_

### `EquipKind`

_The checked entities equipped item slots._

See `ItemNo` notes above for how this array is used.

## Structure Fields (`GP_SERV_EQUIP_INSPECT_MODE3`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `EquipCount`

_The number of `Equip` entries that are present in this packet._

### `Equip`

_The checked entities equipment information._

This array holds the currently equipped items of the checked entity. The `EquipCount` value is used to determine the number of entries stored in this array. The client will also exit out of processing this array if any of the items ids is 0. Due to packet size limitations, the server may send multiple packets of this type to fully populate the entities equipment information.

## Structure Fields (`checkitem_t`)

### `ItemNo`

_The equipped item id._

### `EquipKind`

_The equipped item slot._

### `padding03`

_Padding; unused._

### `Data`

_The item attributes._

This value holds the unique extended data of the item such as:

  - Augments
  - Charges
  - Cooldowns / Timers
  - Trials information.
  - etc.

## Additional Information

When the client checks another player _(`/check`)_, the server will use this packet to populate the information to display the other players information such as their job, level, equipment, linkshell, etc. _(If used during Ballista, it will also show the other players Chevron information.)_ To fully populate the check window information, the server will send multiple copies of this packet. Current retail will send 3 packets when checking other players, two packets using `OptionFlag` 3 to populate the players equipment, and a final packet using `OptionFlag` 1 to populate the players general information.

This packet has 4 total modes that it can function in. _(At this time, modes 0 and 2 are unknown for their purpose or usage.)_

### `OptionFlag` Mode: 0

This mode is the default/fallback mode that will be used when the `OptionFlag` value is not set to a known mode value.

When this mode is handled, the client only looks for a single equipped item entry to be updated.

_The purpose of this mode is currently unknown as the equipped item information does not include any extra data for things like augments, timers, trials, etc. which makes this unlikely to be used any longer._

### `OptionFlag` Mode: 1

This mode is used to populate the players general information, such as their jobs, levels, mastery information, Ballista information, etc. This contains everything except equipment that is used with the inspection system.

### `OptionFlag` Mode: 2

This mode is used to populate a players equipment. When this mode is used, it will loop through the `ItemNo` array until it either hits the maximum count of 8 items, or reaches an `ItemNo` that is 0.

The server does not send information for empty equipment slots, only slots that have items will have an entry sent in this packet!

_The purpose of this mode is currently unknown as the equipped item information does not include any extra data for things like augments, timers, trials, etc. which makes this unlikely to be used any longer._

### `OptionFlag` Mode: 3

This mode is used to populate a players equipment. When this mode is used, the client will use the `EquipCount` value to begin looping through the `Equip` items to populate the equipment slots. If an entry contains an `ItemNo` of 0, then the client will exit the loop early, regardless if there is more potential items in the array. The server will send a maximum of 8 equipped items per packet.

The server does not send information for empty equipment slots, only slots that have items will have an entry sent in this packet!
