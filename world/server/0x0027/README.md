# `GP_SERV_COMMAND_TALKNUMWORK2`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_TALKNUMWORK2` |
| **Client Handler**        | `RecvMessageTalkNumWork2` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0027` |
| **Size**                  | `0x0070` |

## Description

This packet is sent by the server to display a formatted message loaded from the DAT files.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_TALKNUMWORK2
struct GP_SERV_TALKNUMWORK2
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    UniqueNo;   // PS2: UniqueNo
    uint16_t    ActIndex;   // PS2: ActIndex
    uint16_t    MesNum;     // PS2: MesNum
    uint16_t    Type;       // PS2: Type
    uint8_t     Flags;      // PS2: (New; did not exist.)
    uint8_t     padding0F;  // PS2: dummy
    uint32_t    Num1[4];    // PS2: Num
    uint8_t     String1[32];// PS2: String
    uint8_t     String2[16];// PS2: (New; did not exist.)
    uint32_t    Num2[8];    // PS2: (New; did not exist.)
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_TALKNUMWORK2`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The message entity server id._

### `ActIndex`

_The message entity target index._

### `MesNum`

_The message number._

The `MesNum` value holds two separate values; the actual message number and a flag which is used to have the handler function ignore validating the values of the packets `ActIndex` and `UniqueNo` fields. Usually, these values will be validated to ensure the entity information in the packet is valid and accessible on the local client.

  - The message number can be filtered with: `pkt->MesNum & 0x7FFF`
  - The flag can be checked with: `pkt->MesNum & 0x8000`

The flag value is also used to tell the handler to not use the entity name when building the message to be printed. It will instead check if the `String2` value should be used.

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

_These values are used as the chat mode of the message that will be printed._

### `Flags`

_The message flags._

This value is used to determine how the `String1` and `String2` values are used. The client only uses values `1` and `2` for this value.

_See `Additional Information` on how this works._

### `padding0F`

_Padding; unused._

### `Num1`

_The message number parameters. (1)_

### `String1`

_The message string parameter. (1)_

### `String2`

_The message string parameter. (2)_

### `Num2`

_The message number parameters. (2)_

## Additional Information

The `String1` and `String2` values are only used when certain conditions are met. The following is a quick pseudocode example of how these values work with the `Flags` value to determine which string is used.

```cpp
// Check if MesNum flag is not set..
if ((pkt->MesNum & 0x80000) == 0)
{
    // Use String1 as the name initially..
    ::sprintf(buffer, "%s : ", pkt->String1);

    // Check if String2 has a valid string value or if Flags is set to 1..
    if (pkt->String2[0] || (pkt->Flags & 1) != 0)
    {
        // Check if the packet server id is a mob/npc..
        if ((pkt->UniqueNo & 0xFF000000) != 0)
        {
            // Lookup and use the mob/npc name that matches the given server id..
            const auto name = FUNC_SearthMonNameIndex(pkt->UniqueNo);
            ::strncpy(name_buffer, name, 0x20);
        }
        else if (pkt->String2[0])
        {
            // Use the String2 value if its set as an override..
            ::strncpy(name_buffer, pkt->String2, 0x20);
        }
        else
        {
            // Lookup and use the player name that matches the given server id..
            const auto name = FUNC_SearthPcNameIndex(pkt->UniqueNo);
            ::strncpy(name_buffer, name, 0x20);
        }

        // Override the name buffer..
        ::sprintf(buffer, "%s : ", name_buffer);
    }
}
// If MesNum flag is set, check if String2 should be used as the name..
else if (pkt->String2[0] && (pkt->Flags & 2) != 0)
{
    ::sprintf(buffer, "%s : ", pkt->String2);
}
```
