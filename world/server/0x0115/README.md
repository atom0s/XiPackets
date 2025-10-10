# `Unknown - Packet: 0x0115`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0115` |
| **Size**                  | `0x0018` |

## Description

This packet is sent by the server to begin the fishing mini-game within the client. This packet holds the needed information to tell the client how to fight the hooked fish.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    stamina;        // PS2: (New; did not exist.)
    uint16_t    arrow_delay;    // PS2: (New; did not exist.)
    uint16_t    regen;          // PS2: (New; did not exist.)
    uint16_t    move_frequency; // PS2: (New; did not exist.)
    uint16_t    arrow_damage;   // PS2: (New; did not exist.)
    uint16_t    arrow_regen;    // PS2: (New; did not exist.)
    uint16_t    time;           // PS2: (New; did not exist.)
    uint8_t     angler_sense;   // PS2: (New; did not exist.)
    uint8_t     padding13;      // PS2: (New; did not exist.)
    uint32_t    intuition;      // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `stamina`

_The amount of health the fish starts with._

This value is used as the fishes initial (and maximum) amount of health. While fishing, this value will be used to clamp the fishes health from exceeding its original maximum each time the fish is healed either from a failed arrow press, or the normal regen tick.

### `arrow_delay`

_The amount of time the player has to properly interact with the arrow mini-game._

If this value is 0, the client will default to 1 instead. While fishing, this value is used to calculate the length of time the client has to react to an arrow press. This calculation is performed as the following:

```cpp
auto val = 10 * (::rand() % (2 * fish->arrow_delay) + 2);
```

_This value will also be adjusted afterward based on the clients intuition._

### `regen`

_The amount of health the fish will regenerate each tick._

This value is used to heal the fish over time, each tick of the mini-game is played out. This allows the fish to regain its strength over time if the player is not actively trying to catch it. When this packet is received, the regen amount is calculated and stored as:

```cpp
fish->regen = pkt->regen - 128;
```

Then while fishing, the amount the fish will regen is performed as:

```cpp
fish->stamina_current = frame_diff * fish->regen;
```

### `move_frequency`

_The frequency in which the fish will 'thrash' about in the water, moving left or right._

If this value is 0, the client will default to 1 instead. When this packet is received, the frequency is calculated and stored as:

```cpp
fish->move_frequency = 20 * pkt->move_frequency;
```

While fishing, the movement frequency is handled as:

```cpp
move_freq = fish->move_frequency * frame_calc;
```

If the calculated `move_freq` value is `< 6000`, then the client will ignore the movement attempt from the fish and simply tick to the next frame of the mini-game.

_This value will also be adjusted afterward based on the clients intuition._

### `arrow_damage` & `arrow_regen`

_The amount of damage the fish will take (or regen) on a successful (or non-successful) arrow press respectively._

The `arrow_damage` and `arrow_regen` values are used to deal damage (or heal) the fish when the player interacts with the arrows during the fishing mini-game. If the player successfully presses the correct arrow, then the client will deal damage _(using `arrow_damage`)_ to the fish. If the player fails a proper button press, the client will then heal the fish instead, using the `arrow_regen` value.

The amounts the client will use depends on the clients angler sense values. The client will treat these values as follows:

```cpp
if (!correct_arrow_pressed_on_time)
{
    auto dmg = fish->arrow_heal;
    if (FUNC_CheckAnglerSense())
        dmg = dmg * 0.25;
    if (dmg < 1)
        dmg = 1;

    fish->stamina_current += dmg;
}
else
{
    auto dmg = fish->arrow_damage;
    if (FUNC_CheckAnglerSense())
        dmg *= 2;

    fish->stamina_current -= dmg;
}
```

From this, we can see that having good angler sense against the fish will cause the client to deal more damage as well as reduce the amount the fish regens based on the arrow presses.

### `time`

_The amount of time the player has to fight and catch the fish._

This value is used to determine how long the player has to fight and catch the fish on their line. The client will use this value to also determine when to warn the player that they are running out of time and need to hurry up. When this happens, the client will send an update to the server using packet `0x0110` which includes the remaining time and the fishes current stamina. If the client does run out of time, then they will also send another `0x0110` packet informing the server they failed the mini-game.

When the client receives this packet, it will store this time value as:

```cpp
fish->time = 60 * pkt->time;
```

### `angler_sense`

_Flags that represent the players knowledge about the hooked fish._

This value is a set of bits that holds the following values:

```cpp
fish->sense1 = pkt->angler_sense & 1;
fish->sense2 = (pkt->angler_sense >> 1) & 1;
```

The first value (`fish->sense1`) is used to alter the music played during the fishing mini-game. This value is also used to adjust the arrow time when it eventually underflows. It will be recalculated as:

```cpp
fish->arrow_time = 60 * (::rand() % 6 + 5);
```

The second value (`fish->sense2`) is used directly within the packet handler to set the players entity to play the angler sense animation. _(Lightbulb over their head when they first hook the fish.)_

### `padding13`

_Padding; unused._

### `intuition`

_The players fishing intuition._

This value is used to control different aspects of the fishing mini-game; mainly the golden arrows. However, it does also have other uses that affect the mini-game in the players favor.

The first usage of this value is that it is reflected back to the server while the client is reporting the current status of their fishing mini-game. The client will send `0x0110` packets to the server during different times of the mini-game. When the client uses mode 3 of this packet, it will send back this value along with the fishes current stamina.

The next, and main, usage of this value is to calculate the players intuition. The client will do this during the fishing mini-game. This is done as follows:

```cpp
int32_t calculate_fishing_intuition(void)
{
    if (fish)
    {
        auto rval1 = ::rand() % 51;
        if (!rval1)
            rval1 = 1;
        auto rval2 = ::rand() % 51;
        if (!rval2)
            rval2 = 1;

        auto res = fish->intuition / (rval2 + rval1) != 0;
        fish->intuition_flag = res;

        return res;
    }
    else
    {
        // crash (SE derp'd or intentionally hard-coded a crash here..)
    }

    return 0;
}
```

The client will then use this `fish->intuition_flag` value in multiple checks during the mini-game. This is used to affect things such as:

  - Lowering the frequency of the fishes movement. _(Left/Right rod changes.)_
  - Increasing the amount of time the player has to react to arrow presses.
  - Increasing the amount of damage done to the fish when successfully pressing an arrow.
  - Decreasing the amount of health recovered by the fish when failing to press an arrow.

_You can see the damage/health differences in the above information for `arrow_damage` and `arrow_regen`._

## Additional Information

This packets information is used to configure and prepare the fishing mini-game system for use. The values from this packet are also populated into two structures the client uses to manage the fishing mini-game as it plays out. It will reference the information stored from this packet to determine how the game should continue playing out as the player is fighting the fish. There are multiple calculations done by the client throughout the mini-game that make use of these values as well as randomness via `rand()` calls.
