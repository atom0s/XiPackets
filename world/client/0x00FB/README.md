# `GP_CLI_COMMAND_MYROOM_BANKIN`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_MYROOM_BANKIN` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00FB` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when removing a placed piece of furniture in their mog house.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_MYROOM_BANKIN
struct GP_CLI_MYROOM_BANKIN
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    MyroomItemNo;   // PS2: MyroomItemNo
    uint8_t     MyroomItemIndex;// PS2: MyroomItemIndex
    uint8_t     MyroomCategory; // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_MYROOM_BANKIN`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `MyroomItemNo`

_The item id of the furnishing being modified._

### `MyroomItemIndex`

_The index within the container that holds the furnishing being modified._

### `MyroomCategory`

_The item container that holds the furnishing being modified._
