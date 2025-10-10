# `Unknown - Packet: 0x011A`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x011A` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the server to update the clients unlocked job emotes (`/jobemote`) and chairs (`/sitchair`). This is requested by the client when opening the `Main Menu > Communication` menu.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct jobemotes_t
{
    uint32_t WAR        : 1;    // Emote: Warrior
    uint32_t MNK        : 1;    // Emote: Monk
    uint32_t WHM        : 1;    // Emote: White Mage
    uint32_t BLM        : 1;    // Emote: Black Mage
    uint32_t RDM        : 1;    // Emote: Red Mage
    uint32_t THF        : 1;    // Emote: Thief
    uint32_t PLD        : 1;    // Emote: Paladin
    uint32_t DRK        : 1;    // Emote: Dark Knight
    uint32_t BST        : 1;    // Emote: Beastmaster
    uint32_t BRD        : 1;    // Emote: Bard
    uint32_t RNG        : 1;    // Emote: Ranger
    uint32_t SAM        : 1;    // Emote: Samurai
    uint32_t NIN        : 1;    // Emote: Ninja
    uint32_t DRG        : 1;    // Emote: Dragoon
    uint32_t SMN        : 1;    // Emote: Summoner
    uint32_t BLU        : 1;    // Emote: Blue Mage
    uint32_t COR        : 1;    // Emote: Corsair
    uint32_t PUP        : 1;    // Emote: Puppetmaster
    uint32_t DNC        : 1;    // Emote: Dancer
    uint32_t SCH        : 1;    // Emote: Scholar
    uint32_t GEO        : 1;    // Emote: Geomancer
    uint32_t RUN        : 1;    // Emote: Rune Fencer
    uint32_t unused     : 10;   // unused
};

// PS2: (New; did not exist.)
struct chairemotes_t
{
    uint16_t Chair1     : 1;    // Chair: Imperial Chair
    uint16_t Chair2     : 1;    // Chair: Decorative Chair
    uint16_t Chair3     : 1;    // Chair: Ornate Stool
    uint16_t Chair4     : 1;    // Chair: Refined Chair
    uint16_t Chair5     : 1;    // Chair: Portable Container
    uint16_t Chair6     : 1;    // Chair: Chocobo Chair
    uint16_t Chair7     : 1;    // Chair: Ephramadian Throne
    uint16_t Chair8     : 1;    // Chair: Shadow Throne
    uint16_t Chair9     : 1;    // Chair: Leaf Bench
    uint16_t Chair10    : 1;    // Chair: Astral Cube
    uint16_t Chair11    : 1;    // Chair: Chocobo Chair II
    uint16_t unused     : 5;    // unused
};

// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t        id: 9;
    uint16_t        size: 7;
    uint16_t        sync;

    jobemotes_t     JobEmotes;  // PS2: (New; did not exist.)
    chairemotes_t   Chairs;     // PS2: (New; did not exist.)
    uint16_t        padding0A;  // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `JobEmotes`

_The bits representing the clients unlocked job emotes._

This set of bits is used to display the unlocked and accessible job emotes within the `/jobemote` emote sub-menu.

### `Chairs`

_The bits representing the clients unlocked chairs._

This set of bits is used to display the unlocked and accessible chairs within the `/sitchair` emote sub-menu.

### `padding0A`

_Padding; unused._

## Additional Information

When the client opens the `Main Menu -> Communication` menu, it will send the server a `0x0119` packet requesting the clients available unlocked job emotes and chairs. The server will respond with this packet which the client will then use to be able to properly populate the sub-menus for either emote when viewed within this sub-menu.
