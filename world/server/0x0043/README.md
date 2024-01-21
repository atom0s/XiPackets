# `GP_SERV_COMMAND_TALKNUMNAME`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_TALKNUMNAME` |
| **Client Handler**        | `RecvMessageTalkNumName` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0043` |
| **Size**                  | `0x0020` |

## Description

This packet is sent by the server to display a formatted message loaded from the DAT files.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_TALKNUMNAME
struct GP_SERV_TALKNUMNAME
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    UniqueNo;   // PS2: UniqueNo
    uint16_t    ActIndex;   // PS2: ActIndex
    uint16_t    MesNum;     // PS2: MesNum
    uint8_t     Type;       // PS2: Type
    uint8_t     padding00;  // PS2: dummmy
    uint16_t    padding01;  // PS2: dummy2
    uint8_t     sName[16];  // PS2: sName
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_TALKNUMNAME`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `UniqueNo`

_The message entity server id._

### `ActIndex`

_The message entity target index._

### `MesNum`

The `MesNum` value holds two separate values; the actual message number and a flag which is used to have the handler function ignore validating the values of the packets `ActIndex` and `UniqueNo` fields. Usually, these values will be validated to ensure the entity information in the packet is valid and accessible on the local client.

  - The message number can be filtered with: `pkt->MesNum & 0x7FFF`
  - The flag can be checked with: `pkt->MesNum & 0x8000`

The flag value is also used to tell the handler to not use the entity name when building the message to be printed.

The message number value is specifically used with the `SevMess` dialog string table. These strings are the general zone messages.

### `Type`

_The message type._

This value is used to determine the message type. The client uses a local lookup table for this value. Any value that is `>= 8` will be ignored and defaults to `0`, otherwise the following table is used:

| Type | Returned Lookup Value |
| --- | --- |
| `0` | `0x8E` |
| `1` | `0xA1` |
| `2` | `0x90` |
| `3` | `0x91` |
| `4` | `0x92` |
| `5` | `0xA1` |
| `6` | `0x94` |
| `7` | `0x95` |

_These values are used as the chat mode of the message that will be printed._

### `padding00`

_Padding; unused._

### `padding01`

_Padding; unused._

### `sName`

_The message name._
