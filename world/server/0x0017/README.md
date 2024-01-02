# `GP_SERV_COMMAND_CHAT_STD`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_CHAT_STD` |
| **Client Handler**        | `RecvStdChat` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0017` |
| **Size**                  | `(varies)` |

## Description

This packet is sent by the server to display chat messages to the client. The contents of this packet can vary based on the message `Kind`.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_CHAT_STD
struct GP_SERV_CHAT_STD
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Kind;       // PS2: Kind
    uint8_t     Attr;       // PS2: Attr
    uint16_t    Data;       // PS2: (New; did not exist.)
    uint8_t     sName[15];  // PS2: sName
    uint8_t     Mes[];      // PS2: Mes
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_CHAT_STD`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `Kind`

_The message kind._

This value determines the kind of chat message that is being sent to the client. These are the valid message kinds:

| Mode | Kind | Command | Notes |
| --- | --- | --- | --- |
| `0x00` | `Say`            | `/say`        | _N/A_ |
| `0x01` | `Shout`          | `/shout`      | _N/A_ |
| `0x02` | `Unused.`        | `Unused.`     | _N/A_ |
| `0x03` | `Tell`           | `/tell`       | _N/A_ |
| `0x04` | `Party`          | `/party`      | _N/A_ |
| `0x05` | `Linkshell (1)`  | `/linkshell`  | _N/A_ |
| `0x06` | `System Message` | `N/A`         | _N/A_ |
| `0x07` | `System Message` | `N/A`         | _Used for area announcements, system messages, etc._ |
| `0x08` | `Emote Message`  | `/emote`      | _N/A_ |
| `0x09` | `N/A`            | `N/A`         | _N/A_ |
| `0x0A` | `N/A`            | `N/A`         | _N/A_ |
| `0x0B` | `N/A`            | `N/A`         | _N/A_ |
| `0x0C` | `GM Prompt`      | `N/A`         | _Used by GMs to query a client with a prompt._ |
| `0x0D` | `Say`            | `N/A`         | _Message is displayed without a name._ |
| `0x0E` | `Shout`          | `N/A`         | _Message is displayed without a name._ |
| `0x0F` | `Party`          | `N/A`         | _Message is displayed without a name._ |
| `0x10` | `Linkshell (1)`  | `N/A`         | _Message is displayed without a name._ |
| `0x11` | `Std Message`    | `N/A`         | _Standard messages, yellow color._ |
| `0x12` | `Std Message`    | `N/A`         | _Standard messages, yellow color._ |
| `0x13` | `Std Message`    | `N/A`         | _Standard messages, yellow color._ |
| `0x14` | `Std Message`    | `N/A`         | _Standard messages, yellow color._ |
| `0x15` | `Std Message`    | `N/A`         | _Standard messages, yellow color._ |
| `0x16` | `Std Message`    | `N/A`         | _Standard messages, yellow color._ |
| `0x17` | `Std Message`    | `N/A`         | _Standard messages, yellow color._ |
| `0x18` | `Say (Copy)`     | `N/A`         | _N/A_ |
| `0x19` | `Say (Copy)`     | `N/A`         | _N/A_ |
| `0x1A` | `Yell`           | `/yell`       | _N/A_ |
| `0x1B` | `Linkshell (2)`  | `/linkshell2` | _N/A_ |
| `0x1C` | `Linkshell (2)`  | `N/A`         | _Message is displayed without a name._ |
| `0x1D` | `Std Message`    | `N/A`         | _Used for 'Basic System Messages'._ |
| `0x1E` | `Linkshell (3)`  | `/linkshell3` | _N/A_ |
| `0x1F` | `Linkshell (3)`  | `N/A`         | _Message is displayed without a name._ |
| `0x20` | `Std Message`    | `N/A`         | _Standard messages, yellow color._ |
| `0x21` | `Unity`          | `/unity`      | _N/A_ |
| `0x22` | `Assist (J)`     | `/assistj`    | _N/A_ |
| `0x23` | `Assist (E)`     | `/assiste`    | _N/A_ |

_Other values are not used and no message is printed._

### `Attr`

_The message attributes._

This value holds the flags of the message used to set additional properties.

| Flag | Purpose |
| --- | --- |
| `0x00` | _None._ |
| `0x01` | _Prefix the message with `[GM]`._ |
| `0x08` | _Message is in a special format. (See additional info below.)_ |

### `Data`

_The message data._

This value is set depending on the message `Kind` and its type changes based on its usage.

| Kind | Type | Usage |
| --- | --- | --- |
| `0x1A` | `uint16_t` | _Value is the senders zone id._ |
| `0x22` | `uint8_t[2]` | _Values are the senders mastery rank and mentor status._ |
| `0x23` | `uint8_t[2]` | _Values are the senders mastery rank and mentor status._ |

_All other `Kind` usages do not make use of this value._

### `sName`

_The message sender name._

> [!WARNING]
> This string value is not guaranteed to be null-terminated! If a sender name takes up all 15 bytes, there will be no null-terminator!

### `Mes`

_The message string._

> [!WARNING]
> This string value is not guaranteed to be null-terminated! Due to how packet alignment is handled, it is possible the string will not be null-terminated. See info below.

## Additional Information

This packet is used for all main means of chat communication. The older message packets _(`0x14`, `0x16`)_ have been deprecated and merged into this one. Depending on the message `Kind` the data stored inside of the `Mes` string can contain special formatting values. Below are some notes on these special cases.

### `Kind: 0x0C - GM Prompt`

Game Masters can send dialog prompts to clients that shows a title and selection of options to pick from which uses this message kind. This was used back in the older era of the game when GM's actively patrolled looking for botters and dealing with player reports more hands-on. This was often used to send clients a dialog that would ask a basic question and expect a valid response. Bots would generally not react to this dialog and timeout leading to GMs jailing offenders.

When this message kind is used, the format of the `Mes` data is expected to be in quoted string chunks. The first quoted string is used as the dialog prompt title while all further quoted strings in the message are set as the options shown in the dialog.

```cpp
char Mes[150]{};
::sprintf_s(Mes, "\"%s\"\"%s\"\"%s\"\"%s\"\"%s\"\"%s\"\"%s\"",
    // This will be the dialog title..
    "atom0s was here.",
    // This will be the dialog options..
    "Option 1", "Option 2", "Option 3", "Option 4", "Option 5", "Option 6"
    );
```

![https://i.imgur.com/Pv7T53J.png](https://i.imgur.com/Pv7T53J.png)

When this dialog exits by one of many conditions, it will automatically respond to the GM that sent the message to the client. This is done by having the client send a tell back to the GM, in secret. _(The client will not see this tell.)_ The response tell will look like this:

![https://i.imgur.com/MWFnR5Y.png](https://i.imgur.com/MWFnR5Y.png)

As seen in this screenshot, the response message contains the GM's name that caused the dialog, the original question and the result of the dialog closing. The result value can be one of a few things:

  - `Canceled.` - The client closed the dialog without selecting an option.
  - `Canceled due to event activation.` - The client started an event while the dialog was still open.
  - `Currently in an event. This GMTELL was invalidated.` - The client is already in an event, the dialog is ignored.
  - `Currently four GMTELL's have been received. This GMTELL was invalidated.` - The client has 4 pending GMTELL dialogs already, the new one is ignored.
  - `<Option String>` - The client selected an option, the value will be the option string that was sent in the original message.

### `Kind: Any - Attr: 0x08`

The `Attr` value has a special flag `0x08` that can be set which denotes that the `Mes` value contains special formatting which is parsed in a different manner. This is used to allow the `Mes` value to contain values which are parsed back into DAT file lookup values and parameters for formatting a DAT string.

When this attribute flag is set, the client will use `sscanf` on the `Mes` to look for the following format:

```cpp
::sscanf(Mes, "%x,%x,%x,%x,%x,%x,%x,", &val1, &val2, &val3, &val4, &val5, &val6, &val7);
```

The actual value formatting in the `Mes` should be like this:

```cpp
"00,0000,00000000,00000000,00000000,00000000,00000000,"
```

Upon being parsed, these values are then used to lookup and format the DAT message.

  - `val1` - Used as the table index.
  - `val2` - Used as the message index.

The table index value (`val1`) is passed to a helper method which is used to determine which DAT file is used to pull the message from. The following are valid indices:

  - `0` - _None. (Invalid)_
  - `1` - `EventMess` - _The current zones event messages._
  - `2` - `SkillName` - _Skill names._
  - `3` - `BitName` - _Action effects._
  - `4` - `EmotionMess` - _Emote messages._
  - `5` - `BtlMess` - _Action/Combat messages._
  - `6` - `SystemMess` - _System messages._
  - `7` - `MonWazaMess` - _Monster skill names._
  - `8` - `SevMess` - _The current zone general messages._
  - `9` - `TrustMess` - _Trust NPC messages._
  - `10` - `UnityMess` - _Unity NPC messages._

_This kind of message setup expects additional client values to be already set from things such as event related buffers. Because of this, this kind of message handling is not recommended for general usage. Incorrect usage will crash the client as there is little to no error checking performed._

## Additional Information: Message Length / Null-Termination

This packet contains two string values, `sName` and `Mes`, which are both **NOT** null-terminated. Due to the way string values in packets are handled, these must be read properly in order for their contents to not overflow into other values or cause incorrect termination.

The sender name (`sName`) can hold upto a maxmium of 15 characters. If a sender name is this length, then there will be no valid null character in the buffer. Because of this, the name should be read as its full 15 characters always and then trimmed based on if any nulls are present. If not, then the name will be the full 15 characters instead.

The message string (`Mes`) is variable length and based on the message being sent. Due to how packet alignment is handled in FFXI, this value is also not null-terminated and is instead alignment-terminated as well as 'clamped' to a maximum length of 150 characters.

When this packet is received, the client determines the length of the message by doing the following:

```cpp
auto id_size = *static_cast<uint16_t*>(pkt);
auto msg_len = FUNC_enQueAddSizeGet(0x18, id_size >> 0x09) + 0x01;
```

This helper function is used to determine the size of a variable-length field within the packet data; generally the last field in the packet, and looks like this:

```cpp
int __cdecl FUNC_enQueAddSizeGet(int struct_size, int packet_size)
{
    return 4 * packet_size - struct_size;
}
```

  - `struct_size` - _The offset into the packet where the actual variable length member starts. This is used to remove the length of data used for the other packet members._
  - `packet_size` - _The total size of the packet from the packet header._

_The client further clamps the value of `msg_len` returned from this function to a maximum of 150 characters._

_**Please note:** FFXI reuses the same main packet buffer for all game traffic. Due to this, it is important that you read the message length properly and not assume there is any kind of null-termination after the message. The buffer used for packets is not cleared between usages and can contain junk data. Due to the message not being null-terminated, it is possible to read extra junk characters into your message if you try and assume null-termination._
