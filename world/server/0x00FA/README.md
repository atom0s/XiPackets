# `GP_SERV_COMMAND_MYROOM_OPERATION`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_MYROOM_OPERATION` |
| **Client Handler**        | `RecvOperation` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00FA` |
| **Size**                  | `0x0010` |

## Description

This packet is sent by the server when the client is interacting with furniture or plants in their mog house.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_MYROOM_RESULT
enum GP_MYROOM_RESULT
{
    GP_MYROOM_RESULT_OK,
    GP_MYROOM_RESULT_NG_PLANT_ADD_PARAM,
    GP_MYROOM_RESULT_NG_PLANT_CHECK_PARAM,
    GP_MYROOM_RESULT_NG_PLANT_CORP_PARAM,
    GP_MYROOM_RESULT_NG_PLANT_STOP_PARAM,
    GP_MYROOM_RESULT_NG_LAYOUT_PARAM,
    GP_MYROOM_RESULT_NG_BANKIN_PARAM,
    GP_MYROOM_RESULT_END
};

// PS2: GP_SERV_MYROOM_OPERATION
struct GP_SERV_MYROOM_OPERATION
{
    uint16_t            id: 9;
    uint16_t            size: 7;
    uint16_t            sync;
    uint16_t            MyroomItemNo;   // PS2: MyroomItemNo
    GP_MYROOM_RESULT    Result;         // PS2: Result
    uint16_t            unknown00;      // PS2: (New; did not exist.)
    uint8_t             MyroomItemIndex;// PS2: ItemIndex
    uint8_t             MyroomCategory; // PS2: (New; did not exist.)
    uint16_t            unknown01;      // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_MYROOM_OPERATION`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `MyroomItemNo`

_The item id._

### `Result`

_The interaction result._

### `unknown00`

_Unknown._

### `MyroomItemIndex`

_The item index._

### `MyroomCategory`

_The item container._

### `unknown01`

_Unknown._

## Additional Information

The full layout of this packet is not known and the given information above is simply based on easy-to-determine values. The `Result` field is assumed based on the old PS2 beta information. However, it is not possible to fully determine all values in this packet as the client does not use this packet at all. The handle for this packet is simply a return statement, there is no usage of the actual packet at all. This packet is basically junk that is not needed at this time. There is no state change or handling in any manner regarding this packet, so the client can safely ignore it.

There is no means to fully validate the information of `GP_MYROOM_RESULT`. The provided enumeration values above are from the PS2 beta information.
