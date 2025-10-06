# `GP_CLI_COMMAND_EQUIP_INSPECT`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_EQUIP_INSPECT` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00DD` |
| **Size**                  | `0x0010` |

## Description

This packet is sent by the client when inspecting entities. This is used for several means of inspection such as:

  - `/check`
  - `/checkname`
  - `/checkparam`

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_EQUIP_INSPECT
struct GP_CLI_EQUIP_INSPECT
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    UniqueNo;       // PS2: UniqueNo
    uint32_t    ActIndex;       // PS2: ActIndex
    uint8_t     Kind;           // PS2: (New; did not exist.)
    uint8_t     padding00[3];   // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_EQUIP_INSPECT`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The server id of the entity being inspected._

### `ActIndex`

_The target index of the entity being inspected._

### `Kind`

_The kind of inspection being performed._

| Kind | Purpose |
| --- | --- |
| `0x00` | _Used with: `/check`._ |
| `0x01` | _Used with: `/checkname`_ |
| `0x02` | _Used with: `/checkparam`_ |

### `padding00`

_Padding; unused._
