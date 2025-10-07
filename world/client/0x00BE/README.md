# `Packet: 0x00BE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00BE` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the client when interacting with the merit system.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     Kind;   // PS2: (New; did not exist.)
    uint8_t     Param1; // PS2: (New; did not exist.)
    uint16_t    Param2; // PS2: (New; did not exist.)
    uint32_t    Param3; // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Kind`

_The packet kind._

| Kind | Purpose |
| --- | --- |
| `0x01` | _Unknown. (No longer used.)_ |
| `0x02` | _Change Mode (EXP/Limit)_ |
| `0x03` | _Edit Mode (Increase/Decrease Merit Point)_ |

### `Param1`

_The packet parameter. (1)_

This value depends on the packet `Kind`.

| Kind | Usage |
| --- | --- |
| `0x01` | _Set to: `0`_ |
| `0x02` | _Set to: `0` (EXP Mode) or `1` (Limit Points Mode)_ |
| `0x03` | _Set to: `0` (Lower Merit) or `1` (Raise Merit)_ |

### `Param2`

_The packet parameter. (2)_

This value depends on the packet `Kind`.

| Kind | Usage |
| --- | --- |
| `0x01` | _Set to: `0`_ |
| `0x02` | _Set to: `0`_ |
| `0x03` | _Set to: `(merit index)`_ |

### `Param3`

_The packet parameter. (3)_

This value depends on the packet `Kind`.

| Kind | Usage |
| --- | --- |
| `0x01` | _Set to: `0`_ |
| `0x02` | _Set to: `0`_ |
| `0x03` | _Set to: `0`_ |
