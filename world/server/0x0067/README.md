# `Unknown - Packet: 0x0067`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0067` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the server to update entity information.

## Packet Layout

The initial layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    Mode : 6;
    uint16_t    Length : 10;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Mode`

_The packet mode._

This value is used to determine how the data within this packet should be handled. It is used in combination with the `Length` value. _(For validation.)_

The client currently will handle the following modes:

| Mode | Size | Purpose |
| --- | --- | --- |
| `2` | `>= 36` | _Updates a player entity._ |
| `3` | `>= 20` | _Updates an NPC entity._ |
| `4` | `>= 24` | _Updates the local client pet information._ |

### `Length`

_The packet content length._

This value is used to determine the size of the packets content, minus the header. _(It includes the `Mode` and `Length` in this size!)_

## Mode: `2`

This mode is used when the packet is intended to update a player entity. The contents of the packet will pertain to an existing PC entity and is used to update mainly the entities name flags for things such as Ballista, Campaign, Pankration, and similar other events.

The packet layout when this mode is sent is as follows:

```cpp
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    Mode: 6;
    uint16_t    Length: 10;
    uint16_t    ActIndex;
    uint32_t    UniqueNo;
    uint16_t    ActIndexFellow;
    uint16_t    padding00;
    uint32_t    NameFlags;
    uint32_t    NameIcon;
    uint32_t    CustomProperties;
    uint32_t    unknown1C;
    uint32_t    UniqueNoMog;
    uint8_t     MogHouseFlag;
    uint8_t     mjob_lv;
    uint8_t     unknown26;
    uint8_t     MogExpansionFlag;
};
```

### `ActIndex`

_The entity target index._

### `UniqueNo`

_The entity server id._

### `ActIndexFellow`

_The target index of the entities fellow NPC._

### `padding00`

_Padding; unused._

### `NameFlags`

_The entity name flags data._

This value is used to hold multiple pieces of information.

  - `PankrationEnabled` - `(pkt->NameFlags >> 21) & 0xF`
  - `PankrationFlagFlip` - `(pkt->NameFlags >> 25) & 0xF`
  - `CampaignNameFlag` - `(pkt->NameFlags & 2) != 0`

This value is also used to set a bit in the entities render flags:

```cpp
entity->Render.Flags4 ^= (entity->Render.Flags4 ^ ((uint8_t)(pkt->NameFlags >> 2) << 23)) & 0x800000;
```

### `NameIcon`

_The entity name icon data._

This value is used to set the entities nameplate icon replacement id. It is only used when the entity is within a pvp related event under certain conditions. It will check a sub-value that is part of the entities `BallistaInfo` data and is also used with the `BallistaFlags` value to determine its usage. Depending on these values, the `NameIcon` value will be used in one of the two following manners:

  - `2 * (pkt->NameIcon != 0) - 0x5D`
  - `(pkt->NameIcon != 0) - 0x5D`

This will cause a red stone icon to appear in-place of the entities normal name icon. _(ie. Linkshell, Bazaar, etc.)_

_This return value is used as a `int8_t` so negative numbers will wrap!_

### `CustomProperties`

_The entity custom properties data._

This value is only used if the packet is updating the local client.

This value is used to adjust custom properties for the entity. This is generally used for the model information related to the entities Chocobo.

### `unknown1C`

_Unknown. (Unused; potentially padding.)_

This value is only used if the packet is updating the local client.

### `UniqueNoMog`

_The server id of the entity whos moghouse is being visited._

This value is only used if the packet is updating the local client.

This value is used to set the server id of the entity whos mog house is being visited. _(This is used with the 'Open Moghouse' feature.)_

### `MogHouseFlag`

_Flag that is used to determine if the entities mog house is open/closed._

This value is only used if the packet is updating the local client.

This value is used when the client has opened their mog house to other party members. Any non-zero value will be treated as if the clients mog house is open to other players. When this is enabled, the client will disable the `Layout` and `Remodel` buttons from working in the mog menu.

### `mjob_lv`

_The entities main job level._

This value is only used if the packet is updating the local client.

The clients main job level. _(This value may not always be set.)_

### `unknown26`

_Unknown._

This value is only used if the packet is updating the local client.

The purpose of this value is unknown. The client will set this value locally within the global zone state, but its value is never read/used afterward.

### `MogExpansionFlag`

_The entities mog house expansion flags._

This value is only used if the packet is updating the local client.

This value is used to determine if the client has completed the mini-quests to expand their mog house. When those mini-quests are complete, then the client can zone into their home nation mog house and automatically unlock the following additional features:

  - Mog House 2nd Floor
  - Mog Safe 2
  - Exit Mog House To Any Connecting City Zone

## Mode: `3`

This mode is used when the packet is intended to update an NPC entity. It works similar to how `Mode` 2 works with the removal of a few extra bits, mainly for updating the local client related information. This mode is mainly used for updating Trust NPCs.

```cpp
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    Mode: 6;
    uint16_t    Length: 10;
    uint16_t    ActIndex;
    uint32_t    UniqueNo;
    uint16_t    ActIndexTrustOwner;
    uint16_t    padding00;
    uint32_t    NameFlags;
    char        Name[];
};
```

### `ActIndex`

_The entity target index._

### `UniqueNo`

_The entity server id._

### `ActIndexTrustOwner`

_The owner entity target index of this entity._

This value is only used when the entity is a 'pet' or 'trust'. Otherwise, it will be 0.

### `padding00`

_Padding; unused._

### `NameFlags`

This value is used to hold multiple pieces of information.

  - `PankrationEnabled` - `HIBYTE(pkt->NameFlags) & 0xF`
  - `PankrationFlagFlip` - `(pkt->NameFlags >> 28) & 0xF`
  - `CampaignNameFlag` - `(pkt->NameFlags & 2) != 0`

This value is also used to set a bit in the entities render flags:

```cpp
entity->Render.Flags4 ^= (entity->Render.Flags4 ^ ((uint8_t)(pkt->NameFlags >> 2) << 22)) & 0x400000;
```

### `Name`

_The entity name._

This value is only used if the entity name is being overridden. This is generally only used for non-static zone entities that may need their name manually set/updated. The `Length` value of the packet will vary based on if this value is present. The client will check for the first byte being larger than the ASCII space character value (`0x20`) to determine if a name is set in the packet. The client will then assume and attempt to directly copy 24 bytes from the `Name` buffer without any kind of string length check. _(The packet will be 4 byte aligned and the string will be null-terminated when the name is present.)_

## Mode: `4`

This mode is used when the packet is intended to update the local clients personal pet. This is used for summoned pets such as Beast Master pets _(charmed and summoned jugs)_, Summoner avatars, etc. _(This is not used for pet-like entities such as Trusts or Adventuring Fellows!)_

The client handles this mode by simply copying the first 0x18 bytes of the packet _(starting at the `Mode`)_ into a local buffer that holds the clients pet information. This buffer does not care about the full pet name and will potentially chop the name in half when copying the buffer. The client never uses the name from this packet, mode and instead will always pull it from the entity entry for the pet if needed.

The packet layout when this mode is sent is as follows:

```cpp
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    Mode: 6;
    uint16_t    Length: 10;
    uint16_t    ActIndex;
    uint32_t    UniqueNo;
    uint16_t    ActIndexOwner;
    uint8_t     Hpp;
    uint8_t     Mpp;
    uint32_t    Tp;
    uint32_t    UniqueNoTarget;
    char        Name[];
};
```

### `ActIndex`

_The pet entity target index._

### `UniqueNo`

_The pet entity server id._

### `ActIndexOwner`

_The target index of the pet owner._

### `Hpp`

_The pet health percent._

### `Mpp`

_The pet mana percent._

### `Tp`

_The pet TP._

### `UniqueNoTarget`

_The server id of the entity the pet is targeting._

### `Name`

_The pet name._
