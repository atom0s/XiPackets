# World Server Packet Header

The packets sent between the world server and the client share a common packet header. This packet header holds the general information about the packet that either side of the communication can use to determine how the packet should be handled as well as if either side has potentially lost connection or 'sync' with the other.

## Packet Structure

```cpp
struct packet_t
{
    //
    // Packet Header
    //

    uint16_t    id: 9;      // PS2: ID
    uint16_t    size: 7;    // PS2: Size2
    uint16_t    sync;       // PS2: SendCnt

    //
    // Packet Data
    //

    uint8_t     data[];     // PS2: Per-packet fields replace this.
};
```

## Packet Fields

### `id`

_The packet id._

This value is used to determine the purpose of the packet and how the client will handle it. _(This is also often referred to as an opcode when dealing with packets.)_

Due to the manner in which the `id` and `size` values are bitpacked, the maximum packet id that can be used is `511`.

_**Note:** The client has a hardcoded id limit of `286`._

### `size`

_The packet size._

This value is used to determine the total size of the packet.

Due to the manner in which the `id` and `size` values are bitpacked, the maximum value that can be used is `127`. However, it is important to note that the client does not use this value directly, it is instead multiplied by `4`.

_**Note:** The client has a hardcoded size limit of `260`._

### `sync`

_The packet sync count._

This value is used as a means to ensure that the connection between the client and server _(in both directions)_ has not dropped and that both sides are in 'sync' with each other in terms of receiving and handling the latest expected data.

### `data`

_The packet-specific data that relates to the given packet id._

This field is replaced on the various packet information pages with the actual fields that populate the packets additional data.

## Additional Information: `id` & `size`

The packets used with the world server are the most-used _(highest traffic)_ of all of the games servers and services. Due to this, things are condensed as much as possible, when possible to try to save on bandwidth. _(It is important to remember that FFXI was designed when dial-up internet was still the leading service available, thus the game was designed to work with it.)_

The `id` and `size` values of the packet are bitpacked to help save space due to this. The `size` value is also multiplied by 4 _(enforcing 4 byte aligned packets)_ to also allow for the `size` field to hold a larger number than the bits allow for alone.

Both the `id` and `size` have hard-coded limits in-place to prevent the client from trying to make use of invalid data being sent or received.

  - The maximum packet id is: `285`
  - The maximum packet size is: `260` _(After being multiplied.)_

When the client receives a packet, it will determine the `id` and `size` by reading them as follows:

  - `id = *pkt & 0x01FF`;
  - `size = 4 * (*pkt >> 9);`

When the client sends a packet, it will build the `id` and `size` in two steps. The client first requests a buffer to use for the new packet using the `gcZoneSendQueSearch` function. This function takes the packet id as the first parameter. After some general checks to ensure the packet id is valid and can be sent, the client will return the calling function a pointer to a buffer to write the packet into if successful. The `id` will also be set into the header at this point as follows:

  - `pkt->IdSize = id & 0x01FF;`

Once the client has written the desired data to the buffer, it will mark the packet as complete and ready for sending using the `enQueSet` function. This function takes the desired packet size as the second argument. This function will rebuild the id and size in the packet header, validate that it is not too large, and if successful, will mark the buffer as ready to be sent. It will rebuild the id and size as follows:

  - `pkt->IdSize = pkt->IdSize & 0x1FF ^ (((size + add_size) / 4 + ((size + add_size) % 4 != 0)) << 9);`

_**Note:** The `add_size` value here is the alignment helper size used by packets that have strings within the packet; generally as the last field in the packet data._

## Additional Information: `sync`

In order to achieve fast and reliable connections for all clients connected to the server, UDP sockets are used. This type of socket does have its own set of drawbacks/downsides, but makes up for it with the speed and lack of per-packet validation _(causing resends)_ at the protocol layer. This allows for some traffic to be dropped while still maintaining a connection, as long as both sides agree to those losses. _(Through custom implementation enforcement.)_

It is also important to remember that FFXI was coded and designed back when dial-up internet was still the main means of connection. The game was designed to work with this slower technology by chunking and compressing packets together along with using smaller but more frequent blocks of data. Things were also carefully crafted to only use what was needed in most packets, along with making use of bit packing and other forms of compression where it made sense.

Since the world server connection makes use of a UDP socket, it implements its own form of packet tracking _(something TCP handles automatically)_ and sequence counting. This is all done making use of the `sync` value within the packet header. This value is responsible for ensuring the client and server remain aligned to each other and that packets are being handled in the expected order. This value allows either side to recognize if a previous expected chunk was lost or not received and can allow for it to be resent if possible. Either side can also attempt to correct any out-of-order handling or if the client begins to lag/desync, corrective action can be taken to try and get the client back in sync with the server. If the client falls too far behind though, it will be disconnected by the server. _(The client also has its own 'give-up' handling if it feels it cannot do whats needed to correct any out-of-sync problems it has.)_

There are two main sync counts that are used, one for packets the client sends to the server and one for packets the server sends to the client. The client also has a third counter, called `CheckCnt`, that it uses to ensure that either direction is properly aligned with the other and not getting to a point where one is out of order from the other. _(**Note:** The two sync counts will not remain in any kind of lock-step with each other, they will drift over time depending on multiple factors such as packet loss, lag, ignored sends, etc.)_
