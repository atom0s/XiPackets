# `Packet: 0x0026`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0026` |
| **Size**                  | `0x001C` |

## Description

This packet is sent by the server to inform the player of an items sub-container information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     data[24]; // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `data`

_The item data._

This value is a block of data that contains information related to the items sub-container storage.

## Additional Information

The server sends a `0x26` packet for every item that is currently stored within the players `Mog Safe` (`1`) and `Mog Safe 2` (`9`) containers. However, the only item type in the game that currently uses the full contents of this packet is `Mannequin` items.

The `data` field is populated with additional information related to the items sub-contents when used. This field starts with three guaranteed bytes:

```cpp
struct data_t
{
    uint8_t is_used;
    uint8_t container;
    uint8_t index;
};
```

### `is_used`

_Flag that states if the rest of the `data` field is populated._

### `container`

_The items parent container._

### `index`

_The index within the parent container the item is located._

---

As stated, at this time the only item to make use of the `data` field is `Mannequin` items. The `is_used` field will be set to `0` for all items except for `Mannequin` items, which will set this to `1` instead. When this is true, the `container` and `index` fields will be populated with the location of the `Mannequin` item this packet relates to.

The remaining data is then dependent on the kind of item. _(Again, at this time only Mannequin items use this packet fully.)_

## Additional Information: Mannequin Data

When the client receives an `0x26` packet for a `Mannequin` item, the `data` field will be populated as follows:

```cpp
struct data_t
{
    uint8_t     is_used;
    uint8_t     container;
    uint8_t     index;
    uint8_t     unknown00;
    uint8_t     unknown01[2];
    uint16_t    model_id_race_hair;
    uint16_t    model_id_head;
    uint16_t    model_id_body;
    uint16_t    model_id_hands;
    uint16_t    model_id_legs;
    uint16_t    model_id_feet;
    uint16_t    model_id_main;
    uint16_t    model_id_sub;
    uint16_t    model_id_range;
};
```

### `unknown00`

_Unknown._

This value is generally set to `0`.

_The client does not appear to use this value._

### `unknown01`

_Unknown._

This value is generally set to `FC FC`.

_The client does not appear to use this value._

### `model_id_race_hair`

_The mannequins race and face/hair values._

Mannequins use the same race values as player entities. However, they use a custom face/hair model id of `31`.

### `model_id_head`

_The mannequins head model id._

### `model_id_body`

_The mannequins body model id._

### `model_id_hands`

_The mannequins hands model id._

### `model_id_legs`

_The mannequins legs model id._

### `model_id_feet`

_The mannequins feet model id._

### `model_id_main`

_The mannequins main weapon model id._

### `model_id_sub`

_The mannequins sub weapon model id._

### `model_id_range`

_The mannequins ranged weapon model id._
