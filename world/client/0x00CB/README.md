# `GP_CLI_COMMAND_MYROOM_IS`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_MYROOM_IS` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00CB` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when interacting with different mog house functionality.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Kind;
    uint8_t     Param1;
    uint16_t    Param2;
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
| `1` | _Open mog house._ |
| `2` | _Close mog house._ |
| `5` | _Remodel mog house._ |

### `Param1`

_The packet parameter. (1)_

This value will depend on the packet `Kind`.

| Kind | Value |
| --- | --- |
| `1` | _Set to: `0`_ |
| `2` | _Set to: `1`_ |
| `5` | _Set to: `1`_ |

### `Param2`

_The packet parameter. (2)_

This value will depend on the packet `Kind`.

| Kind | Value |
| --- | --- |
| `1` | _Set to: `0`_ |
| `2` | _Set to: `1`_ |
| `5` | _Set to: `103` (San d'Orian Style), `104` (Bastokan Style), `105` (Windurstian Style), `106` (Mog Patio)_ |
