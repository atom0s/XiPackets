# `GP_CLI_COMMAND_GROUP_SOLICIT_RES`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_GROUP_SOLICIT_RES` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0074` |
| **Size**                  | `0x0006` |

## Description

This packet is sent by the client when responding to a party or alliance invite.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_GROUP_SOLICIT_RES
struct GP_CLI_GROUP_SOLICIT_RES
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Res;        // PS2: Res
    uint8_t     padding00;  // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_GROUP_SOLICIT_RES`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Res`

_The party invite response value._

This value is the clients response to the current pending invite.

| Res | Purpose |
| --- | --- |
| `0` | _The client has declined the invite. (`/decline`)_ |
| `1` | _The client has accepted the invite. (`/join`)_ |
