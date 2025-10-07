# `GP_CLI_COMMAND_MOTION`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_MOTION` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x005D` |
| **Size**                  | `0x0010` |

## Description

This packet is sent by the client when using an emote.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_MOTION
struct GP_CLI_MOTION
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    UniqueNo;   // PS2: UniqueNo
    uint16_t    ActIndex;   // PS2: ActIndex
    uint8_t     Number;     // PS2: Number
    uint8_t     Mode;       // PS2: Mode
    uint16_t    Param;      // PS2: (New; did not exist.)
    uint16_t    padding0E;  // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_MOTION`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The server id of the entity the emote is being acted upon._

### `ActIndex`

_The target index of the entity the emote is being acted upon._

### `Number`

_The emote id._

### `Mode`

_The emote mode._

| Mode | Purpose |
| --- | --- |
| `0` | _Both Mode. [ie. `/wave` and `/wave both`] (This is the default mode when no parameter.)_ |
| `1` | _Text Mode. [ie. `/wave text`]_ |
| `2` | _Motion Mode. [ie. `/wave motion`]_ |

### `Param`

_The emote parameter._

This value is used for certain emotes as an extra parameter value.

| Emote | Value |
| --- | --- |
| `(Default)`   | _Set to: `0`_ |
| `/hurray`     | _Set to: `1`_ |
| `/dance1`     | _Set to: `2`_ |
| `/dance2`     | _Set to: `3`_ |
| `/dance3`     | _Set to: `4`_ |
| `/dance4`     | _Set to: `5`_ |
| `/bell`       | _Set to the bell note value. [ie. `/bell c4` will be `6`.]_ |
| `/jobemote`   | _Set to the job id + 30. [ie. `/jobemote war` will be `31`.]_ |
| `/aim`        | _Set to: `53`_ |

### `padding0E`

_Padding; unused._
