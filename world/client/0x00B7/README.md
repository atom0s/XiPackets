# `Packet: 0x00B7`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00B7` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the client when interacting with the newer Assist channel mentor features. _(Thumbs Up, Warning, Mute, Unmute)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Kind;
    uint8_t     unknown00;
    uint8_t     sName[15];
    uint8_t     Mes[];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Kind`

_The packet kind._

| Kind | Purpose |
| --- | --- |
| `0x24` | _Give Thumbs Up_ |
| `0x25` | _Issue Warning_ |
| `0x26` | _Add To Mute List_ |
| `0x27` | _Remove From Mute List_ |

### `unknown00`

_Unknown._

This value is always set to `0`.

### `sName`

_The name of the player the action is being performed against._

### `Mes`

_The message string._

This value is always set to a single space character. _(`0x20`)_

## Additional Information

This packet is used by the client when interacting with the newer Assist channel sub menus and similar related menus, such as the one shown with the newer `/mutelist` command. These new chat channels allow players to help each other while also allowing for self-moderation where players can rate moderators and moderators can warn unhelpful players or even mute them from the assist channels for 24 hours.

There are four sub-commands that can be used with these menus which are the `Kind` values shown above:

  - `Give Thumbs Up` - _Used by players (and mentors) to say thanks to others being helpful._
  - `Issue Warning` - _Used by players (and mentors) to warn unhelpful players._
  - `Add To Mute List` - _Used by mentors to mute others from speaking in the Assist channels for 24 hours._
  - `Remove From Mute List` - _Used by mentors to remove a muted player from the list before the 24 hour period._

To assist with bias as well as to prevent abuse, there are limitations set in place with this system:

  - `Normal Players`
    - May issue a single thumbs up and warning per day.
    - Cannot mute others.
  - `Mentors`
    - May issue multiple thumbs up and warnings per day.
    - May only receive 3 total evaluations per day.
    - Can mute other players. _(Must be rank 6 mentor or above!)_
      - _Mutes must be issued within 10 minutes of the targeted players last message!_
