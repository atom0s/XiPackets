# `GP_SERV_COMMAND_COMBINE_ANS`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_COMBINE_ANS` |
| **Client Handler**        | `gcRecvCombine` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x006F` |
| **Size**                  | `0x0038` |

## Description

This packet is sent by the server to inform the client of personal synthesis results.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_COMBINE_ANS
struct GP_COMBINE_ANS
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Result;         // PS2: Result
    int8_t      Grade;          // PS2: Grade
    uint8_t     Count;          // PS2: Count
    uint8_t     padding00;      // PS2: (New; did not exist.)
    uint16_t    ItemNo;         // PS2: ItemNo
    uint16_t    BreakNo[8];     // PS2: BreakNo
    int8_t      UpKind[4];      // PS2: UpKind
    int8_t      UpLevel[4];     // PS2: UpLevel
    uint16_t    CrystalNo;      // PS2: (New; did not exist.)
    uint16_t    MaterialNo[8];  // PS2: (New; did not exist.)
    uint32_t    padding01;      // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_COMBINE_ANS`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Result`

_The craft result id._

This value determines the result of the craft attempt.

| Result | Type | Meaning |
| --- | --- | --- |
| `0x00` | _Success._   | _Successful synthesis; displays the item information sub-window._ |
| `0x01` | _Fail._      | _Synthesis failed. You lost the crystal you were using._ |
| `0x02` | _Fail._      | _Synthesis interrupted. You lost the crystal and materials you were using._ |
| `0x03` | _Fail._      | _Synthesis canceled. That combination of materials cannot be synthesized._ |
| `0x04` | _Fail._      | _Synthesis canceled._ |
| `0x06` | _Fail._      | _Synthesis canceled. That formula is beyond your current craft skill level._ |
| `0x07` | _Fail._      | _Synthesis canceled. You cannot hold more than one item of that type._ |
| `0x0C` | _Success._   | _Successful synthesis; displays the item information sub-window._ |
| `0x0D` | _Fail._      | _You must wait longer before repeating that action._ |
| `0x0E` | _Fail._      | _Synthesis interrupted. You lost the crystal and materials you were using._ |

_**Note:** All other result ids are treated as a failed synth and will print the lost crystal message._

### `Grade`

_The synthesis grade._

This value determines the level of success for a synthesis, but is used as the message lookup id to pull the proper message from the DAT file to be printed to the chatlog. The base message starts at id `160`, however the packet value will be adjusted by `160` to allow the value to start from `0`.

| Grade | Message |
| --- | --- |
| `0xFF` | _Used when a synthesis has failed._ |
| `0x00` | `You synthesized [a/{num}] [item_name][puncuation].` |
| `0x01` | `You synthesized [a/{num}] [item_name][puncuation].` |
| `0x02` | `You synthesized [a/{num}] [item_name][puncuation].` |
| `0x03` | `You synthesized [a/{num}] [item_name][puncuation].` |
| `0x04` | `A [item_name] was lost.` |

_**Note:** These messages are passed through the client message parser and will parse out special code tags to properly format the message!_

The messages above are stored in the following DAT files:

  - JP: `ROM/27/75.DAT` (File Id: `7030`)
  - NA: `ROM/27/76.DAT` (File Id: `7031`)

### `Count`

_The count of items created by the synthesis._

This value will generally be `1` for any failed or successful synthesis unless the result actually yields more than 1 item. _(ie. crafting stacks of items.)_

### `padding00`

_Padding; unused._

### `ItemNo`

_The item id of the created item from the synthesis._

This value will be set to `29695` _(`Mangled Mess`)_ when a synthesis fails resulting in the loss of crystal/items.

### `BreakNo`

_The item ids for any lost items during the synthesis._

This array holds the item ids for any lost materials during the synthesis. The values stored in this array are aligned to the materials array the client originally sent for the original synthesis request. For example, if the client sends a request that fills in all 8 material slots and the last slots item was lost in the synthesis attempt, then the last item in this array will be set to that item id.

### `UpKind`

_The skill ids for any skills that gained a skillup during the synthesis._

This array holds the skill ids used within the synthesis and if any of those skills received a skillup. _(Stored in the `UpLevel` array.)_ If no skillup was obtained during the synthesis, then the first entry in this array will be set to the main craft skill used for the synthesis.

### `UpLevel`

_The skill value increases for any skillups._

This array holds the skillup values for any crafting skill that gained a skillup during the synthesis attempt. The values in this array are aligned to the skill ids in the `UpKind` array.

These values are stored as whole numbers, but are treated as decimals. The values will be multiplied by `0.1` before being used. _(This means that a value of `1` is equal to `0.1`.)_

### `CrystalNo`

_The item id of the crystal used for the synthesis._

### `MaterialNo`

_The item ids of the materials used for the synthesis._

This array holds the materials used for the synthesis.

Crafting recipes can accept a maximum of 8 items total. This includes any attempted stacks of items, it treats each item in a stack as an individual part of the recipe. When the client submits a synthesis request, it will reduce all items in the recipe window down to a single item per slot. Even if the client puts two of the same item into one slot, the client will separate the stack and put each item into its own slot.

### `padding01`

_Padding; unused._
