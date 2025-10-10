# `GP_SERV_COMMAND_EVENTMES`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_EVENTMES` |
| **Client Handler**        | `RecvEventMes` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x003B` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the server to display a formatted message loaded from the DAT files.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_EVENTMES
struct GP_SERV_EVENTMES
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    UniqueNo;   // PS2: UniqueNoCas
    uint16_t    ActIndex;   // PS2: ActIndexCast
    uint16_t    Number;     // PS2: Number
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_EVENTMES`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The message entities server id._

### `ActIndex`

_The message entities target index._

### `Number`

_The message number._

The `Number` value holds two separate values; the actual message number and a flag which is used to determine if the message should make use of the given entities name. The entity is always validated regardless if this flag is set in this packet.

  - The message number can be filtered with: `pkt->Number & 0x7FFF`
  - The flag can be checked with: `pkt->Number & 0x8000`
