# `GP_CLI_COMMAND_EVENTENDXZY`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_EVENTENDXZY` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x005C` |
| **Size**                  | `0x0020` |

## Description

This packet is sent by the client when updating an event that involves the clients position. _(ie. Requesting to warp between telepoints.)_

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_EVENTENDXZY
struct GP_CLI_EVENTENDXZY
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    float       x;          // PS2: x
    float       y;          // PS2: y
    float       z;          // PS2: z
    uint32_t    UniqueNo;   // PS2: UniqueNo
    uint32_t    EndPara;    // PS2: EndPara
    uint16_t    EventNum;   // PS2: EventNum
    uint16_t    EventPara;  // PS2: EventPara
    uint16_t    ActIndex;   // PS2: ActIndex
    uint8_t     Mode;       // PS2: Mode
    int8_t      dir;        // PS2: dir
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_EVENTENDXZY`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `x`

_The local clients X position._

### `y`

_The local clients Y position._

### `z`

_The local clients Z position._

### `UniqueNo`

_The event server id._

This value is set to the clients local `PTR_CliEventUniqueNo` value. This will generally be set to the local clients server id, however it can also be updated by the server with the various event related packets.

### `EndPara`

_The event end parameter._

This value will be set to the value stored in `PTR_Work_Zone[1]`.

### `EventNum`

_The event number._

This value is set to the clients local `PTR_CliEventNum` value.

### `EventPara`

_The event parameter._

This value is set to the clients local `PTR_CliEventPara` value.

### `ActIndex`

_The event target index._

This value is set to the clients local `PTR_CliEventIndex` value. This will generally be set to the local clients target index, however it can also be updated by the server with the various event related packets.

### `Mode`

_The packet mode._

This value is always set to `1`.

### `dir`

_The local clients heading direction._

## Additional Information

This packet is used by the client during events that will alter the clients position. This includes things such as selecting to warp between Homepoints, entering doors or areas that move the player to enter, attempting to enter restricted areas that move the player back.
