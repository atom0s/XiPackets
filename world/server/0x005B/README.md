# `GP_SERV_COMMAND_WPOS`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_WPOS` |
| **Client Handler**        | `RecvWpos` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x005B` |
| **Size**                  | `0x001C` |

## Description

This packet is sent by the server to update an entities position information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_WPOS
struct GP_SERV_WPOS
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    float       x;          // PS2: x
    float       y;          // PS2: y
    float       z;          // PS2: z
    uint32_t    UniqueNo;   // PS2: UniqueNo
    uint16_t    ActIndex;   // PS2: ActIndex
    uint8_t     Mode;       // PS2: Mode
    char        dir;        // PS2: dir
    uint32_t    padding00;  // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_WPOS`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `x`

_The entities X position._

### `y`

_The entities Z position._

### `z`

_The entities Y position._

### `UniqueNo`

_The entities server id._

### `ActIndex`

_The entities target index._

### `Mode`

_The packet mode._

### `dir`

_The entities heading direction._

### `padding00`

_Padding; unused._

## Additional Information: `Mode`

The `Mode` value is used to determine how this packet will be handled. There are a total of 10 mode values the client will process, with any other mode value being ignored. The following shows which mode values are handled by the client and a brief description of what they do.

  - **Mode:** `0x00`
    - _Updates the entities current position information._
    - _If the entity is the local player; calls: `XiAtelBuff::ResetCamera(entity)` if the player currently does not have control of the camera._

  - **Mode:** `0x01`
    - _Updates the entities current position information._
    - _Updates the entities current event position information; if the entity is in (or part of) an event._
    - _If the entity is the local player; sets `PTR_RecPendingXZYFlag` to `0`._
    - _If the entity is the local player; calls: `XiAtelBuff::ResetCamera(entity)` if the player currently does not have control of the camera._

  - **Mode:** `0x02`
    - _If the entity is the local player; sets `PTR_RecPendingXZYFlag` to `0`._

  - **Mode:** `0x03`
  - **Mode:** `0x06` _(This mode does the same thing as mode `0x03`.)_
    - _Updates the entities current position information._
    - _Adjusts the entities render flags._
    - _Sets the entities `PopEffect` value to the `Mode` value._
    - _Deletes the entity. (Calls `XiAtelBuff::ObjectDelete`.)_

  - **Mode:** `0x05`
    - _Calibrates the entities position._
    - _Updates the entities current position information._
    - _If the entity is the local player; will perform the following:_
      - _Sets `PTR_ZoneChgReqFlag` to `0`._
      - _Sets `PTR_CliEventUcFlag` to `0`._
      - _Calls `FUNC_EventTargetCancel`._
      - _Sets `PTR_CliZoneFadeInFlag` to `1`._
      - _Sets `PTR_g_papp->app_phase` to `APP_ZONEIN`._
      - _Calls `XiAtelBuff::ResetCamera(entity)` if the player currently does not have control of the camera._

  - **Mode:** `0x07`
    - _Updates the entities current position information._
    - _If the entity is the local player; calls: `XiAtelBuff::ResetCamera(entity)` if the player currently has control of the camera._
    - _If the entity is the local player; calls: `XiZone::OpenIndoor(zone, 0);`._

  - **Mode:** `0x08`
    - _If the entity is the local player; sets `PTR_CliZoneFadeOutFlag` to `1`._
    - _If the entity is the local player; sets `PTR_g_papp->app_phase` to `APP_ZONEOUT`._

  - **Mode:** `0x09`
    - _If the entity is the local player; sets `PTR_CliZoneFadeInFlag` to `1`._
    - _If the entity is the local player; sets `PTR_g_papp->app_phase` to `APP_ZONEIN`._

  - **Mode:** `0x0A`
    - _Updates the entities current direction information._
    - _Calls `XiActor::SetDir(entity, pkt->dir)` to update the entity actor direction information if valid._

## Additional Information: `dir`

The `dir` value holds the entities heading direction, packed into a single byte value to conserve space. When the client sends or receives a heading direction within a packet, there is a translation that happens to convert it from a float to a byte, or the opposite, depending on the direction of the packet. When the client reads this value from a packet, it will translate it back to a float using the following function:

```cpp
float __cdecl FUNC_enDirNetToCli(uint8_t dir)
{
    return dir * 6.283185 * 0.00390625;
}
```
