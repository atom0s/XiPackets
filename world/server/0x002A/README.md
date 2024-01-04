# `GP_SERV_COMMAND_TALKNUMWORK`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_TALKNUMWORK` |
| **Client Handler**        | `RecvMessageTalkNumWork` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x002A` |
| **Size**                  | `0x0040` |

## Description

This packet is sent by the server to display a formatted general zone message.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_TALKNUMWORK
struct GP_SERV_TALKNUMWORK
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    UniqueNo;   // PS2: UniqueNo
    int32_t     num[4];     // PS2: num
    uint16_t    ActIndex;   // PS2: ActIndex
    uint16_t    MesNum;     // PS2: MesNum
    uint8_t     Type;       // PS2: Type
    uint8_t     Flag;       // PS2: dummy
    uint8_t     String[32]; // PS2: dummy2
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_TALKNUMWORK`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `UniqueNo`

_The message entity server id._

### `num`

_The message number parameters._

### `ActIndex`

_The message entity target index._

### `MesNum`

_The message number._

The `MesNum` value holds two separate values; the actual message number and a flag which is used to have the handler function ignore validating the values of the packets `ActIndex` and `UniqueNo` fields. Usually, these values will be validated to ensure the entity information in the packet is valid and accessible on the local client.

  - The message number can be filtered with: `pkt->MesNum & 0x7FFF`
  - The flag can be checked with: `pkt->MesNum & 0x8000`

The flag value is also used to tell the handler to not use the entity name when building the message to be printed. It will instead check if the `String` value should be used.

The message number value is specifically used with the `SevMess` dialog string table. These strings are the general zone messages.

### `Type`

_The message type._

This value is used to determine the message type. The client uses a local lookup table for this value. Any value that is `>= 8` will be ignored and defaults to `0`, otherwise the following table is used:

| Type | Returned Lookup Value |
| --- | --- |
| `0` | `0x8E` |
| `1` | `0xA1` |
| `2` | `0x90` |
| `3` | `0x91` |
| `4` | `0x92` |
| `5` | `0xA1` |
| `6` | `0x94` |
| `7` | `0x95` |

### `Flag`

_The message flag._

This value is a flag that is used to trigger if certain parts of the handler should be skipped or enforced when other conditions are met. _(See 'Additional Information' for more details.)_

### `String`

_The message string parameter._

## Additional Information

This packet is used to display general purpose zone messages to the client. It can be configured to handle these messages in different ways based on the data within the packet. The `Flag` value can be used to treat the packet as an event based zone message which also causes it to handle validation and name handling differently. This also expects the `String` value to be set when this mode is enabled in order to work.

The below pseudocode demonstrates how parts of this handler operates:

```cpp
    // Check if event mode is being used, a string is set, and no server id was given..
    if (pkt->Flag && pkt->String[0] && !pkt->UniqueNo)
    {
        // Validate pkt->ActIndex
    }
    else
    {
        // Check if the MesNum flag is set to ignore entity validation..
        if ((pkt->MesNum & 0x8000) != 0)
        {
            PTR_ServerEventIndex = 0;
            goto skip_event;
        }

        // Validate pkt->ActIndex
        // Validate pkt->UniqueNo
    }

    // Set the event entity index to this packets entity index..
    PTR_ServerEventIndex = pkt->ActIndex;

skip_event:

    // Snipped other processing here..

    if (pkt->String[0] && !pkt->Flag)
    {
        // Copy the packet string into a temp buffer.. (Don't be mislead by the buffer name here..)
        ::strncpy(PTR_EventWorldPassBuff, pkt->String, 0x20);
    }

    // Check if the event name skip setup is set..
    if (pkt->String[0] && pkt->Flag && !pkt->UniqueNo)
        goto skip_name;

    // Check if the MesNum flag is not set..
    if ((pkt->MesNum & 0x8000) == 0)
    {
        // Obtain the entity set in the pkt->ActIndex..
        // Obtain the entity name..
        // Call FUNC_EventMessDecodePutMoute(...)
        goto cleanup;
    }

    if (pkt->String[0] && pkt->Flag && !pkt->UniqueNo)
    {
skip_name:
        // Use pkt->String as the entity name for the message..
        // Call FUNC_EventMessDecodePutMoute(..)
    }
    else
    {
        // Call FUNC_EventMessDecodePutMoute(..)
    }

cleanup:
    // Do cleanup here..
    return 1;
```
