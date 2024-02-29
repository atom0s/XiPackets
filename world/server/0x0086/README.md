# `GP_SERV_COMMAND_GUILD_OPEN`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_GUILD_OPEN` |
| **Client Handlers**       | `YkWndGuildMain::RecvGuildOpen`, `gcRecvGuildOpen` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0086` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the server to respond to the client requesting to open a guild shop menu.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: _GP_GUILD_OPEN
struct _GP_GUILD_OPEN
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Stat;
    uint8_t     padding00[3];
    uint32_t    Time;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`_GP_GUILD_OPEN`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Stat`

_The packet status._

| Stat | Usage |
| --- | --- |
| `0` | _The guild is open._ |
| `1` | _The guild is closed._ |
| `2` | _The guild is closed. (Holiday)_ |

### `padding00`

_Padding; unused._

### `Time`

_The guild time information._

This value is used to hold the guilds open hours and holiday information. The information it holds will depend on the `Stat` value.

## Additional Information

The `Stat` value will be used to determine the state of the guild between being opened or closed for a specific reason. The additional information used from the packet will be based on what `Stat` value is currently set.

### `Stat`: 0 - Guild Open

When the guild is open for business, then the `Stat` value will be set to `0`. The client ignores the rest of the packet data when this condition is met.

### `Stat`: 1 - Guild Closed (After Hours)

When the guild is closed due to being after-hours, then the `Stat` value will be set to `1`. When this happens, the client makes use of the `Time` value for a couple of pieces of information. First, it will check and see if a flag is set within the `Time` value. If it is set, the client will simply hard-exit out of the handler:

```cpp
if (!*reinterpret_cast<uint32_t*>(PTR_g_pYkWndGuildMain + 8) && (pkt->Time & 0x80000000) != 0)
    return 1;
```

If this flag is not set, then the client will continue to processing the `Time` value in order to determine the guilds hours of operation. The client will read the hours from the `Time` value by parsing its bits. First, the initial opening hour is determined by walking the `Time` value bits until the first bit that is set is found. When this is reached, the number of bits walked is the opening hour value. Next, the client will continue to walk the value until it reaches the next unset bit. Once the next unset bit is found, the number of bits walked will be the closing hour value.

To better understand this, here is an example with the bits expanded for a given time value. Let's say the guild shop opens at 5 and closes at 9. The `Time` value for this would be `0x000001E0` _(480)_. Expanded into bits, this would be `b0000_0000_0000_0000_0000_0001_1110_0000`. The client will walk this values bits, starting at the lowest bit, until it reaches the first **set** bit. In this case it'll walk `5` bits until it sees the first **set** bit which it will use as the opening hour value. Next, it will continue to walk the remaining bits, starting at the same place it just left off until it reaches the next **unset** bit. With this example, that means it will walk an additional `4` bits, which in total will be `9` bits. This value is then used as the closing hour of the guild.

Here are some other examples:

  - `0x00003FF8` - `b0000_0000_0000_0000_0011_1111_1111_1000`
    - **Open:** `3`
    - **Close:** `14`
  - `0x0000FFF0` - `b0000_0000_0000_0000_1111_1111_1111_0000`
    - **Open:** `4`
    - **Close:** `16`
  - `0x0000FFF0` - `b0000_0000_0000_0000_1111_1111_1111_0000`
    - **Open:** `10`
    - **Close:** `21`

### `Stat`: 2 - Guild Closed (Weekly Holiday)

When the guild is closed due to being the guilds weekly holiday, then the `Stat` value will be set to `2`. When this happens, the client makes use of the `Time` value to determine the day of the week that is used for the guilds holiday. This value is used to be printed to chat with the closed message.

The holiday message will only be printed when the following conditions are met:

```cpp
if (*reinterpret_cast<uint32_t*>(PTR_g_pYkWndGuildMain + 8) && (pkt->Time & 0x80000000) == 0)
{
    if ((pkt->Time & 0x7FFFFFFF) < 8)
    {
        const auto holiday = pkt->Time & 0x7FFFFFFF;

        // snipped..
    }
}
```

The client will then use the `holiday` value to obtain the day of the week string from the DAT Files.

  - JP: `ROM\165\66.DAT` (File Id: `55638`)
  - NA: `ROM\165\80.DAT` (File Id: `55658`)

### `Stat`: *

All other `Stat` values are treated the same and will simply close any currently open guild shop menu without any message.
