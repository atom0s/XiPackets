# `GP_SERV_COMMAND_INSPECT_MESSAGE`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_INSPECT_MESSAGE` |
| **Client Handlers**       | `RecvInspectMessage`, `CTkInspect::RecvBazar` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x00CA` |
| **Size**                  | `0x0094` |

## Description

This packet is sent by the server in response to the client checking another player. This packet is used to populate the players bazaar message and print their title.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_INSPECT_MESSAGE
struct GP_SERV_INSPECT_MESSAGE
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     sInspectMessage[123];   // PS2: sInspectMessage
    uint8_t     BazaarFlag  : 1;        // PS2: BazaarFlag
    uint8_t     MyFlag      : 1;        // PS2: MyFlag
    uint8_t     Race        : 6;        // PS2: (New; was previously padding.)
    uint8_t     sName[16];              // PS2: sName
    uint32_t    DesignationNo;          // PS2: DesignationNo
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_INSPECT_MESSAGE`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `sInspectMessage`

_The checked players bazaar message._

This value holds the checked players full bazaar message, broken into its three lines. Each line takes up 40 characters of space. There is no null terminator used with this string and all unused characters will use spaces _(`0x20`)_ instead of null characters.

### `BazaarFlag`

_Flag that states if the checked player has a bazaar available._

This flag is set when the checked player has a bazaar available for browsing.

### `MyFlag`

_Flag that states if the checked player is the local player._

This flag is set when the local client checks themselves.

### `Race`

_The checked players race id._

This value holds the checked players race id. This is an old feature that was used when the game was also playable in French and German. Those languages offered gender specific title strings which used this value to determine the gender of the player.

### `sName`

_The checked players name._

### `DesignationNo`

_The checked players title._

This value holds the title index value that the player currently has equipped. This value aligns directly with the index within the title DAT file:

  - JP: `ROM\180\77.DAT` (File Id: `55584`)
  - NA: `ROM\180\78.DAT` (File Id: `55704`)

## Additional Information

This packet has two handlers registered for it. Each is used to handle a different part of the packets data.

### `RecvInspectMessage`

This handler is used to display the checked players title. It will check the `Race` value to determine the players gender, then use the `DesignationNo` value to obtain and print the proper title for the player. If the examined `sName` matches the local clients name, then this handler is skipped. _(Does not print the local players own title when checking yourself.)_

### `CTkInspect::RecvBazar`

This handler is used to display the checked players bazaar message. It updates the `CTkInspect` windows data to populate the bazaar comment information. This handler will make use of the `MyFlag` to determine which local buffer in the client to populate the message into.
