# `GP_SERV_COMMAND_EFFECT`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_EFFECT` |
| **Client Handler**        | `RecvEffect` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0030` |
| **Size**                  | `0x0010` |

## Description

This packet is sent by the server to inform the client to of an entity status change.

_This handler is specifically used for setting an entities crafting animation._

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_EFFECT
struct GP_SERV_EFFECT
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    UniqueNo;   // PS2: UniqueNo
    uint16_t    ActIndex;   // PS2: ActIndex
    int16_t     EffectNum;  // PS2: effectnum
    int8_t      Type;       // PS2: type
    int8_t      Status;     // PS2: status
    uint16_t    Timer;      // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_EFFECT`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The entity server id._

### `ActIndex`

_The entity target index._

### `EffectNum`

_The effect number._

This value is used to set the entities `CraftAnimationEffect` value.

### `Type`

_The effect type._

This value is used to set the entities `CraftParam` value.

### `Status`

_The entity server status._

This value is used to set the entities `StatusServer` value.

### `Timer`

_The synthesis timer value._

This value is used to set the entities `CraftTimer` value.
