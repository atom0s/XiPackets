# `GP_CLI_COMMAND_TROPHY_ENTRY`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_TROPHY_ENTRY` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0041` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when casting lots on treasure pool items.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_TROPHY_ENTRY
struct GP_CLI_TROPHY_ENTRY
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     TrophyItemIndex;    // PS2: TrophyItemIndex
    uint8_t     PropertyItemIndex;  // PS2: PropertyItemIndex
    uint8_t     padding00[2];       // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_TROPHY_ENTRY`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `TrophyItemIndex`

_The treasure pool window index of the item being lotted on._

### `PropertyItemIndex`

_The clients local inventory index where it assumed the item can be placed._

The client obtains this value using its `gcItemOneSpeaceGet` function. It walks the clients inventory _(starting at index 1 to skip gil)_ to locate the first item entry that has no set valid item. This value is the incremented index found to be empty.

### `padding00`

_Padding; unused._
