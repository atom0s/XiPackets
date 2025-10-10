# `GP_SERV_COMMAND_GROUP_SOLICIT_NO`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_GROUP_SOLICIT_NO` |
| **Client Handler**        | `RecvGroupSolicitNo` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00DE` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the server to update the local clients party mode. This packet is also used to reset the clients pending party invite status.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_GROUP_SOLICIT_NO
struct GP_SERV_GROUP_SOLICIT_NO
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     Reason;         // PS2: Reason
    uint8_t     padding05[3];   // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_GROUP_SOLICIT_NO`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Reason`

_The party mode update reason._

| Reason | Purpose |
| --- | --- |
| `0` | _Sets the `pGlobalNowZone->GroupSys.Mode` to `GC_GROUP_MODE_SOLICIT_NO_ERR`._ |
| `1` | _Sets the `pGlobalNowZone->GroupSys.Mode` to `GC_GROUP_MODE_SOLICIT_NO_OTHER`._ |
| `2` | _Sets the `pGlobalNowZone->GroupSys.Mode` to `GC_GROUP_MODE_SOLICIT_NO_MEMBER`._ |
| `3` | _Sets the `pGlobalNowZone->GroupSys.Mode` to `GC_GROUP_MODE_SOLICIT_OK_SEND`._ |

If the `Reason` is any other value, the `pGlobalNowZone->GroupSys.Mode` value is set to `GC_GROUP_MODE_SOLICIT_NO_NORMAL`.

### `padding05`

_Padding; unused._

## Additional Information

While the client uses this packet to set the `pGlobalNowZone->GroupSys.Mode`, its actual value and meaning does not matter. This value is not actually used by the client at all. The only function that reads/references it simply discards the value and resets it back to 0 every frame.

However, the client will also use this packet to reset the clients pending party invite status information under certain conditions.

If the `pkt->Reason` value is 0, 1, 2 or 4+ then the client will also reset the following:

  - `pGlobalNowZone->GroupSys.SolicitActIndex = 0;`
  - `pGlobalNowZone->GroupSys.UniqueNo = 0;`
  - `pGlobalNowZone->GroupSys.SolicitKind = 0;`

This will clear any previous pending party invite the client had.
