# `GP_CLI_COMMAND_GROUP_CHANGE2`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_GROUP_CHANGE2` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0077` |
| **Size**                  | `0x0016` |

## Description

This packet is sent by the client when changing group system settings. _(ie. Party, Alliance, Linkshell)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_GROUP_CHANGE2
struct GP_CLI_GROUP_CHANGE2
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint8_t     sName[16];  // PS2: sName
    uint8_t     Kind;       // PS2: Kind
    uint8_t     ChangeKind; // PS2: ChangeKind
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_GROUP_CHANGE2`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `sName`

_The name of the player the command was used with; if one was needed._

### `Kind`

_The packet kind._

This value is used to determine what kind of group is being modified.

| Kind | Purpose |
| --- | --- |
| `0` | _Party_ |
| `1` | _Linkshell (1)_ |
| `2` | _Linkshell (2)_ |
| `5` | _Alliance_ |

### `ChangeKind`

_The packet change kind._

This value is set based on the current command that is being used by the client to manipulate the group.

| ChangeKind | Purpose |
| --- | --- |
| `0` | _The client is requesting to change the party leader. (ie. `/pcmd leader Atomos`)_ |
| `1` | _The client is requesting to change the alliance leader. (ie. `/acmd leader Atomos`)_ |
| `2` | _The client is requesting to change a linkshell members pearl to a sack._ |
| `3` | _The client is requesting to change a linkshell members sack to a pearl._ |
| `4` | _The client is requesting to change the loot distribution to `Quartermaster`. (ie. `/pcmd looter Atomos` or `/acmd looter Atomos`)_ |
| `5` | _The client is requesting to change the loot distribution to `Lottery`._ |
| `6` | _The client is requesting to level sync to a fellow party member._ |
| `7` | _The client is requesting to disable level sync._ |
