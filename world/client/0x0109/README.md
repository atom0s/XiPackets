# `GP_CLI_COMMAND_BAZAAR_OPEN`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_BAZAAR_OPEN` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0109` |
| **Size**                  | `0x0004` |

## Description

This packet is sent by the client when exiting the bazaar 'Set Prices' menu with at least one item having a sale price set.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_BAZAAR_OPEN
struct GP_CLI_BAZAAR_OPEN
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_BAZAAR_OPEN`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

## Additional Information

The client sends this packet when exiting the bazaar 'Set Prices' menu after the client has set a price on at least one item. If the client unsets all prices then exits the menu, this packet will not be sent. _(The client makes use of packet `0x010A` to update a bazaar items price.)_
