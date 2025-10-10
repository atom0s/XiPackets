# `GP_SERV_COMMAND_TRACKING_LIST`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_TRACKING_LIST` |
| **Client Handler**        | `RecvList` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00F4` |
| **Size**                  | `0x001C` |

## Description

This packet is sent by the server in response to a clients wide scan request, used to populate a wide scan entry.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_TRACKING
struct GP_TRACKING
{
    uint32_t    ActIndex   : 16;    // PS2: ActIndex
    uint32_t    Level      : 8;     // PS2: Level
    uint32_t    Type       : 3;     // PS2: Type
    uint32_t    unused     : 5;     // PS2: unused
    int16_t     x;                  // PS2: x
    int16_t     z;                  // PS2: z
    uint8_t     sName[16];          // PS2: sName
};

// PS2: GP_SERV_TRACKING_LIST
struct GP_SERV_TRACKING_LIST
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    GP_TRACKING TrackingListTbl[1]; // PS2: TrackingListTbl
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_TRACKING_LIST`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `TrackingListTbl`

_The wide scan entry._

This value holds the initial information for the wide scan entry to be populated in the list shown to the client.

## Structure Fields (`GP_TRACKING`)

### `ActIndex`

_The entities target index._

### `Level`

_The entities level._

### `Type`

_The entities type._

This value controls the color of the dot shown on the map.

| Type | Color | Purpose |
| --- | --- | --- |
| `0` | `None/Blue` | _Unknown. (Assumed to be used for players.)_ |
| `1` | `Green`     | _NPC entities; friendly._ |
| `2` | `Red`       | _NPC entities; enemies._ |

_**Note:** The client has special handling when the `Type` value is 0. If this type is used, the client will check if `sName` is set. If not, then the entry is hidden from the list and the entity dot is not rendered. If `sName` is set, the dot will be rendered blue and the name in the list will be overridden with the value stored in `sName`._

### `unused`

_Unused._

### `x`

_The X difference between the local client and the wide scan entity._

### `z`

_The Z difference between the local client and the wide scan entity._

### `sName`

_The entity name._
