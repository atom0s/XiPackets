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

    int32_t     conquest_points_sandoria;           // PS2: (New; did not exist.)
    int32_t     conquest_points_bastok;             // PS2: (New; did not exist.)
    int32_t     conquest_points_windurst;           // PS2: (New; did not exist.)
    uint16_t    beastmans_seals_stored;             // PS2: (New; did not exist.)
    uint16_t    kindreds_seals_stored;              // PS2: (New; did not exist.)
    uint16_t    kindreds_crests_stored;             // PS2: (New; did not exist.)
    uint16_t    high_kindreds_crests_stored;        // PS2: (New; did not exist.)
    uint16_t    sacred_kindreds_crests_stored;      // PS2: (New; did not exist.)
    uint16_t    ancient_beastcoins_stored;          // PS2: (New; did not exist.)
    uint16_t    valor_points;                       // PS2: (New; did not exist.)
    uint16_t    scylds;                             // PS2: (New; did not exist.)
    int32_t     guild_points_fishing;               // PS2: (New; did not exist.)
    int32_t     guild_points_woodworking;           // PS2: (New; did not exist.)
    int32_t     guild_points_smithing;              // PS2: (New; did not exist.)
    int32_t     guild_points_goldsmithing;          // PS2: (New; did not exist.)
    int32_t     guild_points_weaving;               // PS2: (New; did not exist.)
    int32_t     guild_points_leathercraft;          // PS2: (New; did not exist.)
    int32_t     guild_points_bonecraft;             // PS2: (New; did not exist.)
    int32_t     guild_points_alchemy;               // PS2: (New; did not exist.)
    int32_t     guild_points_cooking;               // PS2: (New; did not exist.)
    int32_t     cinders;                            // PS2: (New; did not exist.)
    uint8_t     synergy_fewell_fire;                // PS2: (New; did not exist.)
    uint8_t     synergy_fewell_ice;                 // PS2: (New; did not exist.)
    uint8_t     synergy_fewell_wind;                // PS2: (New; did not exist.)
    uint8_t     synergy_fewell_earth;               // PS2: (New; did not exist.)
    uint8_t     synergy_fewell_lightning;           // PS2: (New; did not exist.)
    uint8_t     synergy_fewell_water;               // PS2: (New; did not exist.)
    uint8_t     synergy_fewell_light;               // PS2: (New; did not exist.)
    uint8_t     synergy_fewell_dark;                // PS2: (New; did not exist.)
    int32_t     ballista_points;                    // PS2: (New; did not exist.)
    int32_t     fellow_points;                      // PS2: (New; did not exist.)
    uint16_t    chocobucks_sandoria_team;           // PS2: (New; did not exist.)
    uint16_t    chocobucks_bastok_team;             // PS2: (New; did not exist.)
    uint16_t    chocobucks_windurst_team;           // PS2: (New; did not exist.)
    uint16_t    daily_tally;                        // PS2: (New; did not exist.)
    int32_t     research_marks;                     // PS2: (New; did not exist.)
    uint8_t     wizened_tunnel_worms;               // PS2: (New; did not exist.)
    uint8_t     wizened_morion_worms;               // PS2: (New; did not exist.)
    uint8_t     wizened_phantom_worms;              // PS2: (New; did not exist.)
    uint8_t     unknown67;                          // PS2: (New; did not exist.)
    int32_t     moblin_marbles;                     // PS2: (New; did not exist.)
    uint16_t    infamy;                             // PS2: (New; did not exist.)
    uint16_t    prestige;                           // PS2: (New; did not exist.)
    int32_t     legion_points;                      // PS2: (New; did not exist.)
    int32_t     sparks_of_eminence;                 // PS2: (New; did not exist.)
    int32_t     shining_stars;                      // PS2: (New; did not exist.)
    int32_t     imperial_standing;                  // PS2: (New; did not exist.)
    int32_t     assault_points_l_sanctum;           // PS2: (New; did not exist.)
    int32_t     assault_points_mjtg;                // PS2: (New; did not exist.)
    int32_t     assault_points_l_cavern;            // PS2: (New; did not exist.)
    int32_t     assault_points_periqia;             // PS2: (New; did not exist.)
    int32_t     assault_points_ilrusi_atoll;        // PS2: (New; did not exist.)
    int32_t     tokens;                             // PS2: (New; did not exist.)
    int32_t     zeni;                               // PS2: (New; did not exist.)
    int32_t     jettons;                            // PS2: (New; did not exist.)
    int32_t     therion_ichor;                      // PS2: (New; did not exist.)
    int32_t     allied_notes;                       // PS2: (New; did not exist.)
    uint16_t    copper_aman_vouchers_stored;        // PS2: (New; did not exist.)
    uint16_t    login_points;                       // PS2: (New; did not exist.)
    int32_t     cruor;                              // PS2: (New; did not exist.)
    int32_t     resistance_credits;                 // PS2: (New; did not exist.)
    int32_t     dominion_notes;                     // PS2: (New; did not exist.)
    uint8_t     echelon_battle_trophies_5th;        // PS2: (New; did not exist.)
    uint8_t     echelon_battle_trophies_4th;        // PS2: (New; did not exist.)
    uint8_t     echelon_battle_trophies_3rd;        // PS2: (New; did not exist.)
    uint8_t     echelon_battle_trophies_2nd;        // PS2: (New; did not exist.)
    uint8_t     echelon_battle_trophies_1st;        // PS2: (New; did not exist.)
    uint8_t     cave_conservation_points;           // PS2: (New; did not exist.)
    uint8_t     imperial_army_id_tags;              // PS2: (New; did not exist.)
    uint8_t     op_credits;                         // PS2: (New; did not exist.)
    int32_t     traverser_stones;                   // PS2: (New; did not exist.)
    int32_t     voidstones;                         // PS2: (New; did not exist.)
    int32_t     kupofrieds_corundums;               // PS2: (New; did not exist.)
    uint8_t     moblin_pheromone_sacks;             // PS2: (New; did not exist.)
    uint8_t     unknownCD;                          // PS2: (New; did not exist.)
    uint8_t     rems_tale_chapters_1_stored;        // PS2: (New; did not exist.)
    uint8_t     rems_tale_chapters_2_stored;        // PS2: (New; did not exist.)
    uint8_t     rems_tale_chapters_3_stored;        // PS2: (New; did not exist.)
    uint8_t     rems_tale_chapters_4_stored;        // PS2: (New; did not exist.)
    uint8_t     rems_tale_chapters_5_stored;        // PS2: (New; did not exist.)
    uint8_t     rems_tale_chapters_6_stored;        // PS2: (New; did not exist.)
    uint8_t     rems_tale_chapters_7_stored;        // PS2: (New; did not exist.)
    uint8_t     rems_tale_chapters_8_stored;        // PS2: (New; did not exist.)
    uint8_t     rems_tale_chapters_9_stored;        // PS2: (New; did not exist.)
    uint8_t     rems_tale_chapters_10_stored;       // PS2: (New; did not exist.)
    uint64_t    bloodshed_plans_stored      : 9;    // PS2: (New; did not exist.)
    uint64_t    umbrage_plans_stored        : 9;    // PS2: (New; did not exist.)
    uint64_t    ritualistic_plans_stored    : 9;    // PS2: (New; did not exist.)
    uint64_t    tutelary_plans_stored       : 9;    // PS2: (New; did not exist.)
    uint64_t    primacy_plans_stored        : 9;    // PS2: (New; did not exist.)
    uint64_t    unused                      : 19;   // PS2: (New; did not exist.)
    uint16_t    reclamation_marks;                  // PS2: (New; did not exist.)
    uint16_t    paddingE2;                          // PS2: (New; did not exist.)
    int32_t     unity_accolades;                    // PS2: (New; did not exist.)
    uint16_t    fire_crystals_stored;               // PS2: (New; did not exist.)
    uint16_t    ice_crystals_stored;                // PS2: (New; did not exist.)
    uint16_t    wind_crystals_stored;               // PS2: (New; did not exist.)
    uint16_t    earth_crystals_stored;              // PS2: (New; did not exist.)
    uint16_t    lightning_crystals_stored;          // PS2: (New; did not exist.)
    uint16_t    water_crystals_stored;              // PS2: (New; did not exist.)
    uint16_t    light_crystals_stored;              // PS2: (New; did not exist.)
    uint16_t    dark_crystals_stored;               // PS2: (New; did not exist.)
    uint16_t    deeds;                              // PS2: (New; did not exist.)
    uint16_t    paddingFA;                          // PS2: (New; did not exist.)
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
