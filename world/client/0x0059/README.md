# `GP_CLI_COMMAND_EFFECTEND`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_EFFECTEND` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0059` |
| **Size**                  | `0x0010` |

## Description

This packet is sent by the client when crafting.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_EFFECTEND
struct GP_CLI_EFFECTEND
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    effectpara;     // PS2: effectpara
    uint8_t     padding00[8];   // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_EFFECTEND`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `effectpara`

_The effect parameter._

The client uses this value to inform the server of its local attempt to playout the crafting animations. When the client begins crafting, it will start playing several animations in a specific order along with a result animation based on the success status or failure of the synthesis. If the client fails to begin the crafting animations correctly, it will exit out of the synthesis early and set this value to `1` to make it as a failed attempt locally. Successful animation playing for the full synthesis attempt will set this value to `0`.

If the client fails to be properly set to the correct animation status while crafting, it will also cause this to be set to `1`. _(The client expects itself to be set to status `44` (`CAMP`).)_

### `padding00`

_Padding; unused._
