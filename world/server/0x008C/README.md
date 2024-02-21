# `Unknown - Packet: 0x008C`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x008C` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the server to populate the clients merit information. This packet is also used to update merit information when the client spends merit points.

## Packet Layout

The layout of this packet is the following:

```cpp
struct merit_t
{
    uint16_t    index;
    uint8_t     next;
    uint8_t     count;
};

// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    merit_count;
    uint16_t    padding00;
    merit_t     merits[];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `merit_count`

_The number of entries within the `merits` array._

This value will vary based on the usage of this packet. When the server is populating the clients full merit menu information, then this value will be equal to the number of `merit` entries that are populated, up to a maximum of `61` entries per-packet. The server will send multiple packets in order to populate all the needed information.

When the client spends merit points or removes spent points, then this value will be `1` with the `merit` array entry being the merit the client modified.

### `padding00`

_Padding; unused._

### `merits`

_The array of merit points information._

## Structure Fields (`merit_t`)

### `index`

_The merit index._

_See the additional notes below for more information on this value._

### `next`

_The amount of merits it will cost to upgrade this merit to the next level._

This value will be set to `0` when the merit is maxed out.

### `count`

_The current level of upgrades spent on this merit._

## Additional Information

This packet is used to populate and update the clients merit information. The server will use this packet for multiple uses of the merit points system.

The below sections will break down each of the usages of this packet.

## Additional Information - Usage 1 - Populate Merit Points Menu

The initial and main usage of this packet is to populate the clients merit point menu information.

It is important to note that the server **does not** send every single possible merit point entry to the client. Entries where the client has not spent any points into the merit are not sent. Only merits that have at least 1 merit spent into them will be sent and populated on the client. The client only keeps track of merits that have actual points spent into them as well. _(More info on that later.)_

The server can send a maximum of `61` merit entries per-packet. This means if the client has more than 61 individual merits upgraded, then the server will send multiple packets to fully populate the clients merit information. The server will send as many packets as needed to completely populate the needed information. The packets `merit_count` value will reflect how many entries are within the given packet.

If the client has no merits spent, then the server will not send this packet. The client invalidates and resets its local merit information each time the client zones. Thus it will be empty/unpopulated when no merit information is sent from the server.

## Additional Information - Usage 2 - Raise Merit (Spending Points)

When the client spends merit points to upgrade a merit, the server will send this packet to inform the client of the upgrade being successful.

The packet will contain a single `merit` entry with `merit_count` being set to `1`. The client will then use this information to update its own local merit entries `next` and `count` values to reflect the change.

## Additional Information - Usage 3 - Lower Merit (Removing Points)

When the client removes spent points on a merit, the server will send this packet to inform the client of the downgrade being successful.

The packet will contain a single `merit` entry with `merit_count` being set to `1`. The client will then use this information to update its own local merit entries `next` and `count` values to reflect the change.

However, the client also has a special case that it will handle when the merit has been fully downgraded back to `0`. When this condition happens, the server will set the merit point entry `index` value in the packet to the index value + 1. _(This effectively sets the lowest bit in the value.)_ This tells the client that the merit has no points spent in it anymore and can be removed from the local clients merit storage. As mentioned above, the client only keeps track of merits that have actual points spent in them. This condition is used to remove merits who no longer have points spent in them.

_Any time the client receives this packet, it will loop through the `merit` entries and check the `index` value to see if this bit is set in order to remove the merit entry from its lcoal cache._

## Additional Information - DAT Information & Merit Point Index Values

The merit point information that is sent in this packet aligns directly to the merit point DAT file. The `index` value within the `merit` entries aligns directly to the actual string indexes within the merit point DAT file. With the only exception being when the client has removed all spent points in a merit, then the `index` will be the merits string index + 1. _(Again, setting the lowest bit to 1.)_

These are the DAT files which store the merit point information:

  - JP: `ROM\169\74.DAT` (File Id: `55566`)
  - NA: `ROM\169\75.DAT` (File Id: `55686`)

These files hold all the string information used for the merit points menu. This includes:

  - The merit point category names.
  - The merit point category descriptions.
  - The merit point merit names.
  - The merit point merit descriptions.

The actual merit point indexes currently start at index value: `64` _(Max HP)_

The below tables are the various merit point categories and their respective merits available. The `Index` value is both the index of the merits name string in the DAT file as well as the same index the packet will use for the given merit.

_**Note:** Some merits have been removed or replaced with newer options. This table contains ALL entries from the DAT file that still exist. However, not all of them will be displayed or used any longer._

### Category: `HP / MP`

| Index | Name |
| --- | --- |
| `0x40` | `Max HP` |
| `0x42` | `Max MP` |
| `0x44` | `Maximum Merit Points` |

### Category: `Attributes`

| Index | Name |
| --- | --- |
| `0x80` | `STR` |
| `0x82` | `DEX` |
| `0x84` | `VIT` |
| `0x86` | `AGI` |
| `0x88` | `INT` |
| `0x8A` | `MND` |
| `0x8C` | `CHR` |

### Category: `Combat Skills`

| Index | Name |
| --- | --- |
| `0xC0` | `Hand-to-Hand Skill` |
| `0xC2` | `Dagger Skill` |
| `0xC4` | `Sword Skill` |
| `0xC6` | `Great Sword Skill` |
| `0xC8` | `Axe Skill` |
| `0xCA` | `Great Axe Skill` |
| `0xCC` | `Scythe Skill` |
| `0xCE` | `Polearm Skill` |
| `0xD0` | `Katana Skill` |
| `0xD2` | `Great Katana Skill` |
| `0xD4` | `Club Skill` |
| `0xD6` | `Staff Skill` |
| `0xD8` | `Archery Skill` |
| `0xDA` | `Marksmanship Skill` |
| `0xDC` | `Throwing Skill` |
| `0xDE` | `Guarding Skill` |
| `0xE0` | `Evasion Skill` |
| `0xE2` | `Shield Skill` |
| `0xE4` | `Parrying Skill` |

### Category: `Magic Skills`

| Index | Name |
| --- | --- |
| `0x100` | `Divine Magic Skill` |
| `0x102` | `Healing Magic Skill` |
| `0x104` | `Enhancing Magic Skill` |
| `0x106` | `Enfeebling Magic Skill` |
| `0x108` | `Elemental Magic Skill` |
| `0x10A` | `Dark Magic Skill` |
| `0x10C` | `Summoning Magic Skill` |
| `0x10E` | `Ninjutsu Skill` |
| `0x110` | `Singing Skill` |
| `0x112` | `String Instrument Skill` |
| `0x114` | `Wind Instrument Skill` |
| `0x116` | `Blue Magic Skill` |
| `0x118` | `Geomancy Skill` |
| `0x11A` | `Handbell Skill` |

### Category: `Others`

| Index | Name |
| --- | --- |
| `0x140` | `Enmity Increase` |
| `0x142` | `Enmity Decrease` |
| `0x144` | `Critical Hit Rate` |
| `0x146` | `Enemy Critical Hit Rate` |
| `0x148` | `Spell Interruption Rate` |

### Category: `Warrior`

| Index | Name |
| --- | --- |
| `0x180` | `Berserk Recast` |
| `0x182` | `Defender Recast` |
| `0x184` | `Warcry Recast` |
| `0x186` | `Aggressor Recast` |
| `0x188` | `Double Attack Rate` |
| --- | **Group 2** |
| `0x800` | `Warrior's Charge` |
| `0x802` | `Tomahawk` |
| `0x804` | `Savagery` |
| `0x806` | `Aggressive Aim` |

### Category: `Monk`

| Index | Name |
| --- | --- |
| `0x1C0` | `Focus Recast` |
| `0x1C2` | `Dodge Recast` |
| `0x1C4` | `Chakra Recast` |
| `0x1C6` | `Counter Rate` |
| `0x1C8` | `Kick Attack Rate` |
| --- | **Group 2** |
| `0x840` | `Mantra` |
| `0x842` | `Formless Strikes` |
| `0x844` | `Invigorate` |
| `0x846` | `Penance` |

### Category: `White Mage`

| Index | Name |
| --- | --- |
| `0x200` | `Divine Seal Recast` |
| `0x202` | `Cure Cast Time` |
| `0x204` | `Bar Spell Effect` |
| `0x206` | `Banish Effect` |
| `0x208` | `Regen Effect` |
| --- | **Group 2** |
| `0x880` | `Martyr` |
| `0x882` | `Devotion` |
| `0x884` | `Protectra V` |
| `0x886` | `Shellra V` |
| `0x888` | `Animus Solace` |
| `0x88A` | `Animus Misery` |

### Category: `Black Mage`

| Index | Name |
| --- | --- |
| `0x240` | `Elemental Seal Recast` |
| `0x242` | `Fire Magic Potency` |
| `0x244` | `Ice Magic Potency` |
| `0x246` | `Wind Magic Potency` |
| `0x248` | `Earth Magic Potency` |
| `0x24A` | `Lightning Magic Potency` |
| `0x24C` | `Water Magic Potency` |
| --- | **Group 2** |
| `0x8C0` | `Flare II` |
| `0x8C2` | `Freeze II` |
| `0x8C4` | `Tornado II` |
| `0x8C6` | `Quake II` |
| `0x8C8` | `Burst II` |
| `0x8CA` | `Flood II` |
| `0x8CC` | `Ancient Magic MAB` |
| `0x8CE` | `Ancient Magic MB Dmg.` |
| `0x8D0` | `Elemental Mag. Acc.` |
| `0x8D2` | `Ele. Mag. Debuff Dur.` |
| `0x8D4` | `Ele. Mag. Debuff Pot.` |
| `0x8D6` | `Aspir Abs. Amount` |

### Category: `Red Mage`

| Index | Name |
| --- | --- |
| `0x280` | `Convert Recast` |
| `0x282` | `Fire Magic Accuracy` |
| `0x284` | `Ice Magic Accuracy` |
| `0x286` | `Wind Magic Accuracy` |
| `0x288` | `Earth Magic Accuracy` |
| `0x28A` | `Lightning Magic Accuracy` |
| `0x28C` | `Water Magic Accuracy` |
| --- | **Group 2** |
| `0x900` | `Dia III` |
| `0x902` | `Slow II` |
| `0x904` | `Paralyze II` |
| `0x906` | `Phalanx II` |
| `0x908` | `Bio III` |
| `0x90A` | `Blind II` |
| `0x90C` | `Enfeebling Magic Duration` |
| `0x90E` | `Magic Accuracy` |
| `0x910` | `Enhancing Magic Duration` |
| `0x912` | `Immunobreak Chance` |
| `0x914` | `Enspell Damage` |
| `0x916` | `Accuracy` |

### Category: `Thief`

| Index | Name |
| --- | --- |
| `0x2C0` | `Flee Recast` |
| `0x2C2` | `Hide Recast` |
| `0x2C4` | `Sneak Attack Recast` |
| `0x2C6` | `Trick Attack Recast` |
| `0x2C8` | `Triple Attack Rate` |
| --- | **Group 2** |
| `0x940` | `Assassin's Charge` |
| `0x942` | `Feint` |
| `0x944` | `Aura Steal` |
| `0x946` | `Ambush` |

### Category: `Paladin`

| Index | Name |
| --- | --- |
| `0x300` | `Shield Bash Recast` |
| `0x302` | `Holy Circle Recast` |
| `0x304` | `Sentinel Recast` |
| `0x306` | `Cover Effect Length` |
| `0x308` | `Rampart Recast` |
| --- | **Group 2** |
| `0x980` | `Fealty` |
| `0x982` | `Chivalry` |
| `0x984` | `Iron Will` |
| `0x986` | `Guardian` |

### Category: `Dark Knight`

| Index | Name |
| --- | --- |
| `0x340` | `Souleater Recast` |
| `0x342` | `Arcane Circle Recast` |
| `0x344` | `Last Resort Recast` |
| `0x346` | `Last Resort Effect` |
| `0x348` | `Weapon Bash Recast` |
| --- | **Group 2** |
| `0x9C0` | `Dark Seal` |
| `0x9C2` | `Diabolic Eye` |
| `0x9C4` | `Muted Soul` |
| `0x9C6` | `Desperate Blows Effect` |

### Category: `Beastmaster`

| Index | Name |
| --- | --- |
| `0x380` | `Killer Effects` |
| `0x382` | `Reward Recast` |
| `0x384` | `Call Beast Recast` |
| `0x386` | `Sic Recast` |
| `0x388` | `Tame Recast` |
| --- | **Group 2** |
| `0xA00` | `Feral Howl` |
| `0xA02` | `Killer Instinct` |
| `0xA04` | `Beast Affinity` |
| `0xA06` | `Beast Healer` |

### Category: `Bard`

| Index | Name |
| --- | --- |
| `0x3C0` | `Lullaby Recast` |
| `0x3C2` | `Finale Recast` |
| `0x3C4` | `Minne Effect` |
| `0x3C6` | `Minuet Effect` |
| `0x3C8` | `Madrigal Effect` |
| --- | **Group 2** |
| `0xA40` | `Nightingale` |
| `0xA42` | `Troubadour` |
| `0xA44` | `Foe Sirvente` |
| `0xA46` | `Adventurer's Dirge` |
| `0xA48` | `Con Anima` |
| `0xA4A` | `Con Brio` |

### Category: `Ranger`

| Index | Name |
| --- | --- |
| `0x400` | `Scavenge Effect` |
| `0x402` | `Camouflage Recast` |
| `0x404` | `Sharpshot Recast` |
| `0x406` | `Unlimited Shot Recast` |
| `0x408` | `Rapid Shot Rate` |
| --- | **Group 2** |
| `0xA80` | `Stealth Shot` |
| `0xA82` | `Flashy Shot` |
| `0xA84` | `Snapshot` |
| `0xA86` | `Recycle Rate` |

### Category: `Samurai`

| Index | Name |
| --- | --- |
| `0x440` | `Third Eye Recast` |
| `0x442` | `Warding Circle Recast` |
| `0x444` | `Store TP Effect` |
| `0x446` | `Meditate Recast` |
| `0x448` | `Zanshin Attack Rate` |
| --- | **Group 2** |
| `0xAC0` | `Shikikoyo` |
| `0xAC2` | `Blade Bash` |
| `0xAC4` | `Ikishoten` |
| `0xAC6` | `Overwhelm` |

### Category: `Ninja`

| Index | Name |
| --- | --- |
| `0x480` | `Subtle Blow Effect` |
| `0x482` | `Katon Effect` |
| `0x484` | `Hyoton Effect` |
| `0x486` | `Huton Effect` |
| `0x488` | `Doton Effect` |
| `0x48A` | `Raiton Effect` |
| `0x48C` | `Suiton Effect` |
| --- | **Group 2** |
| `0xB00` | `Sange` |
| `0xB02` | `Ninja Tool Expertise` |
| `0xB04` | `Katon: San` |
| `0xB06` | `Hyoton: San` |
| `0xB08` | `Huton: San` |
| `0xB0A` | `Doton: San` |
| `0xB0C` | `Raiton: San` |
| `0xB0E` | `Suiton: San` |
| `0xB10` | `Yonin Effect` |
| `0xB12` | `Innin Effect` |
| `0xB14` | `Ninjutsu Magic Accuracy` |
| `0xB16` | `Ninjutsu Mag. Atk. Bonus` |

### Category: `Dragoon`

| Index | Name |
| --- | --- |
| `0x4C0` | `Ancient Circle Recast` |
| `0x4C2` | `Jump Recast` |
| `0x4C4` | `High Jump Recast` |
| `0x4C6` | `Super Jump Recast` |
| `0x4C8` | `Spirit Link Recast` |
| --- | **Group 2** |
| `0xB40` | `Deep Breathing` |
| `0xB42` | `Angon` |
| `0xB44` | `Empathy` |
| `0xB46` | `Strafe Effect` |

### Category: `Summoner`

| Index | Name |
| --- | --- |
| `0x500` | `Avatar Physical Accuracy` |
| `0x502` | `Avatar Physical Attack` |
| `0x504` | `Avatar Magical Accuracy` |
| `0x506` | `Avatar Magical Attack` |
| `0x508` | `Summoning Magic Cast Time` |
| --- | **Group 2** |
| `0xB80` | `Meteor Strike` |
| `0xB82` | `Heavenly Strike` |
| `0xB84` | `Wind Blade` |
| `0xB86` | `Geocrush` |
| `0xB88` | `Thunderstorm` |
| `0xB8A` | `Grand Fall` |

### Category: `Blue Mage`

| Index | Name |
| --- | --- |
| `0x540` | `Chain Affinity Recast` |
| `0x542` | `Burst Affinity Recast` |
| `0x544` | `Monster Correlation` |
| `0x546` | `Physical Potency` |
| `0x548` | `Magical Accuracy` |
| --- | **Group 2** |
| `0xBC0` | `Convergence` |
| `0xBC2` | `Diffusion` |
| `0xBC4` | `Enchainment` |
| `0xBC6` | `Assimilation` |

### Category: `Corsair`

| Index | Name |
| --- | --- |
| `0x580` | `Phantom Roll Recast` |
| `0x582` | `Quick Draw Recast` |
| `0x584` | `Quick Draw Accuracy` |
| `0x586` | `Random Deal Recast` |
| `0x588` | `Bust Duration` |
| --- | **Group 2** |
| `0xC00` | `Snake Eye` |
| `0xC02` | `Fold` |
| `0xC04` | `Winning Streak` |
| `0xC06` | `Loaded Deck` |

### Category: `Puppetmaster`

| Index | Name |
| --- | --- |
| `0x5C0` | `Automaton Skills` |
| `0x5C2` | `Maintenance Recast` |
| `0x5C4` | `Repair Effect` |
| `0x5C6` | `Activate Recast` |
| `0x5C8` | `Repair Recast` |
| --- | **Group 2** |
| `0xC40` | `Role Reversal` |
| `0xC42` | `Ventriloquy` |
| `0xC44` | `Fine-Tuning` |
| `0xC46` | `Optimization` |

### Category: `Dancer`

| Index | Name |
| --- | --- |
| `0x600` | `Step Accuracy` |
| `0x602` | `Haste Samba Effect` |
| `0x604` | `Reverse Flourish Effect` |
| `0x606` | `Building Flourish Effect` |
| --- | **Group 2** |
| `0xC80` | `Saber Dance` |
| `0xC82` | `Fan Dance` |
| `0xC84` | `No Foot Rise` |
| `0xC86` | `Closed Position` |

### Category: `Scholar`

| Index | Name |
| --- | --- |
| `0x640` | `Grimoire Recast` |
| `0x642` | `Modus Veritas Duration` |
| `0x644` | `Helix Magic Acc./Atk.` |
| `0x646` | `Max Sublimation` |
| --- | **Group 2** |
| `0xCC0` | `Altruism` |
| `0xCC2` | `Focalization` |
| `0xCC4` | `Tranquility` |
| `0xCC6` | `Equanimity` |
| `0xCC8` | `Enlightenment` |
| `0xCCA` | `Stormsurge` |

### Category: `Weapon Skills`

| Index | Name |
| --- | --- |
| `0x680` | `Shijin Spiral` |
| `0x682` | `Exenterator` |
| `0x684` | `Requiescat` |
| `0x686` | `Resolution` |
| `0x688` | `Ruinator` |
| `0x68A` | `Upheaval` |
| `0x68C` | `Entropy` |
| `0x68E` | `Stardiver` |
| `0x690` | `Blade: Shun` |
| `0x692` | `Tachi: Shoha` |
| `0x694` | `Realmrazer` |
| `0x696` | `Shattersoul` |
| `0x698` | `Apex Arrow` |
| `0x69A` | `Last Stand` |

### Category: `Geomancer`

| Index | Name |
| --- | --- |
| `0x6C0` | `Full Circle Effect` |
| `0x6C2` | `Ecliptic Attrition Recast` |
| `0x6C4` | `Life Cycle Recast` |
| `0x6C6` | `Blaze of Glory Recast` |
| `0x6C8` | `Dematerialize Recast` |
| --- | **Group 2** |
| `0xD40` | `Mending Halation` |
| `0xD42` | `Radial Arcana` |
| `0xD44` | `Curative Recantation` |
| `0xD46` | `Primeval Zeal` |

### Category: `Rune Fencer`

| Index | Name |
| --- | --- |
| `0x700` | `Rune Enchantment Effect` |
| `0x702` | `Vallation Effect` |
| `0x704` | `Lunge Effect` |
| `0x706` | `Pflug Effect` |
| `0x708` | `Gambit Recast` |
| --- | **Group 2** |
| `0xD80` | `Battuta` |
| `0xD82` | `Rayke` |
| `0xD84` | `Inspiration` |
| `0xD86` | `Sleight of Sword` |
