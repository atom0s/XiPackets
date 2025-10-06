# `GP_CLI_COMMAND_ACK_GMMSG`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_ACK_GMMSG` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00D5` |
| **Size**                  | `0x000C` |

## Description

This packet is sent by the client when deleting a received GM message from the Help Desk system.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_ACK_GMMSG
struct GP_CLI_ACK_GMMSG
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    msgId;  // PS2: msgId
    uint16_t    seqId;  // PS2: seqId
    uint16_t    seqNum; // PS2: seqNum
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_ACK_GMMSG`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `msgId`

_The message id._

This value is always set to `0`.

### `seqId`

_The message sequence id._

The client does not use this value.

### `seqNum`

_The message sequence number._

The client does not use this value.

## Additional Information

For more information regarding the GM Help Desk system, please see the documentation for packets:

  - **Client:** [**0x00D3 (GP_CLI_COMMAND_FAQ_GMCALL)**](/world/client/0x00D3/README.md)
  - **Client:** [**0x00D4 (GP_CLI_COMMAND_FAQ_GMPARAM)**](/world/client/0x00D4/README.md)
  - **Client:** [**0x00F0 (GP_CLI_COMMAND_RESCUE)**](/world/client/0x00F0/README.md)
  - **Server:** [**0x00B5 (GP_SERV_COMMAND_FAQ_GMPARAM)**](/world/server/0x00B5/README.md)
  - **Server:** [**0x00B6 (GP_SERV_COMMAND_SET_GMMSG)**](/world/server/0x00B6/README.md)
