# `GP_CLI_COMMAND_PREFERENCE_SAVE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_PREFERENCE_SAVE` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x008D` |
| **Size**                  | `0x0040` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

> [!NOTE]
> This is a special GM-related packet!

This packet is used with the GM command `//savepref`. The purpose of this packet is unknown.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_PREFERENCE_SAVE
struct GP_CLI_PREFERENCE_SAVE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    int32_t     ContentLen;         // PS2: ContentLen
    int32_t     SegmentIndex;       // PS2: SegmentIndex
    char        PreferenceData[50]; // PS2: PreferenceData
    char        SendDone;           // PS2: SendDone
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_PREFERENCE_SAVE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ContentLen`

_The length of the preference data content contained within `PreferenceData`._

### `SegmentIndex`

_The index of the current data segment._

### `PreferenceData`

_The actual preference data string segment._

### `SendDone`

_Flag set when all data has been sent._
