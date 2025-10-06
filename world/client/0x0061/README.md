# `GP_CLI_COMMAND_CLISTATUS`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_CLISTATUS` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0061` |
| **Size**                  | `0x0006` |

## Description

This packet is sent by the client when requesting the current clients local player information.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_CLISTATUS
struct GP_CLI_CLISTATUS
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     unknown00; // PS2: (New; did not exist.)
    uint8_t     padding00; // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_CLISTATUS`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `unknown00`

_Unknown._

This value is used as a flag. The client will use the values `0` or `1` when using this value depending on when this packet is sent.

### `padding00`

_Padding; unused._

## Additional Information

The client uses this packet to request the current players information. The client will send this packet during certain conditions in order to initialize/populate the client player information or to update it.

When the client first enters a zone, and has prepared the needed local information, it will send an `0x000C` packet and then send this packet afterward. This is used as the first initialization request for the information that the server will respond with. Afterward, the client will also send this packet when doing certain actions.

  - When the client opens the `Status` menu. _(via `Main Menu > Status`)_
    - _Sends this packet with `unknown00` set to `0`._
  - When the client opens the `Equipment` menu. _(via `CTRL+E` or `Main Menu > Equipment`)_
    - _Sends this packet with `unknown00` set to `1` first._
    - _Sends this packet with `unknown00` set to `0` afterward._
  - When the client opens the `Quests` menu.
    - _Sends this packet with `unknown00` set to `0`._
  - When the client opens the `Search > Level` menu. _(via `Main Menu > Search > Level`)_
    - _Sends this packet with `unknown00` set to `0`._

_**Note:** Some zones will trigger this packet more often, such as zoning into a Mog House._
