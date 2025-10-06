# `Packet: 0x0053`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Direction**             | `C -> S` |
| **OpCode**                | `0x0053` |
| **Size**                  | `0x0088` |

## Description

This packet is sent by the client when changing their lockstyle.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct lockstyleitem_t
{
    uint8_t     ItemIndex;
    uint8_t     EquipKind;
    uint8_t     Category;
    uint8_t     padding00;
    uint16_t    ItemNo;
    uint16_t    padding01;
};

// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t        id: 9;
    uint16_t        size: 7;
    uint16_t        sync;

    uint8_t         Count;
    uint8_t         Mode;
    uint8_t         Flags;
    uint8_t         padding00;
    lockstyleitem_t Items[16];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Count`

_The count of items the lockstyle has enabled in the `Items` array._

### `Mode`

_The lockstyle mode._

This value is used to determine the kind of lockstyle packet that is being sent.

| Mode | Purpose |
| --- | --- |
| `0` | _Disables the lockstyle feature._ |
| `1` | _Continues the lockstyle feature._ |
| `2` | _Queries the players current lockstyle feature setting from the server._ |
| `3` | _Sets the players lockstyle._ |
| `4` | _Enables the lockstyle feature._ |

The client will use each of these `Mode` values during different conditions.

  - **`Mode`: 0** _(Disable)_
    - When zoning into a new area. _(While lockstyle is disabled.)_
    - When changing jobs.
    - When using the `/lockstyle off` command.
    - When using the `Main Menu > Config > Misc. 3 > Style Lock > Normal` configuration setting.
  - **`Mode`: 1** _(KeepAlive)_
    - When zoning into a new area. _(While lockstyle is enabled.)_
  - **`Mode`: 2** _(Query)_
    - When using the `/lockstyle` command. _(Without any arguments.)_
  - **`Mode`: 3** _(Set)_
    - When using the `/lockstyleset #` command. _(With a valid set id.)_
  - **`Mode`: 4** _(Enable)_
    - When using the `/lockstyle on` command.
    - When using the `/lockstyleset` command. _(Without any arguments.)_
    - When using the `Main Menu > Config > Misc. 3 > Style Lock > Static` configuration setting.

### `Flags`

_The packet flags._

This value is used to hold the additional flags about the packet. The client only makes use of the lowest bit of this value to set the `/lockstyleset` sub-command, `echo`. When this argument is used, this bit will be set.

### `padding00`

_Padding; unused._

### `Items`

_The items that will be applied for the lock style._
