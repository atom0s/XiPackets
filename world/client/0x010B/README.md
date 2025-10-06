# `GP_CLI_COMMAND_BAZAAR_CLOSE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_BAZAAR_CLOSE` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x010B` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when entering the bazaar 'Set Prices' menu.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_BAZAAR_CLOSE
struct GP_CLI_BAZAAR_CLOSE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    AllListClearFlg; // PS2: AllListClearFlg
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_BAZAAR_CLOSE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `AllListClearFlg`

_Unknown._

The client always sets this value to `0`.

## Additional Information

This packet is sent by the client each time it enters the `Set Prices` bazaar menu. This is used to inform the server that it should hide the clients personal bazaar until they have finished updating their desired pricing.
