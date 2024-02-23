# `GP_SERV_COMMAND_MAGIC_DATA`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_MAGIC_DATA` |
| **Client Handler**        | `RecvMagicData` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00AA` |
| **Size**                  | `0x0084` |

## Description

This packet is sent by the server to populate the clients list of available magic spells.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_MAGIC_DATA
struct GP_SERV_MAGIC_DATA
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     MagicDataTbl[128]; // PS2: MagicDataTbl
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_MAGIC_DATA`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `MagicDataTbl`

_The clients available magic information._

This value holds the clients learned magic spell information, in the form of bits. Each bit translates directly to the spell at the same index of the bit.

The first bit in this value is not used, as no spell uses index 0. The first spell at index `1` is `Cure`.

## Additional Information

The client directly copies the information in `MagicDataTbl` into its own local buffer `PTR_pGlobalNowZone->MagicSys`. The client uses this information to determine if the player has access to a given spell when trying to cast them, as well as to know which spells to render in the available spells list. It also uses this information to determine which spells the client knows when showing spells on other menus such as the auction house.

The client will test if a spell is available using the following:

```cpp
const auto msys = FUNC_gcMagicDataGet();
if (msys && ((1 << (index % 8)) & msys->MagicDataTbl[index / 8]) == 0)
{
    // client does not know the given spell..
}
else
{
    // client knows the spell..
}
```
