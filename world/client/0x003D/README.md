# `GP_CLI_COMMAND_BLACK_EDIT`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_BLACK_EDIT` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x003D` |
| **Size**                  | `0x001C` |

## Description

This packet is sent by the client when interacting with the blacklist system.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: SAVE_BLACK
struct SAVE_BLACK
{
    uint32_t    ID;         // PS2: ID
    uint8_t     Name[16];   // PS2: Name
};

// PS2: GP_BLACK_EDIT
struct GP_BLACK_EDIT
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    SAVE_BLACK  Data;           // PS2: Data
    int8_t      Mode;           // PS2: Mode
    uint8_t     padding19[3];   // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_BLACK_EDIT`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Data`

_The blacklist entry._

### `Mode`

_The packets mode._

This value is used to determine what action is being performed with the blacklist.

| Mode | Purpose |
| --- | --- |
| `0` | _Adding a player to the blacklist._ |
| `1` | _Removing a player from the blacklist._ |

### `padding19`

_Padding; unused._

## Structure Fields (`SAVE_BLACK`)

### `ID`

_The blacklisted character server id._

This value is set to `0` when being sent from the client.

### `Name`

_The name of the character to blacklist._
