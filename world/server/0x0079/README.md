# `GP_SERV_COMMAND_SWITCH_PROC`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_SWITCH_PROC` |
| **Client Handler**        | `RecvProc` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0079` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the server to inform the client about a current proposal. _(via `/nominate` or `/propose`)_

This packet will be sent when a player has casted a vote in a current proposal or when a current proposal is completed, showing the final results.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    AllNum;
    uint16_t    VoteNumTbl[9];
    uint8_t     Kind;
    uint8_t     State;
    uint8_t     QuestionNum;
    uint8_t     sPropName[15];
    uint8_t     Str[];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `AllNum`

_Unknown._

The client does not make use of this value.

### `VoteNumTbl`

_The proposals current votes for each option._

**Note:** The first entry in this array _(index 0)_ is not used!

### `Kind`

_The chat mode of the proposal._

| Kind | Usage |
| --- | --- |
| `0x01` | _Party_ |
| `0x02` | _Linkshell (1)_ |
| `0x03` | _Linkshell (2)_ |
| `0x04` | _Linkshell (3)_ |
| `0x05` | _Say (All)_ |
| `0x06` | _Shout (All)_ |

_Any other `Kind` value will cause the handler to simply exit without processing the packet._

### `State`

_The proposal state._

This value is used to determine if the proposal is still active or if the voting has concluded.

  `0` - _The proposal is still going. Used for vote updates._
  `2` - _The proposal has ended. Used for final results._

### `QuestionNum`

_The number of proposal options._

This value is the number of options made available in the proposal. Due to the fact that the first entry in the `VoteNumTbl` array is not used, this value is `+1` the number of actual options given by the player. _(ie. If a player makes a proposal with 2 options to pick from, then this value will be 3.)_

### `sPropName`

_The name of the player who made the proposal._

### `Str`

_The proposal question and options._

This value is used to hold both the original question and the options that were made available with the proposal along with the number of votes casted for that option. This value is specially formatted to allow the client to be able to properly parse out the question and available options that can be voted for.

  - The question will be surrounded with backets. _(`[`, `]`)_
  - The question will end with a `0x0A` character.
  - The options are formatted as follows:
    - The options are prefixed with a number, starting at `1` and increments.
    - The options are formatted in the following manner: `num[votes]:option string`
      - _Where `votes` is the total number of votes that the given option received._
    - The options are separated with a `0x0A` character.
    - _The last option will **NOT** end with a `0x0A` character!_

**Note:** This value is only present in this packet when the `State` value is `2`! Otherwise, it will be a null string.

_The `Str` value is null terminated and will be 4-byte aligned following the normal FFXI packet handling._

## Additional Information

This packet is used to handle both current live updates for a proposal as well as the final results display when a proposal has ended. The client determines which type this packet is for based on the `State` value.

### State 0 - Live Results Update

When the `State` value is 0, it means that this packet contains current voting information live as it happens. The `VoteNumTbl` array will be updated with the current tally of votes for each option. When this state is used, the `Str` value will also be null and unused.

The packet will be a fixed `0x30` bytes in length when this state is used.

### State 2 - Final Results

When the `State` value is 2, it means that this packet contains the final results of a proposal. The `VoteNumTbl` array will contain the final results of the voting. The `Str` value will also be present and contain a specially formatted string which holds both the original question of the proposal as well as each option and its total votes.

The packet will be a variable length when this state is used based on the length of the `Str` value.
