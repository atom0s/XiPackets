# `GP_SERV_COMMAND_GMSCITEM`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_GMSCITEM` |
| **Client Handler**        | `RecvGmScitem` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00B7` |
| **Size**                  | `(unknown)` |

## Description

> [!NOTE]
> This is a special GM-related packet!

This packet is sent by the server to inform a GM of a characters obtained key items.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_GMSCITEM
struct GP_SERV_GMSCITEM
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    ItemFlag[16];   // PS2: ItemFlag
    uint32_t    UniqueNo;       // PS2: UniqueNo
    uint8_t     sName[16];      // PS2: sName
    uint16_t    TableIndex;     // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_GMSCITEM`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ItemFlag`

_The requested players key item information._

### `UniqueNo`

_The requested players server id._

### `sName`

_The requested players name._

### `TableIndex`

_The key item table index._

This value states which block of key item information is stored within the `ItemFlag` data. This value can be `0` to `6`.

## Additional Information

The size of this packet is unknown as this packet is not obtainable by a normal client. The structure of the packet has slightly changed since the PS2 beta as well. Given the alignment of the packet in its current format, it is assumed that the size of this packet would likely be `0x5C`.

The handler for this packet shows that this would be used by GM's to print a players current obtained key item list. This would likely be useful when a GM is assisting a player that is having issues with an event, mission, quest or some other content that requires specific key item(s) in order to complete.
