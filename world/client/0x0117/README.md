# `Packet: 0x0117`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0117` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when requesting its Unity quest information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    Kind;
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

This value is always set to `0`.

The retail client has handling for additional `Kind` values _(`1` and `2`)_ but no call is made that would produce these values to happen.

## Additional Information

The client sends this packet when requesting additional Unity information. This packet will be used when the client does the following:

  - Opening the `Main Menu -> Status -> Unity -> Ranking Info -> (either sub menu)` menus.
  - Opening the `Main Menu -> Quests -> Records of Eminence -> Current` menu.
  - Opening the `Main Menu -> Quests -> Records of Eminence -> Objective List` menu.
