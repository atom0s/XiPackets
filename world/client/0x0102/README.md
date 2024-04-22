# `Packet: 0x0102`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0102` |
| **Size**                  | `0x00A4` |

## Description

This packet is sent by the client when interacting with an extended job-specific system. This is used for things such as:

  - Editing equipped Blue Mage spells.
  - Editing equipped Automaton attachments.
  - Editing equipped Monstrosity instincts.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Data[160];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Data`

_The packet data._

The data held in this array will vary based on the system the client is interacting with.

## Additional Information - Blue Mage (Spell Changes)

This packet is used by the client when changing equipped blue magic spells while playing Blue Mage. The `Data` field of this packet follows this layout:

```cpp
struct data_t
{
    uint8_t SpellId;
    uint8_t unknown00;
    uint8_t padding00[2];
    uint8_t JobIndex;
    uint8_t SupportJobFlg;
    uint8_t padding01[2];
    uint8_t Spells[20];
    uint8_t unused00[132];
};
```

### `SpellId`

_The id of the blue magic spell._

When the client is equipping a spell, this value will be set to the normalized spell id. Spells are normalized by subtracting `512` from their true value before being written to this packet. _(ie. The spell `Venom Shell` is actually id `513`, however when written to this packet it will be `1`.)_ When unequipping a spell, this value will be set to `0`.

The client makes use of the `Spells` array to determine which slot(s) are being modified.

### `unknown00`

_Unknown._

The client always sets this value to `0`.

### `padding00`

_Padding; unused._

### `JobIndex`

_The job index that this packet is being generated for._

This value is set to the job index for the job the packet is related for. For example, when the client is editing their equipped blue magic spells, this value will be set to `16`. It does not matter if the job is set to the clients main or support job, this packet will use the `SupportJobFlg` to denote that.

### `SupportJobFlg`

_Flag set if the `JobIndex` value is currently the clients sub-job._

  - `0` - _The client has the `JobIndex` as their main job._
  - `1` - _The client has the `JobIndex` as their support job._

### `padding01`

_Padding; unused._

### `Spells`

_The array of spell slots._

This array is used to determine which slot is being modified by the client when equipping or unequipping blue magic spells. When the client is changing a spell slot, the slot that is being modified will have its value set to the spell id that is either being equipped or unequipped. _(When unequipping, the packets `SpellId` value will be `0`, but this arrays value for the given slot will still be the spell id value.)_

When unequipping all currently equipped spells, this packets `SpellId` value will be set to `0` and each blue magic slot that has a spell currently equipped will have its `Spells` array entry set to the equipped spell id.

### `unused00`

_Unused._

## Additional Information - Puppet Master (Automaton Equipment Changes)

This packet is used by the client when changing equipped automaton attachments while playing Puppet Master. The `Data` field of this packet follows this layout:

```cpp
struct data_t
{
    uint8_t ItemId;
    uint8_t unknown00;
    uint8_t padding00[2];
    uint8_t JobIndex;
    uint8_t SupportJobFlg;
    uint8_t padding01[2];
    uint8_t Slots[14];
    uint8_t unused00[138];
};
```

### `ItemId`

_The id of the Automaton item._

When the client is equipping an Automaton item _(head, frame or attachment)_, this value will be set to the normalized item id of the given slot kind.

  - `Head` - _Normalized by subtracting `8192` from the item id._
  - `Frame` - _Normalized by subtracting `8192` from the item id._
  - `Attachment` - _Normalized by subtracting `8448` from the item id._

When unequipping an item, this value will be set to `0`.

The client makes use of the `Slots` array to determine which slot(s) are being modified.

### `unknown00`

_Unknown._

The client always sets this value to `0`.

### `padding00`

_Padding; unused._

### `JobIndex`

_The job index that this packet is being generated for._

This value is set to the job index for the job the packet is related for. For example, when the client is editing their Automatons equipment, this value will be set to `18`. It does not matter if the job is set to the clients main or support job, this packet will use the `SupportJobFlg` to denote that.

### `SupportJobFlg`

_Flag set if the `JobIndex` value is currently the clients sub-job._

  - `0` - _The client has the `JobIndex` as their main job._
  - `1` - _The client has the `JobIndex` as their support job._

### `padding01`

_Padding; unused._

### `Slots`

_The array of Automaton equipment slots._

This array is used to determine which slot is being modified by the client when equipping or unequipping Automaton equipment. When the client is changing an equipment slot, the slot that is being modified will have its value set to the item id that is being equipped or unequipped. _(When unequipping, the packets `ItemId` value will be `0`, but this arrays value for the given slot will still be the equipment id value.)_

The slots are in the following order:

| Index | Usage |
| --- | --- |
| `0x00` | _Head_ |
| `0x01` | _Frame_ |
| `0x02` | _Attachment (1)_ |
| `0x03` | _Attachment (2)_ |
| `0x04` | _Attachment (3)_ |
| `0x05` | _Attachment (4)_ |
| `0x06` | _Attachment (5)_ |
| `0x07` | _Attachment (6)_ |
| `0x08` | _Attachment (7)_ |
| `0x09` | _Attachment (8)_ |
| `0x0A` | _Attachment (9)_ |
| `0x0B` | _Attachment (10)_ |
| `0x0C` | _Attachment (11)_ |
| `0x0D` | _Attachment (12)_ |

_**Note:** The attachment slots are ordered left to right starting from the top row._

When unequipping all equipment on an Automaton, this packets `ItemId` value will be set to `0` and each equipment slot on the Automaton that has something currently equipped will have its `Slots` array entry set to the equipment id.

### `unused00`

_Unused._

## Additional Information - Monstrosity (Equipment Changes)

_Todo._
