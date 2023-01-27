# Passwords

In several of the packet information pages for the `lobby` server communications, there is a field being used named `passwd`. While the name suggests its a password of some sort, it technically is not. This is a session based 'password' that the client and server use to help ensure validation on the given socket.

This value is generated via the PlayOnline profile server when the client first logs into PlayOnline. A unique token will be generated that this value will then use. Under certain conditions, this value will then be regenerated and sent to the client to adjust for any changes made. When the client makes use of this value inside of any packet that uses it, it is MD5 hashed using the current internal `md5key` state of the `lobby` TCP object.

# Server Ids

Among the various packets, there are several ids that are used to specify different types of data. These are generally used to create unique identifiers for various things such as accounts, characters, servers, etc. Below is information regarding some of the ids you will find in the packet information for the `lobby` server.

## ffxi_id

The `ffxi_id` value is used as overall static unique id attached to the given content id. This value **CANNOT** change, no matter what. This value is generated when the content id is generated and is attached permanently to it. This value is used as an overall unique id when doing any operations on a given content id. Even if the character moves servers, renames, etc. this value will always remain the same once it is initially generated for the content id.

_This value is used (in hex format) as the name of the `USER` folder for the given content id/character. This folder holds the per-character macros, settings, sort orders, etc._

## ffxi_id_world

The `ffxi_id_world` value is the low-word (`uint16_t`) value of the characters in-game server id.

_(See notes below for more information on calculating the characters server id.)_

## ffxi_id_world_tbl

The `ffxi_id_world_tbl` value is the hi-byte (`uint8_t`) value of the characters in-game server id.

_(See notes below for more information on calculating the characters server id.)_

## worldid

The `worldid` value is the server id that the character is registered on.

The following values are valid for retail servers:

  - `0x62` - `Undine`
  - `0x64` - `Bahamut`
  - `0x65` - `Shiva`
  - `0x68` - `Phoenix`
  - `0x69` - `Carbuncle`
  - `0x6A` - `Fenrir`
  - `0x6B` - `Sylph`
  - `0x6C` - `Valefor`
  - `0x6E` - `Leviathan`
  - `0x6F` - `Odin`
  - `0x73` - `Quetzalcoatl`
  - `0x74` - `Siren`
  - `0x77` - `Ragnarok`
  - `0x7A` - `Cerberus`
  - `0x7C` - `Bismarck`
  - `0x7E` - `Lakshmi`
  - `0x7F` - `Asura`

_(See notes below for more information on calculating the characters server id.)_

# Calculating Server Ids

Depending on certain situations / contexts, the characters server id will be built into a different id using the various parts mentioned above. There are several times where the client will switch between the various ids, but for the most part, only one is used for player characters that is attached to their actor.

## Full Server Id

When a characters 'full' server id is generated, it will contain all 3 parts including `ffxi_id_world`, `ffxi_id_world_tbl` and `worldid`. This id is generally only used on the backend side of things such as communications with the PlayOnline services or when looking up local information behind the scenes.

This variant of the server id is generated as follows:

```cpp
// First generate the characters general in-game server id..
uint32_t server_id = ffxi_id_world + (ffxi_id_world_tbl << 16);

// Next, readjust the id with the full values..
server_id = server_id | (((server_id & 0xFFFF0000) | (worldid << 8)) << 8);
```

For example, see the following for how the id is generated:

```cpp
uint32_t ffxi_id_world       = 0x1122;
uint32_t ffxi_id_world_tbl   = 0x0008;
uint32_t worldid             = 0x0077;

// First generate the characters general in-game server id..
uint32_t server_id = ffxi_id_world + (ffxi_id_world_tbl << 16);

// server_id is now: 0x00081122

// Next, readjust the id with the full values..
server_id = server_id | (((server_id & 0xFFFF0000) | (worldid << 8)) << 8);

// server_id is now: 0x08771122
```

## General Server Id

The more common variant of the server id calculations is the general id. This is the normal in-game server id that is attached to player character actors. The only two parts that are used to make this value are `ffxi_id_world` and `ffxi_id_world_tbl`. As shown in the above example, we just need to mimic the first part of the code:

```cpp
uint32_t ffxi_id_world       = 0x1122;
uint32_t ffxi_id_world_tbl   = 0x0008;
uint32_t worldid             = 0x0077;

// First generate the characters general in-game server id..
uint32_t server_id = ffxi_id_world + (ffxi_id_world_tbl << 16);

// server_id is now: 0x00081122
```

# ExCodes (`excode_client`, `excode_server`, `excode_server2`)

The `excode` fields are bit-fields that hold flags stating extended properties that are available.

  - `excode_client` - Holds the available expansions the client has installed.
  - `excode_server` - Holds the available expansions the server has enabled.
  - `excode_server2` - Holds the available features the client has purchased/enabled. _(Security token, wardrobes, etc.)_

## `excode_client`, `excode_server`

As mentioned above, these bit-fields hold the available expansions on either end of the connection. The layout of this field is as follows:

```cpp
struct expansions_t
{
    uint32_t has_final_fantasy_xi : 1;          // Final Fantasy XI (Base Game)
    uint32_t has_rise_of_the_zilart : 1;        // Rise of the Zilart
    uint32_t has_chains_of_promathia : 1;       // Chains of Promathia
    uint32_t has_treasures_of_aht_urhgan : 1;   // Treasures of Aht Urhgan
    uint32_t has_wings_of_the_goddess : 1;      // Wings of the Goddess
    uint32_t has_a_crystalline_prophecy : 1;    // A Crystalline Prophecy (Requires Rise of the Zilart to be enabled!)
    uint32_t has_a_moogle_kupo_detat : 1;       // A Moogle Kupo d'Etat (Requires Rise of the Zilart to be enabled!)
    uint32_t has_a_shantotto_ascension : 1;     // A Shantotto Ascension (Requires Rise of the Zilart to be enabled!)
    uint32_t has_vision_of_abyssea : 1;         // Vision of Abyssea
    uint32_t has_scars_of_abyssea : 1;          // Scars of Abyssea
    uint32_t has_heroes_of_abyssea : 1;         // Heroes of Abyssea
    uint32_t has_seekers_of_adoulin : 1;        // Seekers of Adoulin
    uint32_t has_bit12 : 1;                     // (unused)
    uint32_t has_bit13 : 1;                     // (unused)
    uint32_t has_bit14 : 1;                     // (unused)
    uint32_t has_bit15 : 1;                     // (unused)
    uint32_t has_bit16 : 1;                     // (unused)
    uint32_t has_bit17 : 1;                     // (unused)
    uint32_t has_bit18 : 1;                     // (unused)
    uint32_t has_bit19 : 1;                     // (unused)
    uint32_t has_bit20 : 1;                     // (unused)
    uint32_t has_bit21 : 1;                     // (unused)
    uint32_t has_bit22 : 1;                     // (unused)
    uint32_t has_bit23 : 1;                     // (unused)
    uint32_t has_bit24 : 1;                     // (unused)
    uint32_t has_bit25 : 1;                     // (unused)
    uint32_t has_bit26 : 1;                     // (unused)
    uint32_t has_bit27 : 1;                     // (unused)
    uint32_t has_bit28 : 1;                     // (unused)
    uint32_t has_bit29 : 1;                     // (unused)
    uint32_t has_bit30 : 1;                     // (unused)
    uint32_t has_bit31 : 1;                     // (unused)
};
```

## `excode_server2`

As mentioned above, this bit-field hold the clients paid for / unlocked additional features. The layout of this field is as follows:

```cpp
struct features_t
{
    uint32_t has_security_token : 1;            // The client has a security token linked to their account.
    uint32_t has_bit1 : 1;                      // (Unknown; see notes below.)
    uint32_t has_wardrobe3 : 1;                 // The client has access to Wardrobe 3.
    uint32_t has_wardrobe4 : 1;                 // The client has access to Wardrobe 4.
    uint32_t has_wardrobe5 : 1;                 // The client has access to Wardrobe 5.
    uint32_t has_wardrobe6 : 1;                 // The client has access to Wardrobe 6.
    uint32_t has_wardrobe7 : 1;                 // The client has access to Wardrobe 7.
    uint32_t has_wardrobe8 : 1;                 // The client has access to Wardrobe 8.
    uint32_t has_bit8 : 1;                      // (unused)
    uint32_t has_bit9 : 1;                      // (unused)
    uint32_t has_bit10 : 1;                     // (unused)
    uint32_t has_bit11 : 1;                     // (unused)
    uint32_t has_bit12 : 1;                     // (unused)
    uint32_t has_bit13 : 1;                     // (unused)
    uint32_t has_bit14 : 1;                     // (unused)
    uint32_t has_bit15 : 1;                     // (unused)
    uint32_t has_bit16 : 1;                     // (unused)
    uint32_t has_bit17 : 1;                     // (unused)
    uint32_t has_bit18 : 1;                     // (unused)
    uint32_t has_bit19 : 1;                     // (unused)
    uint32_t has_bit20 : 1;                     // (unused)
    uint32_t has_bit21 : 1;                     // (unused)
    uint32_t has_bit22 : 1;                     // (unused)
    uint32_t has_bit23 : 1;                     // (unused)
    uint32_t has_bit24 : 1;                     // (unused)
    uint32_t has_bit25 : 1;                     // (unused)
    uint32_t has_bit26 : 1;                     // (unused)
    uint32_t has_bit27 : 1;                     // (unused)
    uint32_t has_bit28 : 1;                     // (unused)
    uint32_t has_bit29 : 1;                     // (unused)
    uint32_t has_bit30 : 1;                     // (unused)
    uint32_t has_bit31 : 1;                     // (unused)
};
```

The field `has_bit1` has been observed to be set on new trial accounts. However, the actual usage/purpose is unknown. It does not appear this bit is ever tested/used within the client side of things. So this may just be a flag used on the server side.
