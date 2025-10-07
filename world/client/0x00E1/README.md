# `GP_CLI_COMMAND_GET_LSMSG`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_GET_LSMSG` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00E1` |
| **Size**                  | `0x0090` |

## Description

This packet is sent by the client when requesting the current linkshell message of an equipped linkshell. _(ie. `/lsmes`, `/linkshell2mes`)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_LINKSHELL_MESSAGE
struct GP_CLI_LINKSHELL_MESSAGE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     unknown00       : 4;    // PS2: stat
    uint8_t     unknown04       : 1;    // PS2: attr (Was originally a single 4 bit value.)
    uint8_t     unknown05       : 1;    // PS2: attr (Was originally a single 4 bit value.)
    uint8_t     unknown06       : 1;    // PS2: attr (Was originally a single 4 bit value.)
    uint8_t     unknown07       : 1;    // PS2: attr (Was originally a single 4 bit value.)
    uint8_t     readLevel       : 2;    // PS2: readLevel
    uint8_t     writeLevel      : 2;    // PS2: writeLevel
    uint8_t     pubEditLevel    : 2;    // PS2: pubEditLevel
    uint8_t     LinkshellId     : 2;    // PS2: dummyBits
    uint8_t     Category;               // PS2: (New; did not exist.)
    uint8_t     ItemIndex;              // PS2: (New; did not exist.)
    uint8_t     padding08[2];           // PS2: (New; did not exist.)
    uint16_t    seqId;                  // PS2: seqId
    uint32_t    uniqNo;                 // PS2: uniqNo
    uint8_t     sMessage[128];          // PS2: sMessage
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_LINKSHELL_MESSAGE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `unknown00`

_Unknown._

This packet does not use this value.

### `unknown04`

_Unknown._

This packet does not use this value.

### `unknown05`

_Unknown._

This packet does not use this value.

### `unknown06`

_Unknown._

This packet does not use this value.

### `unknown07`

_Unknown._

This packet does not use this value.

### `readLevel`

_The access level required to read the linkshell message._

This packet does not use this value.

### `writeLevel`

_The access level required to write the linkshell message._

This packet does not use this value.

### `pubEditLevel`

_Unknown._

This packet does not use this value.

### `LinkshellId`

_The linkshell id._

  - `0` - _Linkshell 1_
  - `1` - _Linkshell 2_

### `Category`

_The container holding the linkshell item._

### `ItemIndex`

_The index inside of the container the linkshell item is located._

### `padding08`

_Padding; unused._

### `seqId`

_The linkshell message sequence id._

This value is set to `0`.

### `uniqNo`

_The client server id._

This value is set to `0`.

### `sMessage`

_The linkshell message._

This packet does not use this value.

## Additional Information

The client sends this packet when requesting to view the linkshell message of an equipped linkshell. This will happen automatically when the client equips a linkshell item or manually when using one of the linkshell message commands. _(`/lsmes`, `/linkshell2mes`)_

_This packets structure is reused for multiple other packets. Due to this, multiple fields are unused depending on the kind of packet that is making use of the structure. Packets that use this same structure layout are: `0x00E1`, `0x00E2`, `0x00E4`_
