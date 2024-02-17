# `Unknown - Packet: 0x0071`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0071` |
| **Size**                  | `0x00CC` |

## Description

This packet is sent by the server to inform the client of the campaign and colonization map information.

## Packet Layout

The initial layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Mode;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `Mode`

_The packet mode._

This value determines which map information is contained within the packet.

  - `2` - _Campaign Map_
  - `3` - _Colonization Map_

_**Note:** All other mode values are ignored._

## Additional Information: Mode `2`

This layout is used when the packet contains campaign map information:

```cpp
struct campaigncontrolledareas_t
{
    uint32_t Sandoria: 5;
    uint32_t Bastok: 5;
    uint32_t Windurst: 5;
    uint32_t Beastman: 5;
    uint32_t unused: 12;
};

struct campaignnation_t
{
    uint32_t Reconnaissance: 4;
    uint32_t unused: 14;
    uint32_t Morale: 7;
    uint32_t Prosperity: 7;
};

struct campaignnations_t
{
    campaignnation_t Sandoria;
    campaignnation_t Bastok;
    campaignnation_t Windurst;
    campaignnation_t BeastmanOrc;
    campaignnation_t BeastmanQuadav;
    campaignnation_t BeastmanYagudo;
    campaignnation_t BeastmanDarkKindred;
};

struct campaignzone_t
{
    uint32_t    unused00: 1;
    uint32_t    Owner: 3;
    uint32_t    CurrentFortifications: 10;
    uint32_t    CurrentResources: 10;
    uint32_t    Heroism: 8;
    uint8_t     InfluenceSandoria;
    uint8_t     InfluenceBastok;
    uint8_t     InfluenceWindurst;
    uint8_t     InfluenceBeastman;
    uint32_t    MaxFortifications: 10;
    uint32_t    MaxResources: 10;
    uint32_t    unused01: 12;
};

struct packet_t
{
    uint16_t                    id: 9;
    uint16_t                    size: 7;
    uint16_t                    sync;
    uint8_t                     Mode;
    uint8_t                     padding00;
    uint8_t                     Length;
    uint8_t                     padding01;
    uint8_t                     padding02;
    uint8_t                     ZoneOffset;
    uint8_t                     padding03[2];
    int32_t                     AlliedNotes;
    campaigncontrolledareas_t   ControlledAreas;
    campaignnations_t           Nations;
    campaignzone_t              Zones[13];
};
```

### `padding00`

_Padding; unused._

### `Length`

_The length of data, starting at the `padding02` value._

The client does not use this value, it is ignored.

_The `padding01` value may be part of this making it a `uint16_t`, however the client never references/uses it so its type size is unknown._

### `padding01`

_Padding; unused._

### `padding02`

_Padding; unused._

### `ZoneOffset`

_The starting offset that the `Zones` information will contain the information for._

This value is used by the client to determine where to copy the `Zones` information into its local memory. The packet handler expects this to be broken into at least two packets, where the offset should be either `0` or `13`. This is enough space to break the zone information in half properly.

### `padding03`

_Padding; unused._

### `AlliedNotes`

_The local clients current allied notes amount._

### `ControlledAreas`

_The current controlled area overlay information._

### `Nations`

_The current main nation node information._

### `Zones`

_The current zone node information._

## Structure Fields (`campaigncontrolledareas_t`)

### `Sandoria`

_The number of zones currently controlled by San d'Oria._

This has a maximum value of 31.

### `Bastok`

_The number of zones currently controlled by Bastok._

This has a maximum value of 31.

### `Windurst`

_The number of zones currently controlled by Windurst._

This has a maximum value of 31.

### `Beastman`

_The number of zones currently controlled by Beastman forces._

This has a maximum value of 31.

### `unused`

_Unused._

## Structure Fields (`campaignnation_t`)

### `Reconnaissance`

_The nations current reconnaissance level._

This has a maximum value of 15.

### `unused`

_Unused._

### `Morale`

_The nations current morale amount._

This has a maximum value of 100 before overflowing the bar. It's value is converted directly to a float.

### `Prosperity`

_The nations current prosperity amount._

This has a maximum value of 100 before overflowing the bar. It's value is converted directly to a float.

## Structure Fields (`campaignzone_t`)

### `unused00`

_Unused._

### `Owner`

_The current owner (controller) of the zone._

  - `0` - _None (Shows a grey orb.)_
  - `1` - _San d'Oria_
  - `2` - _Bastok_
  - `3` - _Windurst_
  - `4` - _Beastman_

### `CurrentFortifications`

_The zones current fortifications amount._

### `CurrentResources`

_The zones current resources amount._

### `Heroism`

_The zones current heroism amount._

This value is used to fill the bar under the zones node on the map when its hovered. The client converts this value to a float by multiplying it as follows:

  - `val = heroism * 0.0099999998;`

### `InfluenceSandoria`

_The zones current influence from San d'Oria._

The client converts this value to a float by multiplying it as follows:

  - `val = influence * 0.40000001;`

### `InfluenceBastok`

_The zones current influence from Bastok._

The client converts this value to a float by multiplying it as follows:

  - `val = influence * 0.40000001;`

### `InfluenceWindurst`

_The zones current influence from Windurst._

The client converts this value to a float by multiplying it as follows:

  - `val = influence * 0.40000001;`

### `InfluenceBeastman`

_The zones current influence from Beastman forces._

The client converts this value to a float by multiplying it as follows:

  - `val = influence * 0.40000001;`

### `MaxFortifications`

_The zones maximum fortifications amount._

### `MaxResources`

_The zones maximum resources amount._

### `unused01`

_Unused._

## Additional Information: Zones

The `ZoneOffset` value is used to determine which set of zone information is stored within the packet for the map.

  - When `ZoneOffset` is set to `0x00` then the `Zones` data will contain `Southern San d'Oria` to `Beadeaux`.
  - When `ZoneOffset` is set to `0x0D` then the `Zones` data will contain `Crawlers' Nest` to `Throne Room`.

The zones are stored in the following order:

| Index | Zone |
| --- | --- |
| `0`   | _Southern San d'Oria_ |
| `1`   | _East Ronfaure_ |
| `2`   | _Jugner Forst_ |
| `3`   | _Vunkerl Inlet_ |
| `4`   | _Batallia Downs_ |
| `5`   | _La Vaule_ |
| `6`   | _The Eldieme Necropolis_ |
| `7`   | _Bastok Markets_ |
| `8`   | _North Gustaberg_ |
| `9`   | _Grauberg_ |
| `10`  | _Pashhow Marshlands_ |
| `11`  | _Rolanberry Fields_ |
| `12`  | _Beadeaux_ |
| `13`  | _Crawlers' Nest_ |
| `14`  | _Windurst Waters_ |
| `15`  | _West Sarutabaruta_ |
| `16`  | _Fort Karugo-Narugo_ |
| `17`  | _Meriphataud Mountains_ |
| `18`  | _Sauromugue Champaign_ |
| `19`  | _Castle Oztroja_ |
| `20`  | _Garlaige Citadel_ |
| `21`  | _Beaucedine Glacier_ |
| `22`  | _Xarcabard_ |
| `23`  | _Castle Zvahl Baileys_ |
| `24`  | _Castle Zvahl Keep_ |
| `25`  | _Throne Room_ |

## Additional Information: Mode `3`

This layout is used when the packet contains colonization map information:

```cpp
struct coalitionranks_t
{
    uint32_t Pioneers: 4;
    uint32_t Peacekeepers: 4;
    uint32_t Couriers: 4;
    uint32_t Scouts: 4;
    uint32_t Inventors: 4;
    uint32_t Mummers: 4;
    uint32_t unused: 8;
};

struct colonizationzone_t
{
    uint32_t ColonizationRate: 7;
    uint32_t CurrentBivouacs: 3;
    uint32_t MaxBivouacs: 3;
    uint32_t unused: 19;
};

struct colonizationzones_t
{
    colonizationzone_t unused;
    colonizationzone_t YahseHuntingGrounds;
    colonizationzone_t CeizakBattlegrounds;
    colonizationzone_t ForetDeHennetiel;
    colonizationzone_t YorciaWeald;
    colonizationzone_t MorimarBasaltFields;
    colonizationzone_t MarjamiRavine;
    colonizationzone_t KamihrDrifts;
    colonizationzone_t RaKaznar;
};

struct packet_t
{
    uint16_t            id: 9;
    uint16_t            size: 7;
    uint16_t            sync;
    uint8_t             Mode;
    uint8_t             padding00;
    uint8_t             Length;
    uint8_t             padding01;
    uint32_t            unknown00;
    uint32_t            unknown01;
    coalitionranks_t    Ranks;
    colonizationzones_t Zones;
    int32_t             Bayld;
};
```

### `padding00`

_Padding; unused._

### `Length`

_The length of data, starting at the `padding02` value._

The client does not use this value, it is ignored.

_The `padding01` value may be part of this making it a `uint16_t`, however the client never references/uses it so its type size is unknown._


### `padding01`

_Padding; unused._

### `unknown00`

_Unknown._

### `unknown01`

_Unknown._

### `Ranks`

_The current coalition rank information._

### `Zones`

_The current zone colonization information._

### `Bayld`

_The local clients current Bayld amount._


## Structure Fields (`coalitionranks_t`)

### `Pioneers`

_The current rank for the Pioneers' coalition._

### `Peacekeepers`

_The current rank for the Peacekeepers' coalition._

### `Couriers`

_The current rank for the Couriers' coalition._

### `Scouts`

_The current rank for the Scouts' coalition._

### `Inventors`

_The current rank for the Inventors' coalition._

### `Mummers`

_The current rank for the Mummers' coalition._

### `unused`

_Unused._

## Structure Fields (`colonizationzone_t`)

### `ColonizationRate`

_The zones colonization rate percent._

### `CurrentBivouacs`

_The zones current Bivouac amount._

### `MaxBivouacs`

_The zones maximum Bivouac amount._

### `unused`

_Unused._

## Structure Fields (`colonizationzones_t`)

### `unused`

_Unused._

### `YahseHuntingGrounds`

_The colonization zone entry for the Yahse Hunting Grounds, and connecting zones._

### `CeizakBattlegrounds`

_The colonization zone entry for the Caizak Battlegrounds, and connecting zones._

### `ForetDeHennetiel`

_The colonization zone entry for Foret de Hennetiel, and connecting zones._

### `YorciaWeald`

_The colonization zone entry for Yorcia Weald, and connecting zones._

### `MorimarBasaltFields`

_The colonization zone entry for the Morimar Besalt Fields, and connecting zones._

### `MarjamiRavine`

_The colonization zone entry for the Marjami Ravine, and connecting zones._

### `KamihrDrifts`

_The colonization zone entry for the Kamihr Drifts, and connecting zones._

### `RaKaznar`

_The colonization zone entry for Ra'Kaznar, and connecting zones._
