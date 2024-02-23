# `GP_SERV_COMMAND_SWITCH_START`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_SWITCH_START` |
| **Client Handler**        | `RecvStart` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0078` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the server to inform the client when a player has made a proposal. _(via `/nominate` or `/propose`)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    UniqueNo;
    uint32_t    AllNum;
    uint16_t    ActIndex;
    uint8_t     sName[15];
    uint8_t     Kind;
    uint8_t     Str[];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The server id of the player that made the proposal._

### `AllNum`

_Unknown._

The client does not make use of this value.

### `ActIndex`

_The target index of the player that made the proposal._

### `sName`

_The name of the player that made the proposal._

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

### `Str`

_The proposal question and options._

This value is used to hold both the original question and the options that were made available with the proposal. This value is specially formatted to allow the client to be able to properly parse out the question and available options that can be voted for.

  - The question will be surrounded with backets. _(`[`, `]`)_
  - The question will end with a `0x0A` character.
  - The options are formatted as follows:
    - The options are prefixed with a number, starting at `1` and increments.
    - The options are formatted in the following manner: `num:option string`
    - The options are separated with a `0x0A` character.
    - _The last option will **NOT** end with a `0x0A` character!_

_The `Str` value is null terminated and will be 4-byte aligned following the normal FFXI packet handling._
