# `GP_CLI_COMMAND_REQLOGOUT`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_REQLOGOUT` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00E7` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when requesting to logout or shutdown.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_REQLOGOUT
struct GP_CLI_REQLOGOUT
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint16_t    Mode; // PS2: Mode
    uint16_t    Kind; // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_REQLOGOUT`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Mode`

_The packet mode._

This value is used to indicate the mode of the command. _(Toggle, On, Off)_

| Mode | Purpose |
| --- | --- |
| `0x00` | _Mode: (Toggle)_ |
| `0x01` | _Mode: `on` (Used with `/logout on`)_ |
| `0x02` | _Mode: `off` (Used with both `/logout off` and `/shutdown off`)_ |
| `0x03` | _Mode: `on` (Used with: `/shutdown on`)_ |

### `Kind`

_The packet kind._

This value is used to determine which command the client has used to cause this packet to be sent.

| Kind | Purpose |
| --- | --- |
| `0x01` | _Logout (`/logout`)_ |
| `0x03` | _Shutdown (`/shutdown`)_ |
