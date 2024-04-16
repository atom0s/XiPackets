# `Packet: 0x009B`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x009B` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the client when interacting with the chocobo racing system.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    Param;
    uint32_t    Kind;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Param`

_The packet parameter._

This value is set based on the event VM opcode execution that opens the requested chocobo racing related window. The client makes use of the event opcode `0x00B4` to work with these menus, among other things. The opcode modes that function with this system are:

  - `0x0F` - _Opens the racing window. (Toteboard)_
  - `0x10` - _Closes the racing window. (Toteboard)_
  - `0x11` - _Opens the card window. (Chocobo List)_
  - `0x12` - _Closes the card window. (Chocobo List)_

When this opcode is used with the above parameters, the opcode will also contain an additional parameter value that is used with the window that is opened. This additional parameter is what populates this value.

### `Kind`

_The packet kind._

| Kind | Purpose |
| --- | --- |
| `1` | _Requesting Toteboard information._ |
| `2` | _Requesting Chocobo List information._ |
