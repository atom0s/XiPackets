# `GP_SERV_COMMAND_ITEM_SAME`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_ITEM_SAME` |
| **Client Handler**        | `RecvItemSame` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x001D` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the server to inform the client of inventory container updates and if all containers have been loaded/updated.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_ITEM_SAME
struct GP_SERV_ITEM_SAME
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     State;          // PS2: State
    uint8_t     padding05[3];   // PS2: (New; did not exist.)
    uint32_t    Flags;          // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_ITEM_SAME`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `State`

_The current state of inventory container loading for the client._

  - `0` - _A container is still loading contents._
  - `1` - _All containers have loaded._

When the `State` value is set to `1`, the client considers all containers as loaded and ready. When this state is seen, the client will set the current `pGlobalNowZone->Flg` value to `OR` in the needed flag values to disable the downloading container icon and mark the client as ready to interact with the containers.

### `padding05`

_Padding; unused._

This data appears to be used as the container index at times, but the client does not use any part of it at all.

### `Flags`

_The overall inventory container table flags._

This value holds the bit flags which mark a container as ready to be used and accessible. If the flag for a given container is not set, then the client will not draw its contents to the player. These flags are checked based on the container index as follows:

```cpp
// Note: This is not the real function name, this is just what I've named it.
bool __cdecl FUNC_gcItemIsContainerReady(uint8_t idx)
{
    return PTR_pGlobalNowZone && ((1 << idx) & PTR_pGlobalNowZone->ItemSys.TblUpdateFlags) != 0;
}
```
