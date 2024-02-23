# `GP_SERV_COMMAND_FEAT_DATA`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_FEAT_DATA` |
| **Client Handler**        | `RecvFeatData` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00AB` |
| **Size**                  | `0x0018` |

## Description

The purpose of this packet is unknown. The below information is based on the PS2 data.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_FEAT_DATA
struct GP_SERV_FEAT_DATA
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     FeatDataTbl[20];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_FEAT_DATA`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `FeatDataTbl`

_Unknown._

## Additional Information

The purpose of this packet is unknown. The client uses this information as part of the feature system, which includes the clients available abilities, spells, etc. However, this packets data is stored in the clients local `PTR_pGlobalNowZone->FeatSys.FeatDataTbl` buffer and then never used. All observed packets across multiple accounts/characters have shown this data to always be fully zero.
