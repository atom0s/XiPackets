# `Unknown - Packet: 0x0075`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0075` |
| **Size**                  | `0x00AC` |

## Description

This packet is sent by the server to inform the client of extra data used for certain battlefield content.

_The observed content that uses this packet is Delve, Domain Invasion and Skirmish._

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    Mode;
    uint32_t    Timestamp;
    uint32_t    Duration;
    uint32_t    DurationWarn;
    int32_t     FenceX;
    int32_t     FenceY;
    uint32_t    FenceRadius;
    uint32_t    FenceRotation;
    uint8_t     Flags;
    uint8_t     FenceColor;
    uint8_t     unknown26;
    uint8_t     padding00;
    uint8_t     Data[128];
    uint16_t    MesNumTitle;
    uint16_t    MesNumDescription;
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

This value determines the content type that this packet is being used with. It will cause the client to change how additional information in this packet is used _(mainly the `Data`)_ when certain flags are set.

  - `0` - _Used when the content information is being cleared or unset. (Combined with cleared flags and other data.)_
  - `1` - _Used for timed based content that has no additional or special case handling._
  - `0x1000` - _Used for Yorcia Alluvion Skirmish, causes the client to show the 'Marshland' and 'Stronghold' scoreboard information._
  - `0xFFFF` - _Used for targeted enemy content that shows up to 6 named progress bars._

The client will treat any non-zero `Mode` value the same as `1` unless it is one of the specific values above. Both `0x1000` and `0xFFFF` can also have timer information displayed when the proper `Flags` value is used.

### `Timestamp`

_The base timestamp of the content._

This value is the timestamp of when the content was started. This can also just be recalculated to the proper difference when entering already started content. It's value is the number of seconds since Vana'diel Epoch.

_Vana'diel epoch starts at: `1009810800`_

### `Duration`

_The content duration, in seconds._

This value is the duration that will be displayed on the timer, in seconds.

### `DurationWarn`

_The content duration, in seconds, to begin warning the client._

This value is the duration amount when the client will start blinking the timer red, warning the client of running out of time.

If no value is given, it will default to `60`.

### `FenceX`

_The center X position where the client will display a fence around the content._

This value is the center X position of the fencing that shows for content such as Domain Invasion.

The client uses this value as follows: `FenceX * 0.001`

### `FenceY`

_The center Y position where the client will display a fence around the content._

This value is the center Y position of the fencing that shows for content such as Domain Invasion.

The client uses this value as follows: `FenceY * 0.001`

### `FenceRadius`

_The fence radius._

This value is the radius of the fencing, from the center XY position.

The client uses this value as follows: `FenceRadius * 0.001`

For example, Domain Invasion often uses a radius value of `25000`.

### `FenceRotation`

_The fence rotation._

This value is the rotation of the fencing. It's value must be at least the same value of `FenceRadius` otherwise the client will not render the fence. This value is generally set to the same value as `FenceRadius`.

For example, Domain Invasion often uses a rotation value of `25000`.

### `Flags`

_The packet flags._

This value is used to determine which parts of this packets information is populated and should be used.

| Flag | Usage | Fields Used |
| --- | --- | --- |
| `0x01` | _Used when timestamp information is used._   | `Timestamp`, `Duration`, `DurationWarn` |
| `0x02` | _Used when extra data is used._              | `Data` |
| `0x04` | _Used when message ids are used._            | `MesNumTitle`, `MesNumDescription` |
| `0x08` | _Used when unknown value is used._           | `unknown26` |

The following fields are always used regardless of these `Flags` and `Mode`:
  - `FenceColor`, `FenceX`, `FenceY`, `FenceRadius`, `FenceRotation`

### `FenceColor`

_The fence color id._

This value is used as the index into a lookup table that is then translated to a DAT file id. This value can only be either `0` or `1`. All other values are treated as `0`.

  - `0` - The fencing will be orange/red.
  - `1` - The fencing will be blue.

The client uses this value as an index into a lookup table that is then translated to a DAT file id. The following two DATs are used for this:

  - `ROM\322\125.DAT` (File Id: `52741`)
  - `ROM\338\74.DAT` (File Id: `52751`)

_**Note:** The clients entity must be 'refreshed' causing `XiSkeletonActor::Setup` to be called again in order for the fence to refresh itself and change colors if it was already spawned!_

### `unknown26`

_Unknown._

This value is used in a check that happens during `XiSkeletonActor::OnMove`. If this value is `0`, then the check is skipped. Otherwise, all other values will cause the check to happen. _(It appears that this is intended to be used to hide all other players not part of the battle content, but its exact purpose is not known at this time.)_

### `padding00`

_Padding; unused._

### `Data`

_The content data._

This value is used to hold extended information about the content. It holds different kind of information based on the current packet `Mode`.

_See notes below for more information._

### `MesNumTitle`

_The title message number._

This value is the string index used for the menu title that is displayed in the top-left corner of the client.

### `MesNumDescription`

_The description message number._

This value is the string index used for the menu description that is displayed in the top-center of the client.

## Additional Information - `Data`

The `Data` field holds the extended information about the content and will vary based on the current `Mode`. Currently, there are two different forms of data that will be stored in this field, which are used for modes `0x1000` and `0xFFFF`.

### Mode: `0x1000`

When this mode is present, the `Data` content will be the scoreboard information for the `Marchland` and `Stronghold`. This data is stored as follows:

```cpp
struct data_t
{
    int32_t     MarchlandScore;
    int32_t     StrongholdScore;
    uint32_t    MarchlandProgress;
    uint32_t    MarchlandProgressMax;
    uint32_t    StrongholdProgress;
    uint32_t    StrongholdProgressMax;
    uint32_t    MarchlandNameOverride;
    uint32_t    StrongholdNameOverride;
};
```

### `MarchlandScore`

_The current score value for Marchland._

### `StrongholdScore`

_The current score value for Stronghold._

### `MarchlandProgress`

_The current progress value for Marchland._

This value is used as the progress bars current value.

### `MarchlandProgressMax`

_The max progress value for Marchland._

This value is used as the progress bars maximum value. If this is set to 0, then the progress bar will be greyed out.

### `StrongholdProgress`

_The current progress value for Stronghold._

### `StrongholdProgressMax`

_The max progress value for Stronghold._

This value is used as the progress bars maximum value. If this is set to 0, then the progress bar will be greyed out.

### `MarchlandNameOverride`

_The Marchland name override flag._

This value is not used.

### `StrongholdNameOverride`

_The Stronghold name override flag._

When this value is set to any non-zero flag, then the `Stronghold` name will be replaced with `Balamor's Adumbration`.

### Mode: `0xFFFF`

When this mode is present, the `Data` content holds up to six entries that are used to display a name and progress bar for focused/targeted enemies or other related information. The structure layout for each entry is as follows:

```cpp
struct data_t
{
    uint32_t    Progress;
    int8_t      Name[16];
    uint32_t    unused;
};
```

### `Progress`

_The progress value for the entry._

This value is used to draw the progress bar percent that is filled. If this value is set to `0` the bar will be greyed out. Otherwise, its value is the percent of the bar that should be filled, up to a maximum of `100`. _(Anything higher and the bar will reset.)_

### `Name`

_The entry name._

This value is used for the name that is displayed with the bar. If the first byte of this value is `0`, then the entry is not shown. The client allows the full 16 bytes to be used for displaying a name.

### `unused`

_Unused._

This value is unused; assumed to be for alignment purposes.

## Additional Information - `MesNumTitle` & `MesNumDescription`

These values are used to display the contents title and description in the top-left/center of the client menu system, if used. These strings are loaded from the following DAT files:

  - JP: - `ROM\333\15.DAT` (File Id: `55559`)
  - NA: - `ROM\333\16.DAT` (File Id: `55679`)

The `MesNumTitle` value will be an index between `0` and `18` _(currently)_ which is the list of titles stored in the above DAT files.

The `MesNumDescription` value will be an index starting at `0`. The client automatically adds `19` to this value itself, which shifts it just beyond the title block within the DAT file, allowing it to pick from the description strings with a `0` based index.
