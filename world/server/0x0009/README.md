# `GP_SERV_COMMAND_MESSAGE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_MESSAGE` |
| **Client Handler**        | `RecvMessage` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0009` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the server to display general purpose system messages to the client. The packet contains the message id to be loaded from the games data files along with the parameters used to populate the strings formatting arguments.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_MESSAGE
struct GP_SERV_MESSAGE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    UniqueNo;   // PS2: UniqueNo
    uint16_t    ActIndex;   // PS2: ActIndex
    uint16_t    MesNo;      // PS2: MesNo
    uint8_t     Attr;       // PS2: Attr
    uint8_t     Data[];     // PS2: Data
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_MESSAGE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `UniqueNo`

_The entity server id._

### `ActIndex`

_The entity target index._

### `MesNo`

_The system message id._

### `Attr`

_The message attributes._

The client only ever checks this value for `0x10` in the following manner:

```cpp
if ((packet->Attr & 0x10) == 0 || FUNC_gcCheckBlackID(packet->UniqueNo) != 1)
{
    // Handle packet..
}
```

If this is false, the client will ignore the packet.

### `Data`

_The message parameters string._

This value contains a string holding the additional formatting arguments used to format the message. This value is variable length and based on the packet length to determine its proper size.

This value is formatted in a specific manner in which the client parses for an expected 'key' string and uses the string that follows it as the value. These values are expected to be in a specific format based on the key being searched for. The following table explains what keys the client looks for and what format the values should be in.

| Key | Value Type | Value Size |
| --- | :---: | :---: |
|
| `CasUniqueNo` | `int32_t` | `4` |
| `TarUniqueNo` | `int32_t` | `4` |
| `Para0`       | `int32_t` | `4` |
| `Para1`       | `int32_t` | `4` |
| `Para2`       | `int32_t` | `4` |
| `Para3`       | `int32_t` | `4` |
| `Mode`        | `int32_t` | `4` |
| `string2`     | `string`  | `24` |
| `string3`     | `string`  | `24` |

The client uses the following function to look for the given key and return the value:

```cpp
char *__cdecl FUNC_Packet_Incoming_0x0009_RecvMessage_Inline(char *haystack, char *needle)
{
    const char* str = ::strstr(haystack, needle);
    if (str)
    {
        str += ::strlen(needle);
        for (char i = *str; i; i = *++str)
        {
            if (i != ' ' && i != '\t')
                break;
        }
    }
    return str;
}
```

When handling an integer value, the client uses `atoi(value)` to parse the value. For string values, the client uses another helper function to pull the string from the `Data`:

```cpp
int __cdecl FUNC_Packet_Incoming_0x0009_RecvMessage_Inline2(char *buffer, char *value, int32_t len)
{
    int32_t idx = 0;

    for (idx = 0; idx < len; ++value)
    {
        char c = *value;
        if (!c || c == '\t' || c == ' ')
            break;

        *buffer = c;
        ++idx;
        ++buffer;
    }
    return idx;
}
```

## Additional Information

The messages this packet makes use of are stored in the following DAT files:

| Language | DAT File Id | DAT File |
| ---: | :---: | :---: |
| `Japanese` | `7030` | `\ROM\27\75.DAT` |
| `English` | `7031` | `\ROM\27\76.DAT` |

The client parses this packet into a temporary structure, `SYSMES`. This is then passed to `PutSystemMessage` to process and print the message.

The `UniqueNo` and `ActIndex` values are not always populated. These are used when the message is triggered by another player entity towards the local client. The client uses the `UniqueNo` value to check if the player is blacklisted by the local client. If they are blacklisted, the message is ignored and not processed.
