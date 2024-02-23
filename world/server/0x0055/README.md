# `GP_SERV_COMMAND_SCENARIOITEM`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_SCENARIOITEM` |
| **Client Handler**        | `RecvScenarioItem` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0055` |
| **Size**                  | `0x0088` |

## Description

This packet is sent by the server to populate the clients key item information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_SCENARIOITEM
struct GP_SERV_SCENARIOITEM
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    GetItemFlag[16];    // PS2: GetItemFlag
    uint32_t    LookItemFlag[16];   // PS2: LookItemFlag
    uint16_t    TableIndex;         // PS2: (New; did not exist.)
    uint16_t    padding00;          // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_SCENARIOITEM`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `GetItemFlag`

_The clients obtained key item bit data._

### `LookItemFlag`

_The clients viewed key item bit data._

### `TableIndex`

_The key item table index this data will populate._

This value can be `0` to `6`.

### `padding00`

_Padding; unused._
