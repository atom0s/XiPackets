# `GP_CLI_COMMAND_SCENARIOITEM`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_SCENARIOITEM` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0064` |
| **Size**                  | `0x004C` |

## Description

This packet is sent by the client when viewing a key item that has not been viewed before. _(This marks the key item as 'seen' by the client to remove the yellow bubble when looking at the menu in the future. ie. 'Mark as Read')_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_SCENARIOITEM
struct GP_CLI_SCENARIOITEM
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    UniqueNo;           // PS2: UniqueNo
    uint32_t    LookItemFlag[16];   // PS2: para
    uint16_t    ActIndex;           // PS2: ActIndex
    uint16_t    TableIndex;         // PS2: Dammy
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_SCENARIOITEM`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The clients server id._

### `LookItemFlag`

_The clients seen key item information for the table that contains the key item that was viewed._

This value holds bits that mark key items as seen by the client. The key item information for both obtained and seen values makes use of table offsetting to allow for smaller amounts of data being transferred instead of sending the full object list in either direction. The `TableIndex` value is used to determine the starting point of where the `LookItemFlag` data begins.

### `ActIndex`

_The clients target index._

### `TableIndex`

_The key item table index that the `LookItemFlag` is for._
