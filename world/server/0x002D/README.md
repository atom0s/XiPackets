# `GP_SERV_COMMAND_BATTLE_MESSAGE2`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_BATTLE_MESSAGE2` |
| **Client Handler**        | `RecvBattleMessage2` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x002D` |
| **Size**                  | `0x001C` |

## Description

This packet is sent by the server to display combat related messages to the client.

_This packet is generally used for end-of-combat related messages such as chains, experience/limit points gains, merit points/job points gains, etc._

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_BATTLE_MESSAGE2
struct GP_SERV_BATTLE_MESSAGE2
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;

    uint32_t    UniqueNoCas;    // PS2: UniqueNoCas
    uint32_t    UniqueNoTar;    // PS2: UniqueNoTar
    uint16_t    ActIndexCas;    // PS2: ActIndexCas
    uint16_t    ActIndexTar;    // PS2: ActIndexTar
    uint32_t    Data;           // PS2: data
    uint32_t    Data2;          // PS2: data2
    uint16_t    MessageNum;     // PS2: MessageNum
    uint8_t     Type;           // PS2: Type
    uint8_t     padding1B;      // PS2: dammy3
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_BATTLE_MESSAGE2`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `UniqueNoCas`

_The caster entity server id._

### `UniqueNoTar`

_The target entity server id._

### `ActIndexCas`

_The caster entity target index._

### `ActIndexTar`

_The target entity target index._

### `Data`

_The message data._

This value is used as a parameter to the message.

### `Data2`

_The message data. (2)_

This value is used as a parameter to the message.

### `MessageNum`

_The message number._

This value is used to determine both the message color and the message string.

  - The message color is determined based on various factors and uses hard-coded lookup tables in the client. _(These tables are huge so they will be excluded from this documentation.)_
  - The message string is obtained through the `BtlMess` dialog string table. The color tables are rather large

### `Type`

_The message type._

### `padding1B`

_Padding; unused._
