# `Unknown - Packet: 0x00AE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00AE` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the server to populate the clients mount information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     MountDataTbl[8];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `MountDataTbl`

_The clients available mount information._

This value is used to populate the clients local `PTR_pGlobalNowZone->MountSys.MountDataTbl` buffer, which is used to hold the clients unlocked and available mounts.

## Additional Information

The client directly copies the information in `MountDataTbl` into its own local buffer `PTR_pGlobalNowZone->MountSys.MountDataTbl`. The client uses this block of information to determine if the player has access to the various castable mounts. This block of data is treated as bits for each available mount. It aligns directly with the mount names DAT file:

  - JP: `ROM\351\82.DAT` (File Id: `55561`)
  - NA: `ROM\351\84.DAT` (File Id: `55681`)

The current list of available mounts is as follows:

| Index | Name |
| --- | --- |
| `0x00` | `Chocobo` |
| `0x01` | `Raptor` |
| `0x02` | `Tiger` |
| `0x03` | `Crab` |
| `0x04` | `Red Crab` |
| `0x05` | `Bomb` |
| `0x06` | `Sheep` |
| `0x07` | `Morbol` |
| `0x08` | `Crawler` |
| `0x09` | `Fenrir` |
| `0x0A` | `Beetle` |
| `0x0B` | `Moogle` |
| `0x0C` | `Magic Pot` |
| `0x0D` | `Tulfaire` |
| `0x0E` | `Warmachine` |
| `0x0F` | `Xzomit` |
| `0x10` | `Hippogryph` |
| `0x11` | `Spectral Chair` |
| `0x12` | `Spheroid` |
| `0x13` | `Omega` |
| `0x14` | `Coeurl` |
| `0x15` | `Goobbue` |
| `0x16` | `Raaz` |
| `0x17` | `Levitus` |
| `0x18` | `Adamantoise` |
| `0x19` | `Dhalmel` |
| `0x1A` | `Doll` |
| `0x1B` | `Golden Bomb` |
| `0x1C` | `Buffalo` |
| `0x1D` | `Wivre` |
| `0x1E` | `Red Raptor` |
| `0x1F` | `Iron Giant` |
| `0x20` | `Byakko` |
| `0x21` | `Noble Chocobo` |
| `0x22` | `Ixion` |
| `0x23` | `Phuabo` |
