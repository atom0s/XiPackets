# `Unknown - Packet: 0x0113`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0113` |
| **Size**                  | `0x00FC` |

## Description

This packet is sent by the server to update the clients currency information. (1)

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    int32_t     conquest_points_sandoria;
    int32_t     conquest_points_bastok;
    int32_t     conquest_points_windurst;
    uint16_t    beastmans_seals_stored;
    uint16_t    kindreds_seals_stored;
    uint16_t    kindreds_crests_stored;
    uint16_t    high_kindreds_crests_stored;
    uint16_t    sacred_kindreds_crests_stored;
    uint16_t    ancient_beastcoins_stored;
    uint16_t    valor_points;
    uint16_t    scylds;
    int32_t     guild_points_fishing;
    int32_t     guild_points_woodworking;
    int32_t     guild_points_smithing;
    int32_t     guild_points_goldsmithing;
    int32_t     guild_points_weaving;
    int32_t     guild_points_leathercraft;
    int32_t     guild_points_bonecraft;
    int32_t     guild_points_alchemy;
    int32_t     guild_points_cooking;
    int32_t     cinders;
    uint8_t     synergy_fewell_fire;
    uint8_t     synergy_fewell_ice;
    uint8_t     synergy_fewell_wind;
    uint8_t     synergy_fewell_earth;
    uint8_t     synergy_fewell_lightning;
    uint8_t     synergy_fewell_water;
    uint8_t     synergy_fewell_light;
    uint8_t     synergy_fewell_dark;
    int32_t     ballista_points;
    int32_t     fellow_points;
    uint16_t    chocobucks_sandoria_team;
    uint16_t    chocobucks_bastok_team;
    uint16_t    chocobucks_windurst_team;
    uint16_t    daily_tally;
    int32_t     research_marks;
    uint8_t     wizened_tunnel_worms;
    uint8_t     wizened_morion_worms;
    uint8_t     wizened_phantom_worms;
    uint8_t     unknown67;
    int32_t     moblin_marbles;
    uint16_t    infamy;
    uint16_t    prestige;
    int32_t     legion_points;
    int32_t     sparks_of_eminence;
    int32_t     shining_stars;
    int32_t     imperial_standing;
    int32_t     assault_points_l_sanctum;
    int32_t     assault_points_mjtg;
    int32_t     assault_points_l_cavern;
    int32_t     assault_points_periqia;
    int32_t     assault_points_ilrusi_atoll;
    int32_t     tokens;
    int32_t     zeni;
    int32_t     jettons;
    int32_t     therion_ichor;
    int32_t     allied_notes;
    uint16_t    copper_aman_vouchers_stored;
    uint16_t    login_points;
    int32_t     cruor;
    int32_t     resistance_credits;
    int32_t     dominion_notes;
    uint8_t     echelon_battle_trophies_5th;
    uint8_t     echelon_battle_trophies_4th;
    uint8_t     echelon_battle_trophies_3rd;
    uint8_t     echelon_battle_trophies_2nd;
    uint8_t     echelon_battle_trophies_1st;
    uint8_t     cave_conservation_points;
    uint8_t     imperial_army_id_tags;
    uint8_t     op_credits;
    int32_t     traverser_stones;
    int32_t     voidstones;
    int32_t     kupofrieds_corundums;
    uint8_t     moblin_pheromone_sacks;
    uint8_t     unknownCD;
    uint8_t     rems_tale_chapters_1_stored;
    uint8_t     rems_tale_chapters_2_stored;
    uint8_t     rems_tale_chapters_3_stored;
    uint8_t     rems_tale_chapters_4_stored;
    uint8_t     rems_tale_chapters_5_stored;
    uint8_t     rems_tale_chapters_6_stored;
    uint8_t     rems_tale_chapters_7_stored;
    uint8_t     rems_tale_chapters_8_stored;
    uint8_t     rems_tale_chapters_9_stored;
    uint8_t     rems_tale_chapters_10_stored;
    uint64_t    bloodshed_plans_stored      : 9;
    uint64_t    umbrage_plans_stored        : 9;
    uint64_t    ritualistic_plans_stored    : 9;
    uint64_t    tutelary_plans_stored       : 9;
    uint64_t    primacy_plans_stored        : 9;
    uint64_t    unused                      : 19;
    uint16_t    reclamation_marks;
    uint16_t    padding00;
    int32_t     unity_accolades;
    uint16_t    fire_crystals_stored;
    uint16_t    ice_crystals_stored;
    uint16_t    wind_crystals_stored;
    uint16_t    earth_crystals_stored;
    uint16_t    lightning_crystals_stored;
    uint16_t    water_crystals_stored;
    uint16_t    light_crystals_stored;
    uint16_t    dark_crystals_stored;
    uint16_t    deeds;
    uint16_t    padding01;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

## Additional Information

This packet is used to populate the clients `Main Menu -> Status -> Currencies` page. _(This packet has too many fields to be listed individually as there is no reason or purpose to list them individually.)_

When the client opens this menu, it will send a request to the server using packet `0x010F` and wait for the response from the server with this packet. The client will then parse the packets contents on-the-fly and display the information immediately. The client does not store this packets information.
