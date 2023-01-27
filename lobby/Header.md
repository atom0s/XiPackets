# Lobby Packet Header

Every packet sent between the client and the `lobby` server shares a common header. This header holds the basic information related to the packet.

## Header Structure

```cpp
struct packet_t
{
    //
    // Packet Header
    //

    uint32_t        packet_size;                // PS2: packet_size
    uint32_t        terminator;                 // PS2: terminator
    uint32_t        command;                    // PS2: command
    uint8_t         identifer[16];              // PS2: identifer

    //
    // Packet Data
    //

    uint8_t         data[];                     // PS2: Per-packet fields replace this.
};
```

## Packet Fields

### packet_size

_The total size of the packet, including the header information._

### terminator

_The lobby server packet signature._ _(`FFXI`)_

### command

_The opcode of the packet, signifying its purpose._

### identifer

_The MD5 checksum hash of the full packet._

_The checksum is of the full packet (including the header data) with the `identifer` data nulled._

### data

_The packet-specific data that relates to the given opcode. This field is replaced on the various packet information pages with the actual fields that populate the packets additional data._
