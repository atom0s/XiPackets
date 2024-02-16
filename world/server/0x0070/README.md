# `GP_SERV_COMMAND_COMBINE_INF`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_COMBINE_INF` |
| **Client Handler**        | `gcRecvCombineInfo` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0070` |
| **Size**                  | `0x0030` |

## Description

This packet is sent by the server to inform the client of others synthesis results.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_COMBINE_INF
struct GP_COMBINE_INF
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Result;         // PS2: Result
    int8_t      Grade;          // PS2: Grade
    uint8_t     Count;          // PS2: Count
    uint8_t     padding00;      // PS2: (New; did not exist.)
    uint16_t    ItemNo;         // PS2: ItemNo
    uint16_t    BreakNo[8];     // PS2: BreakNo
    uint16_t    UniqueNo;       // PS2: UniqueNo
    uint16_t    ActIndex;       // PS2: ActIndex
    uint8_t     name[16];       // PS2: Name
    uint8_t     padding01[2];   // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_COMBINE_INF`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `Result`

_The craft result id._

This value determines the result of the craft attempt.

| Result | Type | Meaning |
| --- | --- | --- |
| `0x00` | _Success._ | _Successful synthesis; displays synthesized message._ |
| `0x0C` | _Success._ | _Successful synthesis; displays item obtained message._ |

_**Note:** All other result ids are treated as a failed synth. The client will not display a message about this other than showing what materials the player lost._

When the `Result` value is `0x0C`, the message id the client will use is `205`. When the `Result` value is any other non-zero value, the synthesis is treated as a fail. If any items are lost, the message id to print that will be `204`.  _(See the `Grade` information below for how the DAT information is used for this purpose.)_

### `Grade`

_The synthesis grade._

This value determines the level of success for a synthesis, but is used as the message lookup id to pull the proper message from the DAT file to be printed to the chatlog. The base message starts at id `200`, however the packet value will be adjusted by `200` to allow the value to start from `0`.

| Grade | Message |
| --- | --- |
| `0xFF` | _Used when a synthesis has failed._ |
| `0x00` | `[player_name] synthesized [a/{num}] [item_name][puncuation].` |
| `0x01` | `[player_name] synthesized [a/{num}] [item_name][puncuation].` |
| `0x02` | `[player_name] synthesized [a/{num}] [item_name][puncuation].` |
| `0x03` | `[player_name] synthesized [a/{num}] [item_name][puncuation].` |
| `0x04` | `[player_name] lost [item_name].` |

_**Note:** These messages are passed through the client message parser and will parse out special code tags to properly format the message!_

The messages above are stored in the following DAT files:

  - JP: `ROM/27/75.DAT` (File Id: `7030`)
  - NA: `ROM/27/76.DAT` (File Id: `7031`)

### `Count`

_The count of items created by the synthesis._

This value will generally be `1` for any failed or successful synthesis unless the result actually yields more than 1 item. _(ie. crafting stacks of items.)_

### `padding00`

_Padding; unused._

### `ItemNo`

_The item id of the created item from the synthesis._

### `BreakNo`

_The item ids for any lost items during the synthesis._

This array holds the item ids for any lost materials during the synthesis. The client will use this information to print the players lost materials in the event their synthesis failed.

### `UniqueNo`

_The player server id._

The client does not use this value.

### `ActIndex`

_The player target index._

The client does not use this value.

### `name`

_The player name._

## Additional Information

The information in this packet has little to no validation in the client and is used to directly print messages to the chat log about another players' synthesis results. While this packet does contain the `UniqueNo` and `ActIndex` of the player who the message is for, the client does not use either of these values and instead only uses the `name` value to print who it belongs to. _(**Note:** The `UniqueNo` value in this packet is also truncated from its full usual `uint32_t` value, which is likely why it is now ignored as this would result in invalid server ids for some players.)_
