# `GP_SERV_COMMAND_DIG`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_DIG` |
| **Client Handler**        | `RecvDig` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x002F` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the server to inform the client to play a digging animation on the given entity.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_DIG
struct GP_SERV_DIG
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    TarUniqueNo;    // PS2: TarUniqueNo
    uint16_t    TarActIndex;    // PS2: TarActIndex
    uint8_t     Flags;          // PS2: (New; did not exist.)
    uint8_t     padding00;      // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_DIG`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `TarUniqueNo`

_The target entity server id._

### `TarActIndex`

_The target entity target index._

### `Flags`

_The flags._

### `padding00`

_Padding; unused._

## Additional Information

The `Flags` value is used to change how a conditional check is done within the packet handler. The conditional check performed looks like this:

```cpp
    if ( (pkt->Flags & 0x0F) == 1 && (FUNC_XiAtelBuff_StatusCheck_IsRidingChocobo(entity, 255) || FUNC_XiAtelBuff_StatusCheck_IsMount(entity, 255)) && (entity->Render.Flags0 & 0x200) != 0 )
    {
        entity->Render.Flags3 |= 0x30;
        return 1;
    }

    entity->Render.Flags3 = entity->Render.Flags3 & 0xFFFFFFCF | 0x20;
```
