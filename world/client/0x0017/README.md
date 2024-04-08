# `GP_CLI_COMMAND_CHARREQ2`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_CHARREQ2` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0017` |
| **Size**                  | `0x0014` |

## Description

This packet is sent by the client if it is attempting to access an entity that is in an unexpected state.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_CHARREQ2
struct GP_CLI_CHARREQ2
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    ActIndex;   // PS2: ActIndex
    uint16_t    padding00;  // PS2: dammy
    uint32_t    UniqueNo2;  // PS2: UniqueNo2
    uint32_t    UniqueNo3;  // PS2: UniqueNo3
    uint16_t    Flg;        // PS2: Flg
    uint16_t    Flg2;       // PS2: Flg2
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_CHARREQ2`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ActIndex`

_The target index of the entity being requested._

### `padding00`

_Padding; unused._

### `UniqueNo2`

_The server id of the entity; sent from the previous obtained `0x000D` or `0x000E` packet._

### `UniqueNo3`

_The server id of the entity; local last-known server id of the entity for the given target index._

### `Flg`

_The last known `SendFlg` value of the entity; sent from the previous obtained `0x000D` or `0x000E` packet._

### `Flg2`

_The entities `SendFlg` value; sent from the previous obtained `0x000D` or `0x000E` packet._

## Additional Information

This packet is used by the client when informing the server that it has landed up in an invalid state with an entity. Each time the client receives an entity update packet for either players _(`0x000D`)_ or NPCs _(`0x000E`)_ it will do a series of debug checks to ensure that the entity is in a valid state locally and can be properly updated for the client. If the entity is found to be in an invalid / unexpected state, the client will send this packet letting the server know information about the invalid state.

  - The `UniqueNo2` and `Flg2` values are set to the same values that the client just received from the given `0x000D` or `0x000E` packet.
  - The `UniqueNo3` and `Flg` values are set to the clients local information for the given entity.

## Additional Information - Player Entity Usage

When the client receives a player entity packet _(`0x000D`)_, it will perform the following to determine if this packet will need to be sent:

_**Note:** This is pseudo code; not meant to be an exact match._
```cpp
if (pkt->ActIndex >= 2304)
    return 0;

// Obtain the entities character table entry..
const auto chr = zone->CharTbl[pkt->ActIndex];

// Check if the entity is already the expected entity..
if (chr->UniqueNo == pkt->UniqueNo)
{
    // Check if not spawning.. (char)
    if ((chr->CharTbl[pkt->ActIndex].SendFlg & 0x1F) != 0x1F)
    {
        // Check if not spawning.. (packet)
        if ((pkt->SendFlg & 0x1F) != 0x1F)
        {
            // Check if not despawning..
            if (!(pkt->SendFlg == 0x20))
                FUNC_DebugReqCharSend(pkt->ActIndex, pkt->UniqueNo, chr->UniqueNo, chr->SendFlg, pkt->SendFlg);
        }
    }
}

// Check if the entity is set but a different valid server id..
else if (chr->UniqueNo && chr->UniqueNo != -1)
{
    // Check if not spawning.. (packet)
    if (!(pkt->SendFlg == 0x1F))
        FUNC_DebugReqCharSend(pkt->ActIndex, pkt->UniqueNo, chr->UniqueNo, chr->SendFlg, pkt->SendFlg);
}

// snipped..
```

## Additional Information - NPC Entity Usage

When the client receives an entity packet _(`0x000E`)_, it will perform the following to determine if this packet will need to be sent:

_**Note:** This is pseudo code; not meant to be an exact match._
```cpp
if (pkt->ActIndex >= 2304)
    return 0;

// Obtain the entities character table entry..
const auto chr = zone->CharTbl[pkt->ActIndex];

// Determine the SendFlg check value based on the entity SubKind..
auto sflg = 0;
if (pkt->SubKind == 1)
    sflg = 0x17;
else
{
    sflg = 0x07;
    if (pkt->SubKind == 0x07)
        sflg = 0x17;
}

// Check if the entities do not match..
if (chr->UniqueNo != pkt->UniqueNo)
{
    // Check if the local character entity is valid..
    if (chr->UniqueNo && chr->UniqueNo != -1)
    {
        if (pkt->SendFlg != sflg)
            FUNC_DebugReqCharSend(pkt->ActIndex, pkt->UniqueNo, chr->UniqueNo, chr->SendFlg, pkt->SendFlg);
    }
}

if ((chr->SendFlg & sflg) != sflg && (sflg & pkt->SendFlg) != sflg && pkt->SendFlg != 0x20)
    FUNC_DebugReqCharSend(pkt->ActIndex, pkt->UniqueNo, chr->UniqueNo, chr->SendFlg, pkt->SendFlg);

// snipped..
```
