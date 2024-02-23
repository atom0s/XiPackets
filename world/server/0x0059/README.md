# `GP_SERV_COMMAND_FRIENDPASS`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_FRIENDPASS` |
| **Client Handler**        | `RecvFriendPass` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0059` |
| **Size**                  | `0x0024` |

## Description

This packet is sent by the server when interacting with an NPC that handles world passes.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_FRIENDPASS
struct GP_SERV_FRIENDPASS
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    int32_t     leftNum;    // PS2: leftNum
    int32_t     leftDays;   // PS2: leftDays
    int32_t     passPop;    // PS2: passPop
    char        String[16]; // PS2: String
    char        Type;       // PS2: Type
    char        unknown21;  // PS2: (New; did not exist.)
    uint16_t    padding00;  // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_FRIENDPASS`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `leftNum`

_The number of uses left on the current world pass._

### `leftDays`

_The amount of time (in hours) left on the current world pass._

### `passPop`

_The cost of the world pass._

This value is the amount of gil the world pass will cost to purchase.

### `String`

_The world pass string._

### `Type`

_The packet type._

This value will vary based on the current state of the world pass event.

### `unknown21`

_Unknown._

### `padding00`

_Padding; unused._

## Additional Information

When the client first talks to the world pass vendor, they will be prompted with a menu that allows them to choose between purchasing a world pass, a gold world pass or turning in rewards with a referred friend. This packet is used as a response to those various menu options. The packets `Type` value is used to inform the event what kind of response is being handled for the users selected option.

When choosing to purchase either kind of world pass, the following `Type` values are used:

| Type | Action |
| --- | --- |
| `0x00` | _Exits event; displays: `Currently unable to connect to World Pass Center.` message._ |
| `0x01` | _Exits event; displays the clients current active world pass information._ |
| `0x02` | _Exits event; displays: `You do not have enough gil to obtain one.` message._ |
| `0x03` | _Price validation; prompts to user to confirm their purchase. Displays the price of the world pass._ |

_Additional `Type` values higher than `0x03` will all cause the price validation dialog during this state. The server uses `0x03` for this._

When the client chooses 'Yes' to purchase the world pass, then the `Type` value will be used as follows:

| Type | Action |
| --- | --- |
| `0x00` | _Exits event; displays no message._ |
| `0x01` | _Exits event; displays no message._ |
| `0x02` | _Exits event; displays no message._ |
| `0x03` | _Exits event; displays no message._ |
| `0x04` | _Exits event; displays: `You do not have enough gil to obtain one.` message._ |
| `0x05` | _Exits event; displays: `Currently unable to connect to World Pass Center.` message._ |
| `0x06` | _Exits event; displays the clients world pass information._ |

_Additional `Type` values higher than `0x06` will all exit the event with no message._

## Additional Information: Friend Rewards

_This section is currently undocumented._
