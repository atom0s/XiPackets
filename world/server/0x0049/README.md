# `Unknown - Packet: 0x0049`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0049` |
| **Size**                  | `0x0048` |

## Description

This packet is sent by the server to respond to a client `/itemsearch` request.

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
    uint8_t     Flag;
    uint8_t     padding00;
    uint8_t     ItemName[64];
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `ItemNo`

_The item number._

### `Flag`

_The response flag._

### `padding00`

_Padding; unused._

### `ItemName`

_The item name._

## Additional Information

The client can search for items within their various containers using the `/itemsearch` command. When this command is used, it will send a request to the server to perform the initial search. The server will then respond with this packet to inform the client of how to handle the response.

First, the client will check the `Flag` value. If it is non-zero, the response is considered async and will cause the client to defer handling the packet until the next frame. When this flag is set, the client will also search each container in an async manner. This means that the client will only check one container per frame, for a total of 18 frames. This flag also causes the client to ignore the `ItemNo` field in the packet and only use the `ItemName` value instead.

Next, if the `Flag` value is not set, then the client will check the `ItemNo` value. If this value is `0` then the client will exit the callback handler and print that the item was found found.

If `Flag` is not set, then the client will then check if the `ItemNo` value is set. If `ItemNo` is set, then the client will immediately loop through all the clients containers and check to see if the item is found. If found, the client will print a message stating which container the item was found in.

## Additional Information: `Flag`

The intended purpose of this flag is unknown at this time other than to cause the client to handle the response in an async manner. This could be something the server decides is required under high-stress load times or when the client is in certain areas that are congested.

When `Flag` is set _(non-zero)_ the client ignores the `ItemNo` value and will manually do the item lookups locally by the name they previously sent in the search request. _(The server echos back the same item name search string.)_ However, when the `Flag` is not set, the client confirms that the server performed the search and found the item by checking the `ItemNo` value for `0`. The client will still also perform the search locally, however this time it will use the `ItemNo` value instead of the name.

The client performs the item searching using the following functions during the given conditions:

  - `pkt->Flag >= 1` - _The client will use `GetItemDataByName`._
  - `pkt->Flag == 0 && pkt->ItemNo != 0` - _The client will use `GetItemCountByItemNo`._
