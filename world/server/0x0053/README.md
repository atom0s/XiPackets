# `GP_SERV_COMMAND_SYSTEMMES`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_SYSTEMMES` |
| **Client Handler**        | `RecvSystemMessage` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0053` |
| **Size**                  | `0x0010` |

## Description

This packet is sent by the server to display a formatted message loaded from the DAT files. _(via `PutSystemMessage`)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_SYSTEMMES
struct GP_SERV_SYSTEMMES
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    para;       // PS2: para
    uint32_t    para2;      // PS2: para2
    uint16_t    Number;     // PS2: Number
    uint16_t    padding00;  // PS2: padding00
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_SYSTEMMES`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `para`

_The message parameter. (1)_

### `para2`

_The message parameter. (2)_

### `Number`

_The message number._

### `padding00`

_Padding; unused._

## Additional Information

This packet loads and prints a formatted message from the following DAT files:

| Language | File Id | File |
| --- | --- | --- |
| `Japanese` | `7030` | `ROM/27/75.DAT` |
| `English` | `7031` | `ROM/27/76.DAT` |
