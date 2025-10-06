# `GP_CLI_COMMAND_ITEM_PRESENT`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_ITEM_PRESENT` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0035` |
| **Size**                  | `0x0008` |

## Description

> [!NOTE]
> Deprecated: This packet is no longer used.

> [!NOTE]
> This is a special GM-related packet!

This packet is used with the GM command `//itempresent`.

```cpp
// PS2: GP_CLI_ITEM_PRESENT
struct GP_CLI_ITEM_PRESENT
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     Index;      // PS2: Index
    uint8_t     ReqID;      // PS2: ReqID
    uint8_t     TakeFlg;    // PS2: TakeFlg
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_ITEM_PRESENT`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Index`

_Unknown._

### `ReqID`

_Unknown._

### `TakeFlg`

_Unknown._

## Additional Information

This packets intended purpose is unknown. The command that made use of this packet is no longer part of the retail client. However, the function that sends this packet still exists.
