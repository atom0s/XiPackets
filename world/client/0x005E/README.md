# `GP_CLI_COMMAND_MAPRECT`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_CLI_COMMAND_MAPRECT` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x005E` |
| **Size**                  | `0x0018` |

## Description

This packet is sent by the client when requesting to change zones after touching a zone line.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_CLI_MAPRECT
struct GP_CLI_MAPRECT
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    RectID;         // PS2: RectID
    float       x;              // PS2: x
    float       y;              // PS2: y
    float       z;              // PS2: z
    uint16_t    ActIndex;       // PS2: ActIndex
    uint8_t     MyRoomExitBit;  // PS2: MyRoomExitBit
    uint8_t     MyRoomExitMode; // PS2: MyRoomExitMode
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_CLI_MAPRECT`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `RectID`

_The fourcc tag of the zone line that has been touched._

When exiting the mog house, this will be set to `zmrq` _(`0x71726D7A`)_.

### `x`

_Unused._

This value is no longer used.

### `y`

_Unused._

This value is no longer used.

### `z`

_Unused._

This value is no longer used.

### `ActIndex`

_Unused._

This value is no longer used.

### `MyRoomExitBit`

_The mog house exit type._

This value is used to determine which area the client is exiting from. The client handles this value with a few conditions. If the client is exiting an area that has a sub-map number, this value will be set to the current `PTR_pGlobalNowZone->MyroomType` value.

Otherwise, the client will do a switch-case handling the `PTR_pGlobalNowZone->MyroomMapNumber`.

| `MyroomMapNumber` | Value | Region |
| --- | --- | --- |
| `0x00BD` | `6` | `Ronfaure Front` |
| `0x00C7` | `7` | `Gustaberg Front` |
| `0x00D6` | `5` | `Whitegate` |
| `0x00DB` | `8` | `Saruta Front` |
| `0x0100` | `4` | `Jeuno` |
| `0x0101` | `1` | `San d'Oria` |
| `0x0121` | `1` | `San d'Oria` |
| `0x0102` | `2` | `Bastok` |
| `0x0122` | `2` | `Bastok` |
| `0x0120` | `3` | `Windurst` |
| `0x0123` | `3` | `Windurst` |
| `0x0124` | `9` | `Adoulin` |
| `(Default)` | `0` | `(Default)` |

### `MyRoomExitMode`

_The zone change exit mode._

This value is used to determine how the client is exiting their mog house. The values used with this will relate to the mog house exit menu selection the client chooses. The client can complete side quests in certain areas to unlock additional menu options when exiting their mog house, such as being able to choose connected areas in the same region to exit to.

If the client has not completed the given areas exit quest and only has the option to simply leave the mog house, this value will be set to `0`.

If the client has completed the given areas exit quest, then this value will change based on the menu option they select.

  - Selecting `Area you entered from.` will set this value to `0`.
  - Selecting `Select an area to exit to.` will set this value based on the menu option chosen.

For example, when exiting the mog house of San d'Oria and selecting `Select an area to exit to.`, the following values are used for the available menu options:

  - Selecting `Southern San d'Oria` will set this value to `1`.
  - Selecting `Northern San d'Oria` will set this value to `2`.
  - Selecting `Port San d'Oria` will set this value to `3`.
  - Selecting `Mog Garden` will set this value to `127`.

When the client unlocks their 2nd floor, this will also change how this value is set.

  - Changing to 1st floor: `126`
  - Changing to 2nd floor: `125`
