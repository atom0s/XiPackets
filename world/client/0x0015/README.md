# `GP_CLI_COMMAND_POS`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_POS` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0015` |
| **Size**                  | `0x0020` |

## Description

This packet is sent by the client to update its position, and other general status information, with the server.

## Packet Layout

The layout of this packet is the following:

```cpp
struct GP_CLI_POS
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    float       x;                  // PS2: x
    float       z;                  // PS2: z
    float       y;                  // PS2: y
    uint16_t    MovTime;            // PS2: MovTime
    uint16_t    MoveFlame;          // PS2: MoveFlame
    int8_t      dir;                // PS2: dir
    uint8_t     TargetMode  : 1;    // PS2: TargetMode
    uint8_t     RunMode     : 1;    // PS2: RunMode
    uint8_t     GroundMode  : 1;    // PS2: GroundMode
    uint8_t     unused      : 5;    // PS2: dummy
    uint16_t    facetarget;         // PS2: facetarget
    uint32_t    TimeNow;            // PS2: TimeNow
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_POS`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `x`

_The clients current X position._

### `z`

_The clients current Z position._

### `y`

_The clients current Y position._

### `MovTime`

_Unused._

The client does not use this value.

### `MoveFlame`

_The clients current animation time._

### `dir`

_The clients current heading direction._

### `TargetMode`

_Unknown._

This flag is sent from the server which the client will reflect back in this packet.

### `RunMode`

_Unknown._

This flag is sent from the server which the client will reflect back in this packet.

### `GroundMode`

_Flag set if the client has any means of wallhack enabled._

This flag is set if the client has any of its various flags set that enable wallhacking. _(`GroundMode`, `HackMove`, etc.)_

### `unused`

_Unused._

### `facetarget`

_The clients current input target index._

### `TimeNow`

_The clients current timestamp._

This timestamp value is returned from `ntTimeNowGet`.

## Additional Information

The client sends this packet to the server each time that the `XiAtelBuff::SendCharPos` method is called and valid conditions are met. _(This is called every frame.)_ This packet will only be sent for the local client entity. The client will check to ensure the following is valid before sending this packet:

  - `entity->Status != 28 && entity->Status != 31` - _Event based status values: ES2, ES5_
  - `(entity->Flags0 & 0x200) != 0` - _Is valid entity/rendering check._
  - `pos.x + pos.y + pos.z != 0.0 && !CliEventUcFlag` - _Valid position detected and not in an event._

The `facetarget` field is only set if the client has a valid main target via:

```cpp
if (PTR_g_pTkInputCtrl)
{
    target_id = CKaTarget::GetTargetCharID(&PTR_g_pTkInputCtrl->m_MainTarget);
    if (target_id)
        pkt->facetarget = target_id->GuideNo;
}
```

Additional checks will happen within this function to determine if the packet will be sent and what data is used to populate certain information. If the local player is in an event, then the client will make use of the event position information from the clients event VM instead of the actual entity position. Otherwise, it makes use of the clients `entity->Movement.LocalPosition` position information. When the client is in a valid animation state, it will increment the AnimationTime value of the entity and use its previous value as the value to populate the `MoveFlame` value.
