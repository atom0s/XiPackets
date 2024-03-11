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
    uint32_t WAR        : 1;
    uint32_t MNK        : 1;
    uint32_t WHM        : 1;
    uint32_t BLM        : 1;
    uint32_t RDM        : 1;
    uint32_t THF        : 1;
    uint32_t PLD        : 1;
    uint32_t DRK        : 1;
    uint32_t BST        : 1;
    uint32_t BRD        : 1;
    uint32_t RNG        : 1;
    uint32_t SAM        : 1;
    uint32_t NIN        : 1;
    uint32_t DRG        : 1;
    uint32_t SMN        : 1;
    uint32_t BLU        : 1;
    uint32_t COR        : 1;
    uint32_t PUP        : 1;
    uint32_t DNC        : 1;
    uint32_t SCH        : 1;
    uint32_t GEO        : 1;
    uint32_t RUN        : 1;
    uint32_t unused     : 10;
};

// PS2: (New; did not exist.)
struct chairemotes_t
{
    uint16_t Chair1     : 1; // Chair: Imperial Chair
    uint16_t Chair2     : 1; // Chair: Decorative Chair
    uint16_t Chair3     : 1; // Chair: Ornate Stool
    uint16_t Chair4     : 1; // Chair: Refined Chair
    uint16_t Chair5     : 1; // Chair: Portable Container
    uint16_t Chair6     : 1; // Chair: Chocobo Chair
    uint16_t Chair7     : 1; // Chair: Ephramadian Throne
    uint16_t Chair8     : 1; // Chair: Shadow Throne
    uint16_t Chair9     : 1; // Chair: Leaf Bench
    uint16_t Chair10    : 1; // Chair: Astral Cube
    uint16_t Chair11    : 1; // Chair: Chocobo Chair II
    uint16_t unused     : 5;
};

// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t        id: 9;
    uint16_t        size: 7;
    uint16_t        sync;
    jobemotes_t     JobEmotes;
    chairemotes_t   Chairs;
    uint16_t        padding00;
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

## Additional Information

When the client opens the `Main Menu -> Communication` menu, it will send the server a `0x0119` packet requesting the clients available unlocked job emotes and chairs. The server will respond with this packet which the client will then use to be able to properly populate the sub-menus for either emote when viewed within this sub-menu.
