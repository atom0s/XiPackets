# `Packet: 0x00C1`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00C1` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when upgrading an Alter Ego Points system category.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    Kind;
    uint8_t     padding06[2];
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

This value is used to tell which sub-category is being upgraded.

### `padding06`

_Padding; unused._

## Additional Information

The following `Kind` values are used with this packet:

### Category: `HP / MP`

| Kind | Sub-Category |
| --- | --- |
| `0x08` | `Max HP` |
| `0x09` | `Max MP` |

### Category: `Stats`

| Kind | Sub-Category |
| --- | --- |
| `0x0A` | `STR` |
| `0x0B` | `DEX` |
| `0x0C` | `VIT` |
| `0x0D` | `AGI` |
| `0x0E` | `INT` |
| `0x0F` | `MND` |
| `0x10` | `CHR` |
