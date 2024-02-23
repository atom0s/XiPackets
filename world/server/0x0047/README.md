# `Unknown - Packet: 0x0047`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0047` |
| **Size**                  | `0x0088` |

## Description

This packet is sent by the server to respond to a clients `/translate` command request.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint16_t    ItemNo;
    uint8_t     FromIndex;
    uint8_t     ToIndex;
    uint8_t     FromString[64];
    uint8_t     ToString[64];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ItemNo`

_The id of the item that was translated._

This will be `0` if the translation failed.

### `FromIndex`

_The language index being translated from._

  - `0` - _Japanese_
  - `1` - _English_
  - `2` - _German (Deprecated)_
  - `3` - _French (Deprecated)_

### `ToIndex`

_The language index being translated to._

  - `0` - _Japanese_
  - `1` - _English_
  - `2` - _German (Deprecated)_
  - `3` - _French (Deprecated)_

### `FromString`

_The original string sent to the server to be translated._

### `ToString`

_The translated string._

## Additional Information

This packet is used as a response to when the client makes an item `/translate` request. The `/translate` command is used to request translations of an items name from one language to another which will also be added to the clients current auto-translate dictionary. The format of this command dictates the direction in which the translation will happen in regards to the language of the item being sent and what the client expects back as the response.

For example, the following would request translating `Fire Crystal` from English to Japanese:

  - `/translate "Fire Crystal" ej`

If the item translation request fails, then the server will respond with this packet with the `ItemNo` set to `0`.

If the requested translations `ToIndex` is set to either German or French, the server will ignore the request and instead echo back the original requests `FromIndex` and `FromString` values into the `ToIndex` and `ToString` respectively. The request will succeed and set the `ItemNo` value, but the translation will not happen. Only English and Japanese languages are supported.

The `ItemNo` and `FromIndex` values are used to build the auto-translate code that will be parsed into its proper string format through the clients translation system. The `ItemNo` value is the actual item id of the item being translated. However, it cannot be directly used in its full id format. Instead, the client must rebuild the id to be used in the auto-translation code tag format. The client does the following in order to do this:

```cpp
const auto item_no = pkt->ItemNo;
uint8_t item_tag[6]{};

if (!(item_no & 0xFF00))
{
    item_tag[1] = 0x09;
    item_tag[3] = 0xFF;
    item_tag[4] = item_no & 0x00FF;
}
else
{
    item_tag[3] = (item_no & 0xFF00) >> 8;

    if ((item_no & 0x00FF) > 0)
    {
        item_tag[1] = 0x07;
        item_tag[4] = item_no & 0x00FF;
    }
    else
    {
        item_tag[1] = 0x0A;
        item_tag[4] = 0xFF;
    }
}

item_tag[0] = 0xFD;
item_tag[2] = pkt->FromIndex + 1;
item_tag[5] = 0xFD;
```
