# `Packet: 0x001B`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x001B` |
| **Size**                  | `0x84` |

## Description

This packet is sent by the server to inform the client of the characters general job information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: unknown
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     dancer[0x44];
    uint8_t     job_lev2[0x18];
    uint32_t    encumbrance;
    uint8_t     can_thumbs_up_mentor;
    uint8_t     mentor_rank;
    uint8_t     mastery_rank;
    uint8_t     padding00;
    uint32_t    job_mastery_flags;
    uint8_t     job_mastery_levels[0x18];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `dancer`

_The characters general information._

This value is a partial copy of a `GP_MYROOM_DANCER` object. The part of the structure used is as follows:

```cpp
struct GP_MYROOM_DANCER
{
    uint16_t  mon_no;
    uint16_t  face_no;
    uint8_t   mjob_no;
    uint8_t   hair_no;
    uint8_t   size;
    uint8_t   sjob_no;
    uint32_t  get_job_flag;
    int8_t    job_lev[16];
    uint16_t  bp_base[7];
    int16_t   bp_adj[7];
    int32_t   hpmax;
    int32_t   mpmax;
    uint8_t   sjobflg;
    uint8_t   Unknown00[3];

    // Rest of struct trimmed..
};
```

### `job_lev2`

_The characters job levels. (Continued.)_

This value is copied into the ending part of the `GP_MYROOM_DANCER` structure, which has a field `int8_t job_lev2[16]` that is populated with this data.

### `encumbrance`

_The characters current encumbrance flags._

This value is used as bit flags to lock the characters different equipment slots and stats that can be encumbered.

### `can_thumbs_up_mentor`

_Flag that states if the player can thumbs up a mentor in an assist channel._

This value is used to enable the `Thumbs Up` menu option when reviewing a message from a mentor in an assist channel. This will be set to `1` if the player can use this menu _(once per earth day)_, otherwise it will be set to 0.

### `mentor_rank`

_The characters mentor rank._

This value determines which mentor rank flag is shown when this user talks in Assist channels. _(This is also shown inside of the `Status -> Profile` menu.)_

  - `0` - _No rank._
  - `1` - _Bronze Flag_
  - `2` - _Silver Flag_
  - `3` - _Gold Flag_

_All other values will crash the client._

### `mastery_rank`

_The characters mastery rank._

This value determines the characters current mastery rank. This value is shown with the mentor flag within assist channels. _(This is also shown inside of the `Status -> Profile` menu.)_

This value can be `0` to `10`. _(Retail currently caps this to `8`)._ Values above 10 will cause the value to wrap back to 1 and show the next flag regardless of the players mentor rank. For example:

  - `mastery_rank` = _`11` will display silver flag and mastery rank 1._
  - `mastery_rank` = _`21` will display gold flag and mastery rank 1._

### `padding00`

_Padding; unused._

### `job_mastery_flags`

_The characters mastered jobs flags._

This value is holds the bitflags used to determine which jobs have mastery unlocked. The first bit is unused as a non-job value.

### `job_mastery_levels`

_The characters mastered job array._

This value holds the characters job mastery levels. The first byte is unused as a non-job value.
