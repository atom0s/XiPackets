# `GP_CLI_COMMAND_EVENTEND`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_EVENTEND` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x005B` |
| **Size**                  | `0x0014` |

## Description

This packet is sent by the client when ending an event or updating a pending event status.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_EVENTEND
struct GP_CLI_EVENTEND
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    UniqueNo;   // PS2: UniqueNo
    uint32_t    EndPara;    // PS2: EndPara
    uint16_t    ActIndex;   // PS2: ActIndex
    uint16_t    Mode;       // PS2: Mode
    uint16_t    EventNum;   // PS2: EventNum
    uint16_t    EventPara;  // PS2: EventPara
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_EVENTEND`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The event server id._

This value is set to the clients local `PTR_CliEventUniqueNo` value. This will generally be set to the local clients server id, however it can also be updated by the server with the various event related packets.

### `EndPara`

_The event end parameter._

This value will depend on the packets `Mode` and the manner in which the packet is being created and sent.

  - **`Mode`:** `0`
    - The `EndPara` value will be set based on the clients local `PTR_CancelEvent` status.
      - If `false`, then `EndPara` will be set to `PTR_Work_Zone[1]`.
      - If `true`, then `EndPara` will be set to `0x40000000`.
  - **`Mode`:** `1`
    - The `EndPara` value will be set to `PTR_Work_Zone[1]`.

### `ActIndex`

_The event target index._

This value is set to the clients local `PTR_CliEventIndex` value. This will generally be set to the local clients target index, however it can also be updated by the server with the various event related packets.

### `Mode`

_The packet mode._

This value is used to determine the kind of packet that is being sent.

| Mode | Purpose |
| --- | --- |
| `0` | _This mode is used when ending an event._ |
| `1` | _This mode is used when updating a pending event tag._ |

### `EventNum`

_The event number._

This value is set to the clients local `PTR_CliEventNum` value.

### `EventPara`

_The event parameter._

This value is set to the clients local `PTR_CliEventPara` value.

## Additional Information

This packet is used to both update an event, generally after the client has made some kind of interaction or input and needs to await for the server to respond with updated event information, or to end an event.
