# `GP_SERV_COMMAND_PREFERENCE_DATA`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_PREFERENCE_DATA` |
| **Client Handler**        | `receivePreferenceData` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0064` |
| **Size**                  | `(unknown)` |

## Description

Unknown. This packet is deprecated.

> [!WARNING]
> This packets information is dumped from the PS2 beta. The handler usage and data align to this structure still.

This packet was originally used with a set of commands that are no longer part of the retail client:

  - `/getpref`
  - `/savepref`
  - `/showpref`

Its intended purpose is unknown. The value(s) stored in the data this packet used were strings the client would print out directly when requested.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_PREFERENCE_DATA
struct GP_SERV_PREFERENCE_DATA
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

## Structure Fields (`GP_SERV_PREFERENCE_DATA`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ContentLen`

_The content length._

This value is unused.

### `SegmentIndex`

_The segment index._

### `PreferenceData`

_The preference data to be stored._

### `SendDone`

_Flag that states all preference data has been sent/populated._
