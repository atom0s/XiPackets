# `GP_CLI_COMMAND_FAQ_GMPARAM`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_FAQ_GMPARAM` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00D4` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when interacting with the Help Desk system. More specifically, when opening the inner Help Desk menu.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_FAQ_GMPARAM
struct GP_CLI_FAQ_GMPARAM
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    Id;     // PS2: Id
    uint16_t    Option; // PS2: Option
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_FAQ_GMPARAM`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Id`

_The param id._

This value is set to the clients local `FDtGmParam.Id` value. This value auto-increments each time the client opens the inner Help Desk menu.

### `Option`

_The param option._

This value is always set to `0`.

## Additional Information

The client will send this packet each time it opens the inner Help Desk menu. It is used to request the current ticket system information from the server. _(However, the information returned from the server is not really used any longer.)_

For more information regarding the GM Help Desk system, please see the documentation for packets:

  - **Client:** [**0x00D3 (GP_CLI_COMMAND_FAQ_GMCALL)**](/world/client/0x00D3/README.md)
  - **Client:** [**0x00D5 (GP_CLI_COMMAND_ACK_GMMSG)**](/world/client/0x00D5/README.md)
  - **Client:** [**0x00F0 (GP_CLI_COMMAND_RESCUE)**](/world/client/0x00F0/README.md)
  - **Server:** [**0x00B5 (GP_SERV_COMMAND_FAQ_GMPARAM)**](/world/server/0x00B5/README.md)
  - **Server:** [**0x00B6 (GP_SERV_COMMAND_SET_GMMSG)**](/world/server/0x00B6/README.md)
