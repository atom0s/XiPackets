# `GP_SERV_COMMAND_BLACK_EDIT`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_BLACK_EDIT` |
| **Client Handler**        | `RecvBlackEdit` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0042` |
| **Size**                  | `0x001C` |

## Description

This packet is sent by the server to edit the clients blacklist. _(Add or Delete)_

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
    uint8_t     padding00[3];   // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_BLACK_EDIT`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `Data`

_The blacklist entry._

### `Mode`

_The packets mode._

The client uses this value to determine how the edit is being done.

  - `0` - _Add - Adds the new entry to the clients blacklist._
  - `1` - _Delete - Removes the entry from the clients blacklist._

### `padding00`

_Padding; unused._

## Structure Fields (`SAVE_BLACK`)

### `ID`

_The blacklisted character server id._

### `Name`

_The blacklisted character name._
