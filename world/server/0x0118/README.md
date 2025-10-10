# `Unknown - Packet: 0x0118`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0118` |
| **Size**                  | `0x0094` |

## Description

This packet is sent by the server to update the clients currency information. (2)

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    int32_t     bayld;                          // PS2: (New; did not exist.)
    uint16_t    kinetic_units;                  // PS2: (New; did not exist.)
    uint8_t     coalition_imprimaturs;          // PS2: (New; did not exist.)
    uint8_t     mystical_canteens;              // PS2: (New; did not exist.)
    int32_t     obsidian_fragments;             // PS2: (New; did not exist.)
    uint16_t    lebondopt_wings_stored;         // PS2: (New; did not exist.)
    uint16_t    pulchridopt_wings_stored;       // PS2: (New; did not exist.)
    int32_t     mweya_plasm_corpuscles;         // PS2: (New; did not exist.)
    uint8_t     ghastly_stones_stored;          // PS2: (New; did not exist.)
    uint8_t     ghastly_stones_plus1_stored;    // PS2: (New; did not exist.)
    uint8_t     ghastly_stones_plus2_stored;    // PS2: (New; did not exist.)
    uint8_t     verdigris_stones_stored;        // PS2: (New; did not exist.)
    uint8_t     verdigris_stones_plus1_stored;  // PS2: (New; did not exist.)
    uint8_t     verdigris_stones_plus2_stored;  // PS2: (New; did not exist.)
    uint8_t     wailing_stones_stored;          // PS2: (New; did not exist.)
    uint8_t     wailing_stones_plus1_stored;    // PS2: (New; did not exist.)
    uint8_t     wailing_stones_plus2_stored;    // PS2: (New; did not exist.)
    uint8_t     snowslit_stones_stored;         // PS2: (New; did not exist.)
    uint8_t     snowslit_stones_plus1_stored;   // PS2: (New; did not exist.)
    uint8_t     snowslit_stones_plus2_stored;   // PS2: (New; did not exist.)
    uint8_t     snowtip_stones_stored;          // PS2: (New; did not exist.)
    uint8_t     snowtip_stones_plus1_stored;    // PS2: (New; did not exist.)
    uint8_t     snowtip_stones_plus2_stored;    // PS2: (New; did not exist.)
    uint8_t     snowdim_stones_stored;          // PS2: (New; did not exist.)
    uint8_t     snowdim_stones_plus1_stored;    // PS2: (New; did not exist.)
    uint8_t     snowdim_stones_plus2_stored;    // PS2: (New; did not exist.)
    uint8_t     snoworb_stones_stored;          // PS2: (New; did not exist.)
    uint8_t     snoworb_stones_plus1_stored;    // PS2: (New; did not exist.)
    uint8_t     snoworb_stones_plus2_stored;    // PS2: (New; did not exist.)
    uint8_t     leafslit_stones_stored;         // PS2: (New; did not exist.)
    uint8_t     leafslit_stones_plus1_stored;   // PS2: (New; did not exist.)
    uint8_t     leafslit_stones_plus2_stored;   // PS2: (New; did not exist.)
    uint8_t     leaftip_stones_stored;          // PS2: (New; did not exist.)
    uint8_t     leaftip_stones_plus1_stored;    // PS2: (New; did not exist.)
    uint8_t     leaftip_stones_plus2_stored;    // PS2: (New; did not exist.)
    uint8_t     leafdim_stones_stored;          // PS2: (New; did not exist.)
    uint8_t     leafdim_stones_plus1_stored;    // PS2: (New; did not exist.)
    uint8_t     leafdim_stones_plus2_stored;    // PS2: (New; did not exist.)
    uint8_t     leaforb_stones_stored;          // PS2: (New; did not exist.)
    uint8_t     leaforb_stones_plus1_stored;    // PS2: (New; did not exist.)
    uint8_t     leaforb_stones_plus2_stored;    // PS2: (New; did not exist.)
    uint8_t     duskslit_stones_stored;         // PS2: (New; did not exist.)
    uint8_t     duskslit_stones_plus1_stored;   // PS2: (New; did not exist.)
    uint8_t     duskslit_stones_plus2_stored;   // PS2: (New; did not exist.)
    uint8_t     dusktip_stones_stored;          // PS2: (New; did not exist.)
    uint8_t     dusktip_stones_plus1_stored;    // PS2: (New; did not exist.)
    uint8_t     dusktip_stones_plus2_stored;    // PS2: (New; did not exist.)
    uint8_t     duskdim_stones_stored;          // PS2: (New; did not exist.)
    uint8_t     duskdim_stones_plus1_stored;    // PS2: (New; did not exist.)
    uint8_t     duskdim_stones_plus2_stored;    // PS2: (New; did not exist.)
    uint8_t     duskorb_stones_stored;          // PS2: (New; did not exist.)
    uint8_t     duskorb_stones_plus1_stored;    // PS2: (New; did not exist.)
    uint8_t     duskorb_stones_plus2_stored;    // PS2: (New; did not exist.)
    uint8_t     pellucid_stones_stored;         // PS2: (New; did not exist.)
    uint8_t     fern_stones_stored;             // PS2: (New; did not exist.)
    uint8_t     taupe_stones_stored;            // PS2: (New; did not exist.)
    uint16_t    mellidopt_wings_stored;         // PS2: (New; did not exist.)
    uint16_t    escha_beads;                    // PS2: (New; did not exist.)
    int32_t     escha_silt;                     // PS2: (New; did not exist.)
    int32_t     potpourri;                      // PS2: (New; did not exist.)
    int32_t     hallmarks;                      // PS2: (New; did not exist.)
    int32_t     total_hallmarks;                // PS2: (New; did not exist.)
    int32_t     badges_of_gallantry;            // PS2: (New; did not exist.)
    int32_t     crafter_points;                 // PS2: (New; did not exist.)
    uint8_t     fire_crystals_set;              // PS2: (New; did not exist.)
    uint8_t     ice_crystals_set;               // PS2: (New; did not exist.)
    uint8_t     wind_crystals_set;              // PS2: (New; did not exist.)
    uint8_t     earth_crystals_set;             // PS2: (New; did not exist.)
    uint8_t     lightning_crystals_set;         // PS2: (New; did not exist.)
    uint8_t     water_crystals_set;             // PS2: (New; did not exist.)
    uint8_t     light_crystals_set;             // PS2: (New; did not exist.)
    uint8_t     dark_crystals_set;              // PS2: (New; did not exist.)
    uint8_t     mc_i_sr01s_set;                 // PS2: (New; did not exist.)
    uint8_t     mc_i_sr02s_set;                 // PS2: (New; did not exist.)
    uint8_t     mc_i_sr03s_set;                 // PS2: (New; did not exist.)
    uint8_t     liquefactions_spheres_set;      // PS2: (New; did not exist.)
    uint8_t     induration_spheres_set;         // PS2: (New; did not exist.)
    uint8_t     dentonation_spheres_set;        // PS2: (New; did not exist.)
    uint8_t     scission_spheres_set;           // PS2: (New; did not exist.)
    uint8_t     impaction_spheres_set;          // PS2: (New; did not exist.)
    uint8_t     reverberation_spheres_set;      // PS2: (New; did not exist.)
    uint8_t     transfixion_spheres_set;        // PS2: (New; did not exist.)
    uint8_t     compression_spheres_set;        // PS2: (New; did not exist.)
    uint8_t     fusion_spheres_set;             // PS2: (New; did not exist.)
    uint8_t     distortion_spheres_set;         // PS2: (New; did not exist.)
    uint8_t     fragmentation_spheres_set;      // PS2: (New; did not exist.)
    uint8_t     gravitation_spheres_set;        // PS2: (New; did not exist.)
    uint8_t     light_spheres_set;              // PS2: (New; did not exist.)
    uint8_t     darkness_spheres_set;           // PS2: (New; did not exist.)
    uint8_t     padding7D[3];                   // PS2: (New; did not exist.)
    int32_t     silver_aman_vouchers_stored;    // PS2: (New; did not exist.)
    int32_t     domain_points;                  // PS2: (New; did not exist.)
    int32_t     domain_points_earned_today;     // PS2: (New; did not exist.)
    int32_t     mog_segments;                   // PS2: (New; did not exist.)
    int32_t     gallimaufry;                    // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

## Additional Information

This packet is used to populate the clients `Main Menu -> Status -> Currencies 2` page. _(This packet has too many fields to be listed individually as there is no reason or purpose to list them individually.)_

When the client opens this menu, it will send a request to the server using packet `0x0115` and wait for the response from the server with this packet. The client will then parse the packets contents on-the-fly and display the information immediately. The client does not store this packets information.
