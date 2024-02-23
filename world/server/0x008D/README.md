# `Unknown - Packet: 0x008D`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x008D` |
| **Size**                  | `0x0104` |

## Description

This packet is sent by the server to populate the clients job point information. This packet is also used to update job point information when the client spends job points.

## Packet Layout

The layout of this packet is the following:

```cpp
struct jobpoint_t
{
    uint16_t    index: 5;
    uint16_t    job_no: 11;
    uint16_t    next: 10;
    uint16_t    level: 6;
};

// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    jobpoint_t  points[64];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `points`

_The array of job points information._

## Structure Fields (`jobpoint_t`)

The data stored in these entries is bitpacked to conserve space.

### `index`

_The index of the job point._

_See the additional information below._

### `job_no`

_The job id that this entry belongs to._

### `next`

_The amount of points needed to raise the job point to the next level._

### `level`

_The current level of the job point._

## Additional Information

This packet is sent by the server to populate _(and update)_ the clients job point menu information. The server also makes use of packet `0x0063` to populate the clients other job point related information, such as each jobs current capacity points and available job points. This packet is only used to populate the menu entries for each job, showing the current level of each job point upgrade and the required amount of points for the next upgrade level. The server will send multiple packets of this type in order to fully populate the clients menu. Each packet can hold a total of `64` `jobpoint_t` entries, although it will not use all entries. Instead, the server will currently put a maximum of two jobs' point information into each packet. On the client side, there is storage space to hold up to `32` job point entries for up to 24 jobs. Again, not all entries are currently used.

The server will send a total of 11 packets of this type when populating the full menu at this time. _(22 playable jobs, 2 jobs per packet.)_ Since each job can have a maximum of 32 entries, they keep the extra space available in the event they do add more job points at a later date rather than stuff more jobs into a single packet. The data that is stored into each `jobpoint_t` entry is bitpacked to help save space as well. They are able to fit all of the information needed for each job point into a single `uint32_t` value by using bits.

When the client receives this packet, it will loop through the `points` array and check for valid entries. It does this by checking to ensure that there is a any value set for the `index` and `job_no` values. _(**Note:** A `job_no` of 0 is considered invalid, but the client has storage for this index even though it will never be used.)_ The `job_no` is also validated to ensure it is less than 24, ensuring its a valid job id.

For each valid entry, the client will then offset its local `PointsSys.JobPoints` storage array using the `index` and `job_no` values.

  - `auto ptr = &zone->PointsSys.JobPoints[32 * job + (index & 0x1F)];`

The client then rebuilds the local job points storage container using the `next` and `level` values. Rather than directly copying the packets data, it will rebuild the entries into a different format. The clients own local storage formats each entry as:

```cpp
struct pointssys_jobpoint_t
{
    uint16_t level: 6;
    uint16_t next: 10;
};
```

## Additional Information - DAT Information & Job Points Index Values

The job point information that is sent in these packets does align to the data inside of the DAT file. However, it is calculated based on the job id, index and a base offset into the file. Each entry is also offset by 2 to align to the actual job point name string entry. _(**Note:** The client has some weird oddities where it will display some entries out of order from the actual DAT file.)_

These are the DAT files which store the job point information:

  - JP: `ROM\314\61.DAT` (File Id: `55574`)
  - NA: `ROM\314\62.DAT` (File Id: `55694`)

These files hold all the string information used for the merit points menu. This includes:

  - The job point category names.
  - The job point category descriptions.
  - The job point names.
  - The job point descriptions.

_**Note:** Job point gifts are stored in a different file._

The actual job point indexes currently start at index value: `64` _(Mighty Strikes Effect)_

The below tables are the various job point categories and their respective job points available.

### Category: `Warrior`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0x40` | `0` | `Mighty Strikes Effect` |
| `0x42` | `1` | `Berserk Effect` |
| `0x44` | `2` | `Brazen Rush Effect` |
| `0x46` | `3` | `Defender Effect` |
| `0x48` | `4` | `Warcry Effect` |
| `0x4A` | `5` | `Aggressor Effect` |
| `0x4C` | `6` | `Retaliation Effect` |
| `0x4E` | `7` | `Restraint Effect` |
| `0x50` | `8` | `Blood Rage Effect` |
| `0x52` | `9` | `Double Attack Effect` |

### Category: `Monk`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0x80` | `0` | `Hundred Fists Effect` |
| `0x82` | `1` | `Dodge Effect` |
| `0x84` | `2` | `Inner Strength Effect` |
| `0x86` | `3` | `Focus Effect` |
| `0x88` | `4` | `Chakra Effect` |
| `0x8A` | `5` | `Counterstance Effect` |
| `0x8C` | `6` | `Footwork Effect` |
| `0x8E` | `7` | `Perfect Counter Effect` |
| `0x90` | `8` | `Impetus Effect` |
| `0x92` | `9` | `Kick Attacks Effect` |

### Category: `White Mage`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0xC0` | `0` | `Benediction Effect` |
| `0xC2` | `1` | `Divine Seal Effect` |
| `0xC4` | `2` | `Asylum Effect` |
| `0xC6` | `3` | `Magic Accuracy Bonus` |
| `0xC8` | `4` | `Afflatus Solace Effect` |
| `0xCA` | `5` | `Afflatus Misery Effect` |
| `0xCC` | `6` | `Divine Caress Duration` |
| `0xCE` | `7` | `Sacrosanctity Effect` |
| `0xD0` | `8` | `Regen Duration` |
| `0xD2` | `9` | `Bar Spell Effect` |

### Category: `Black Mage`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0x100` | `0` | `Manafont Effect` |
| `0x102` | `1` | `Elemental Seal Effect` |
| `0x104` | `2` | `Subtle Sorcery Effect` |
| `0x106` | `3` | `Magic Burst Damage Bonus` |
| `0x108` | `4` | `Mana Wall Effect` |
| `0x10A` | `5` | `Magic Accuracy Bonus` |
| `0x10C` | `6` | `Enmity Douse Recast` |
| `0x10E` | `7` | `Manawell Effect` |
| `0x110` | `8` | `Magic Burst Enmity Decrease` |
| `0x112` | `9` | `Magic Damage Bonus` |

### Category: `Red Mage`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0x140` | `0` | `Chainspell Effect` |
| `0x142` | `1` | `Convert Effect` |
| `0x144` | `2` | `Stymie Effect` |
| `0x146` | `3` | `Magic Accuracy Bonus` |
| `0x148` | `4` | `Composure Effect` |
| `0x14A` | `5` | `Magic Atk. Bonus` |
| `0x14C` | `6` | `Saboteur Effect` |
| `0x14E` | `7` | `Enfeebling Magic Duration` |
| `0x150` | `8` | `Quick Magic Effect` |
| `0x152` | `9` | `Enhancing Magic Duration` |

### Category: `Thief`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0x180` | `0` | `Perfect Dodge Effect` |
| `0x182` | `1` | `Sneak Attack Effect` |
| `0x184` | `2` | `Larceny Duration` |
| `0x186` | `3` | `Trick Attack Effect` |
| `0x188` | `4` | `Steal Recast` |
| `0x18A` | `5` | `Mug Effect` |
| `0x18C` | `6` | `Despoil Effect` |
| `0x18E` | `7` | `Conspirator Effect` |
| `0x190` | `8` | `Bully Effect` |
| `0x192` | `9` | `Triple Attack Effect` |

### Category: `Paladin`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0x1C0` | `0` | `Invincible Effect` |
| `0x1C2` | `1` | `Holy Circle Effect` |
| `0x1C4` | `2` | `Intervene Effect` |
| `0x1C6` | `3` | `Sentinel Effect` |
| `0x1C8` | `4` | `Shield Bash Effect` |
| `0x1CA` | `5` | `Cover Duration` |
| `0x1CC` | `6` | `Divine Emblem Effect` |
| `0x1CE` | `7` | `Sepulcher Duration` |
| `0x1D0` | `8` | `Palisade Effect` |
| `0x1D2` | `9` | `Enlight Effect` |

### Category: `Dark Knight`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0x200` | `0` | `Blood Weapon Effect` |
| `0x202` | `1` | `Arcane Circle Effect` |
| `0x204` | `2` | `Soul Enslavement Effect` |
| `0x206` | `3` | `Last Resort Effect` |
| `0x208` | `4` | `Souleater Duration` |
| `0x20A` | `5` | `Weapon Bash Effect` |
| `0x20C` | `6` | `Nether Void Effect` |
| `0x20E` | `7` | `Arcane Crest Duration` |
| `0x210` | `8` | `Scarlet Delirium Duration` |
| `0x212` | `9` | `Endark Effect` |

### Category: `Beastmaster`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0x240` | `0` | `Familiar Effect` |
| `0x242` | `1` | `Pet Accuracy Bonus` |
| `0x244` | `2` | `Unleash Effect` |
| `0x246` | `3` | `Charm Success Rate` |
| `0x248` | `4` | `Reward Effect` |
| `0x24A` | `5` | `Pet Attack Speed` |
| `0x24C` | `6` | `Ready Effect` |
| `0x24E` | `7` | `Spur Effect` |
| `0x250` | `8` | `Run Wild Duration` |
| `0x252` | `9` | `Pet Magic Accuracy Bonus` |

### Category: `Bard`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0x280` | `0` | `Soul Voice Effect` |
| `0x282` | `1` | `Minne Effect` |
| `0x284` | `2` | `Clarion Call Effect` |
| `0x286` | `3` | `Minuet Effect` |
| `0x288` | `4` | `Pianissimo Effect` |
| `0x28A` | `5` | `Song Accuracy Bonus` |
| `0x28C` | `6` | `Tenuto Effect` |
| `0x28E` | `7` | `Lullaby Duration` |
| `0x290` | `8` | `Marcato Effect` |
| `0x292` | `9` | `Requiem Effect` |

### Category: `Ranger`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0x2C0` | `0` | `Eagle Eye Shot Effect` |
| `0x2C2` | `1` | `Sharpshot Effect` |
| `0x2C4` | `2` | `Overkill Effect` |
| `0x2C6` | `3` | `Camouflage Effect` |
| `0x2C8` | `4` | `Barrage Effect` |
| `0x2CA` | `5` | `Shadowbind Duration` |
| `0x2CC` | `6` | `Velocity Shot Effect` |
| `0x2CE` | `7` | `Double Shot Effect` |
| `0x2D0` | `8` | `Decoy Shot Effect` |
| `0x2D2` | `9` | `Unlimited Shot Effect` |

### Category: `Samurai`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0x300` | `0` | `Meikyo Shisui Effect` |
| `0x302` | `1` | `Warding Circle Effect` |
| `0x304` | `2` | `Yaegasumi Effect` |
| `0x306` | `3` | `Hasso Effect` |
| `0x308` | `4` | `Meditate Effect` |
| `0x30A` | `5` | `Seigan Effect` |
| `0x30C` | `6` | `Konzen-ittai Effect` |
| `0x30E` | `7` | `Hamanoha Duration` |
| `0x310` | `8` | `Hagakure Effect` |
| `0x312` | `9` | `Zanshin Effect` |

### Category: `Ninja`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0x340` | `0` | `Mijin Gakure Effect` |
| `0x342` | `1` | `Yonin Effect` |
| `0x344` | `2` | `Mikage Effect` |
| `0x346` | `3` | `Innin Effect` |
| `0x348` | `4` | `Ninjutsu Accuracy Bonus` |
| `0x34A` | `5` | `Ninjutsu Cast Time` |
| `0x34C` | `6` | `Futae Effect` |
| `0x34E` | `7` | `Elemental Ninjutsu Effect` |
| `0x350` | `8` | `Issekigan Effect` |
| `0x352` | `9` | `Tactical Parry Effect` |

### Category: `Dragoon`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0x380` | `0` | `Spirit Surge Effect` |
| `0x382` | `1` | `Ancient Circle Effect` |
| `0x384` | `2` | `Fly High Effect` |
| `0x386` | `3` | `Jump Effect` |
| `0x388` | `4` | `Spirit Link Effect` |
| `0x38A` | `5` | `Wyvern Max HP Bonus` |
| `0x38C` | `6` | `Dragon Breaker Duration` |
| `0x38E` | `7` | `Wyvern Breath Effect` |
| `0x390` | `8` | `High Jump Effect` |
| `0x392` | `9` | `Wyvern Attr. Increase Effect` |

### Category: `Summoner`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0x3C0` | `0` | `Astral Flow Effect` |
| `0x3C2` | `1` | `Avatar/Spirit Accuracy Bonus` |
| `0x3C4` | `2` | `Astral Conduit Effect` |
| `0x3C6` | `3` | `Avatar/Spirit Mag. Acc. Bonus` |
| `0x3C8` | `4` | `Elemental Siphon Effect` |
| `0x3CA` | `5` | `Avatar/Spirit Physical Attack` |
| `0x3CC` | `6` | `Mana Cede Effect` |
| `0x3CE` | `7` | `Avatar's Favor Effect` |
| `0x3D0` | `8` | `Avatar/Spirit Mag. Damage` |
| `0x3D2` | `9` | `Blood Pact Damage` |

### Category: `Blue Mage`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0x400` | `0` | `Azure Lore Effect` |
| `0x402` | `1` | `Blue Magic Point Bonus` |
| `0x404` | `2` | `Unbridled Wisdom Effect` |
| `0x406` | `3` | `Burst Affinity Bonus` |
| `0x408` | `4` | `Chain Affinity Effect` |
| `0x40A` | `5` | `Phys. Blue Magic Effect Acc.` |
| `0x40C` | `6` | `Unbridled Learning Effect` |
| `0x40E` | `7` | `Unbridled Learning Effect II` |
| `0x410` | `8` | `Efflux Effect` |
| `0x412` | `9` | `Magic Accuracy Bonus` |

### Category: `Corsair`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0x440` | `0` | `Wild Card Effect` |
| `0x442` | `1` | `Phantom Roll Duration` |
| `0x444` | `2` | `Cut Card Effect` |
| `0x446` | `3` | `Bust Evasion` |
| `0x448` | `4` | `Quick Draw Effect` |
| `0x44A` | `5` | `Ammo Consumption` |
| `0x44C` | `6` | `Random Deal Effect` |
| `0x44E` | `7` | `Ranged Accuracy Bonus` |
| `0x450` | `8` | `Triple Shot Effect` |
| `0x452` | `9` | `Optimal Range Effect` |

### Category: `Puppetmaster`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0x480` | `0` | `Overdrive Effect` |
| `0x482` | `1` | `Automaton HP and MP Bonus` |
| `0x484` | `2` | `Heady Artifice Effect` |
| `0x486` | `3` | `Activate Effect` |
| `0x488` | `4` | `Repair Effect` |
| `0x48A` | `5` | `Deus Ex Automata Recast` |
| `0x48C` | `6` | `Tactical Switch` |
| `0x48E` | `7` | `Cooldown Effect` |
| `0x490` | `8` | `Deactivate Effect` |
| `0x492` | `9` | `Martial Arts Effect` |

### Category: `Dancer`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0x4C0` | `0` | `Trance Effect` |
| `0x4C2` | `1` | `Step Duration` |
| `0x4C4` | `2` | `Grand Pas Effect` |
| `0x4C6` | `3` | `Samba Duration` |
| `0x4C8` | `4` | `Waltz Potency` |
| `0x4CA` | `5` | `Flourish I Effect` |
| `0x4CC` | `6` | `Jig Duration` |
| `0x4CE` | `7` | `Flourish II Effect` |
| `0x4D0` | `8` | `Flourish III Effect` |
| `0x4D2` | `9` | `Contradance Effect` |

### Category: `Scholar`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0x500` | `0` | `Tabula Rasa Effect` |
| `0x502` | `1` | `Light Arts Effect` |
| `0x504` | `2` | `Caper Emissarius Effect` |
| `0x506` | `3` | `Dark Arts Effect` |
| `0x508` | `4` | `Stratagem Effect` |
| `0x50A` | `5` | `Stratagem Effect II` |
| `0x50C` | `6` | `Stratagem Effect III` |
| `0x50E` | `7` | `Stratagem Effect IV` |
| `0x510` | `8` | `Modus Veritas Effect` |
| `0x512` | `9` | `Sublimation Effect` |

### Category: `Geomancer`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0x540` | `0` | `Bolster Effect` |
| `0x542` | `1` | `Life Cycle Effect` |
| `0x544` | `2` | `Widened Compass Effect` |
| `0x546` | `3` | `Blaze of Glory Effect` |
| `0x548` | `4` | `Magic Atk. Bonus` |
| `0x54A` | `5` | `Magic Accuracy Bonus` |
| `0x54C` | `6` | `Dematerialize Duration` |
| `0x54E` | `7` | `Theurgic Focus Effect` |
| `0x550` | `8` | `Concentric Pulse Effect` |
| `0x552` | `9` | `Indicolure Spell Effect Dur.` |

### Category: `Rune Fencer`

| DAT Index | Packet Index | Name |
| --- | --- | --- |
| `0x580` | `0` | `Elemental Sforzo Effect` |
| `0x582` | `1` | `Rune Enchantment Effect` |
| `0x584` | `2` | `Odyllic Subterfuge Effect` |
| `0x586` | `3` | `Vallation Duration` |
| `0x588` | `4` | `Swordplay Effect` |
| `0x58A` | `5` | `Swipe Effect` |
| `0x58C` | `6` | `Embolden Effect` |
| `0x58E` | `7` | `Vivacious Pulse` |
| `0x590` | `8` | `One for All Effect Duration` |
| `0x592` | `9` | `Gambit Effect Duration` |
