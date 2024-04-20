# `GP_CLI_COMMAND_CAMP`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_CAMP` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x00E8` |
| **Size**                  | `0x0008` |

## Description

This packet is sent by the client when requesting to heal. (`/heal`)

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_CAMP
struct GP_CLI_CAMP
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    Mode; // PS2: Mode
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_CAMP`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Mode`

_The packet mode._

This value is used to indicate the mode of the command. _(Toggle, On, Off)_

| Mode | Purpose |
| --- | --- |
| `0x00` | _Mode: (Toggle)_ |
| `0x01` | _Mode: `on`_ |
| `0x02` | _Mode: `off`_ |
