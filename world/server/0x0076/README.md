# `Unknown - Packet: 0x0076`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0076` |
| **Size**                  | `0x00F4` |

## Description

This packet is sent by the server to update the clients party members' buff information.

## Packet Layout

The layout of this packet is the following:

```cpp
struct partymemberbuffs_t
{
    uint32_t    UniqueNo;
    uint16_t    ActIndex;
    uint16_t    padding06;
    uint64_t    Bits;
    uint8_t     Buffs[32];
};

// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t            id: 9;
    uint16_t            size: 7;
    uint16_t            sync;

    partymemberbuffs_t  Members[5]; // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Members`

_The buff information for each party member._

This array holds the buff information for the local clients own party members. _(It does not include information for the local clients own buffs!)_

## Structure Fields (`partymemberbuffs_t`)

### `UniqueNo`

_The party member server id._

### `ActIndex`

_The party member target index._

### `padding06`

_Padding; unused._

### `Bits`

_The buff bitmask data._

### `Buffs`

_The buff ids._

## Additional Information

This packet is used to populate the buff information for the clients other party members. The client uses this information to display the party members status icons when they are targeted when the client has the `/statusparty` feature enabled. To be able to store all five party members' buff information in a single packet, the ids are broken into two parts and stored in a manner that causes the ids to be rebuilt on the fly within the client anytime they are accessed.

The packet stores each party members' buff information into two separate values; `Bits` and `Buffs`.

First, the `Buffs` array holds the base lower-byte value of the given members buff ids. However, because the packet is constructed to save space and store all other members buff information, these values are only the lower `uint8_t` of the full id. Next, the `Bits` value holds the remaining bits needed for each buff to rebuild its full id. Each buff has 2 bits within this value that it will use to determine the upper-byte value of the full buff id.

There are a total of 32 buff slots available for each party member entry, with each buff using 2 bits inside of the `Bits` value; totalling 64 bits. Due to this, basic math can be used to determine which byte and set of bits will be used inside of the `Bits` value for a given buffs upper-byte value.

  - To determine which byte inside of the `Bits` value will be used for a given buff, one can calculate that via: `idx >> 2`
  - To determine which bits inside of the selected byte of the `Bits` value are used, one can calculate that via: `2 * (idx % 4)`

The resulting value returned by this placement into the `Bits` value can then be masked by `& 3` to obtain the proper value. The client obtains a full buff id by using the following helper function:

```cpp
int32_t FUNC_Helper_GetPartyMemberBuffByIdx(const partymemberbuffs_t* member, int32_t idx)
{
    if (member->Buffs[idx] == 0xFF && ((*(reinterpret_cast<const uint8_t*>(&member->Bits) + (idx >> 2)) >> (2 * (idx % 4))) & 3) == 0)
        return 0xFFFF;

    const auto val = (*(reinterpret_cast<const uint8_t*>(&member->Bits) + (idx >> 2)) >> (2 * (idx % 4))) & 3;
    return val << 8 | member->Buffs[idx];
}
```

Afterward, when the client is making use of the buff ids returned from this helper function it will ignore all ids that are either `0xFFFF` _(-1)_ or `0`.
