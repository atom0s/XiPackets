# `Unknown - Packet: 0x0069`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `(Unknown)` |
| **Client Handler**        | `(Unknown)` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0069` |
| **Size**                  | `0x00C8` |

## Description

This packet is sent by the server in relation to the chocobo racing event. It has multiple purposes related to this event.

## Packet Layout

The initial layout of this packet is the following:

```cpp
// PS2: (New; did not exist.)
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Mode;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`packet_t`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/HEADER.md)_

### `Mode`

_The packet mode._

This value is used to determine how the data within this packet should be handled. The client currently will handle the following modes:

| Mode | Purpose |
| --- | --- |
| `1` | _Used to update the `ChocoboRacingSys.RacingParams` data._ |
| `2` | _Used to update the `ChocoboRacingSys.ChocoboParams` data._ |
| `3` | _Used to update the `ChocoboRacingSys.SectionParams` data._ |
| `4` | _Used to update the `ChocoboRacingSys.ResultParams` and `ChocoboRacingSys.SectionParams` data._ |
| `5` | _The final mode used to inform the client all other data has been populated._ |

_**Note:** Retail uses `Mode` 5 to set the `ChocoboRacingSys.DownloadFlg` to 1, which marks the system as fully populated and ready to be used. However, it will treat any non-listed `Mode` value in the same manner as a default behavior._

## Mode: `1`

This mode is used to update the `ChocoboRacingSys.RacingParams` data.

The packet layout when this mode is sent is as follows:

```cpp
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Mode;
    uint8_t     padding00[3];
    uint32_t    RaceParams[2];
    uint8_t     junk00[];
};
```

### `padding00`

_Padding; unused._

### `RaceParams`

_The chocobo system racing params._

### `junk00`

_Junk; unused._

The size of this array will be the rest of the packet data. It is not used and ignored by the client.

## Mode: `2` & `3`

These modes are used to update the `ChocoboRacingSys.ChocoboParams` and `ChocoboRacingSys.SectionParams` respectively. They share the same packet layout thus are being combined here.

The packet layout when these modes are sent is as follows:

```cpp
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Mode;
    uint8_t     ParamIndex;
    uint8_t     ParamSize;
    uint8_t     padding00;
    uint8_t     ParamData[ParamSize];
    uint8_t     junk00[];
};
```

### `ParamIndex`

_The parameter block index._

This value is used to tell the client where to begin indexing the array the data will be written to. This value is multiplied by 3.

### `ParamSize`

_The parameter data size._

This value is used to determine the size of the `ParamData` array.

### `padding00`

_Padding; unused._

### `ParamData`

_The parameter data._

### `junk00`

The size of this array will be the rest of the packet data. It is not used and ignored by the client.

_This field is not always present in the packet! If the packet matches the maximum length of this packet type (`0xC8` bytes) then this field will not be included._

## Mode: `4`

This mode is used to update the `ChocoboRacingSys.ResultParams` and `ChocoboRacingSys.SectionParams` data.

The layout for this packet will change based on the data that is included in it as well as the size of the initial block of data.

```cpp
struct packet_t
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint8_t     Mode;
    uint8_t     padding00;
    uint8_t     ParamSize;
    uint8_t     padding01;
    uint8_t     ParamData1[];
    uint8_t     ParamData2[];
};
```

_**Note:** Please read the notes below before assuming how this layout is setup!_

### `padding00`

_Padding; unused._

### `ParamSize`

_The parameter size._

This value is used to determine the size of both `ParamData1` and `ParamData2`. There is a possibility that the `ParamData2` value is not present in this packet depending on this size.

### `padding01`

_Padding; unused._

### `ParamData1`

_The parameter data. (1)_

This value holds the `ChocoboRacingSys.ResultParams` data. The client determines the length of this data by doing the following: `4 * (ParamSize >> 2)`

### `ParamData2`

_The parameter data. (2)_

This value holds the `ChocoboRacingSys.SectionParams` data. This value is not always present in the packet and will be dependent on the `ParamSize` to determine if it is. The client determines the length of this data by doing the following: `ParamSize & 7`

When this value is present, the position its data is stored in the packet is located at: `4 * (ParamSize >> 2) + 8`

## Mode: `5` _(And all others; default case.)_

This mode is used as the default case when no other defined mode is being handled. When this happens, the client ignores all data in the packet and will only set the `ChocoboRacingSys.DownloadFlg` to 1, marking the system as fully updated and ready for use.

The packet layout for this mode does not matter. Only the `Mode` value will be checked/used. The rest of the packet is ignored and not used.

## Additional Information

This packet is used to update the `ChocoboRacingSys` information block in the client. Modes `1`, `2`, `3`, and `4` will always reset `ChocoboRacingSys.DownloadFlg` back to zero. When this happens the system is considered 'stale' and the client will not use any data that is stored in it. The server will always end a block of data being sent and updated in this system with a `Mode` 5 packet which tells the client to set `ChocoboRacingSys.DownloadFlg` to 1, marking it ready for use.

The general flow of this packet usage is as follows:

  - The server will send a `Mode` 1 packet, updating the `RacingParams` information.
    - _`ChocoboRacingSys.DownloadFlg` is set to `0`._
  - The server will send a `Mode` 2 packet, updating the `ChocoboParams` information.
    - _`ChocoboRacingSys.DownloadFlg` is set to `0`._
  - The server will send a `Mode` 3 packet, updating the `SectionParams` information.
    - _`ChocoboRacingSys.DownloadFlg` is set to `0`._
  - The server will send a `Mode` 4 packet, updating the `ResultParams` and `SectionParams` information.
    - _`ChocoboRacingSys.DownloadFlg` is set to `0`._
  - The server will send a `Mode` 5 packet, marking the system ready for use.
    - _`ChocoboRacingSys.DownloadFlg` is set to `1`._

During this flow, it is possible the server will send multiple `Mode` 2 and `Mode` 3 packets to update different blocks of data. When result information sent to the client, the server will send [at least] two packets to handle this information, which will be a `Mode` 4 and `Mode` 5 packet. _(Again, the `Mode` 5 packet is sent to remark the system as ready for use.)_
