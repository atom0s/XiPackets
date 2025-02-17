# `GP_SERV_COMMAND_BAZAAR_SHOPPING`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_BAZAAR_SHOPPING` |
| **Client Handlers**       | `RecvShopping`, `RecvBazarShop` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0108` |
| **Size**                  | `0x0020` |

## Description

This packet is sent by the server when another player has entered or left the local clients bazaar.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_BAZAAR_SHOPPING_STATE
enum GP_BAZAAR_SHOPPING_STATE
{
    GP_BAZAAR_SHOPPING_STATE_ENTER,
    GP_BAZAAR_SHOPPING_STATE_EXIT,
    GP_BAZAAR_SHOPPING_STATE_END
};

// PS2: GP_SERV_BAZAAR_SHOPPING
struct GP_SERV_BAZAAR_SHOPPING
{
    uint16_t                    id: 9;
    uint16_t                    size: 7;
    uint16_t                    sync;
    uint32_t                    UniqueNo;   // PS2: UniqueNo
    GP_BAZAAR_SHOPPING_STATE    State;      // PS2: State
    uint8_t                     HideLevel;  // PS2: HideLevel
    uint8_t                     padding00;  // PS2: padding00
    uint16_t                    ActIndex;   // PS2: ActIndex
    uint8_t                     sName[16];  // PS2: sName
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_BAZAAR_SHOPPING`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNo`

_The viewing players server id._

### `State`

_The players viewing state._

| State | Name | Purpose |
| --- | --- | --- |
| `0` | `GP_BAZAAR_SHOPPING_STATE_ENTER`    | _Player has entered the local clients bazaar._ |
| `1` | `GP_BAZAAR_SHOPPING_STATE_EXIT`     | _Player has exited the local clients bazaar._ |

### `HideLevel`

_Flag used to hide the viewers message upon entering or leaving the local clients bazaar._

### `padding00`

_Padding; unused._

### `ActIndex`

_The viewing players target index._

### `sName`

_The viewing players name._
