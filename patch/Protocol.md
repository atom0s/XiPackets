<div align="center">
    <img width="200" src="https://github.com/atom0s/XiPackets/raw/main/repo/icon.png" alt="XiPackets">
    </br>
</div>

# Patch Server Protocol

This documentation will outline the various information related to the `patch` server operations. The following block of information relates to the manner in which the `patch` server functions:

| Name | Information |
| ---: | --- |
| **Encryption:**   | `None (*)` |
| **Protocol:**     | `TCP` |
| **Port(s):**      | `54000`, `53001` |

> [!NOTE]
> While the general packet information from the `patch` server is not encrypted, there are cases where data is compressed to save space!

> [!NOTE]
> The listed ports above are based on the English and Japanese PlayOnline and Final Fantasy XI clients. Additional ports may be used for other applications/titles that connect to the PlayOnline services.

## Patch Server Addresses

The `patch` server makes use of load balancing based on the connecting clients region as well as the title information being requested. This means that English and Japanese clients will attempt to connect to different servers when handling `patch` server related operations. This also includes balancing based on the title _(ie. `PlayOnline`, `Final Fantasy XI`, etc.)_ that the client is requesting version related information for.

The following server information has been observed using the Windows based English and Japanese clients for `PlayOnline` and `Final Fantasy XI`:

| Client | Language | Address | Port |
| :---: | :---: | :---: | :---: |
| `PlayOnline`          | `NA` | `pt008.pol.com`   | `54000` |
| `PlayOnline`          | `JP` | `pt004.pol.com`   | `54000` |
| `Final Fantasy XI`    | `NA` | `pc001W2U.pol.com` | `53001` |
| `Final Fantasy XI`    | `JP` | `pc001W20.pol.com` | `53001` |

The following servers have been observed to be used and valid over the last several years:

| Client | Addresses |
| :---: | --- |
| `PlayOnline`          | `pt002.pol.com`, `pt004.pol.com`, `pt007.pol.com`, `pt008.pol.com` |
| `Final Fantasy XI`    | `pc001W20.pol.com`, `pc002W20.pol.com`, `pc001W2U.pol.com`, `pc002W2U.pol.com` |

The following formatters are used to build the patch domains:

  - The patch domain used for `PlayOnline` is formatted as: `pt%03d.pol.com`
  - The patch domain used for `Final Fantasy XI` is formatted as: `pc%03d%s.pol.com`

_The `PlayOnline` client determines which patch server it will connect to based on some internal factors. However, the main patch server URL that is used is stored inside of the encrypted `env.dat` file found within the `/data/utf/` folder. This file contains the various environment variables that are used to configure `PlayOnline` for the clients current region. This includes things such as the client country code, various website urls that are used throughout the client, the default language code, various domain urls for different services that the client will connect to, and such._

## Packet Encryption

The `patch` server does not encrypt its packets in either direction. Instead, the traffic is sent unencrypted but can contain compressed data depending on the response being sent from the server. When the client requests a file chunk to be downloaded for an update, the server can send that chunk back compressed. Other than this basic compression, there is no actual encryption used for this servers traffic.

## Packet Header

The `patch` server makes use of a common header for all of its packets. The packet header is as follows:

```cpp
struct header_t
{
    uint32_t size;
    uint32_t hash;
    uint32_t signature;
    uint32_t opcode;
};
```

### `size`

_The packet size._

This value is the total size of the packet, including this full header.

### `hash`

_The packet MD5 hash._

This value is the partial MD5 hash of the packet data, excluding the `size` and `hash` fields. The hash value is trimmed and only the first four bytes are kept which are stored in this value.

### `signature`

_The packet signature._

This value is hardcoded as `0x504C4F50` in all `patch` server packets. _(This is `POLP` in ASCII.)_

### `opcode`

_The packet opcode._

This value is used to determine the type of packet being used. Please see the [`patch server packets`](/patch/packets/README.md) for more information.

## Socket Creation and Options

The following pseudo code demonstrates how the `patch` server socket is created and prepared for usage _(excluding any error checking)_:

```cpp
// Resolve the patch server address..
const auto host = ::gethostbyname("pt008.pol.com");

// Prepare the socket address..
sockaddr_in addr{};
std::memcpy(&addr.sin_addr, host->h_addr_list[0], host->h_length);

// Create a TCP socket..
const auto sock = ::socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);

// Set the socket to be non-blocking..
u_long arg = 1;
::ioctlsocket(sock, FIONBIO, &arg);

// Listen for inbound connections on the socket..
::bind(sock, reinterpret_cast<const struct sockaddr*>(&addr), sizeof(sockaddr_in));

// Set the socket buffer sizes..
arg = 0x10000;
::setsockopt(sock, SOL_SOCKET, SO_RCVBUF, reinterpret_cast<const char*>(&arg), 4);
::setsockopt(sock, SOL_SOCKET, SO_SNDBUF, reinterpret_cast<const char*>(&arg), 4);

// Connect to the lobby server..
::connect(sock, reinterpret_cast<const struct sockaddr*>(&addr), sizeof(sockaddr_in));

// Check for errors..
::getsockopt(sock, SOL_SOCKET, SO_ERROR, reinterpret_cast<const char*>(&arg), 4);

// Additional handling (ie. send/recv) happens here..
```

## Packet Flow

The `patch` server makes use of a basic request-response communication approach in which the client will connect to the server, make a request and wait for a response before taking further action. It is expected that the client will make the first request in order for the server to send a response. _(If the client does not send any request to the server after connecting, the server will eventually timeout the connection and close the socket without sending any data. At this time, there has not been any observed instances in which the server sends a request to the client first.)_

During the total operation(s) involving the `patch` server, the client may disconnect and reconnect to the server multiple times. The same socket will not be used to complete every exchange. Instead, it is common for the client to connect to the server, make a single request, read the response then disconnect while it processes the information that it received. It will then reconnect to the server to do further requests as needed.

The most common usage of the `patch` server is when the client performs an 'ask newest' request about a given hardware/application combination. This kind of check is performed every single time the client attempts to log into the `PlayOnline` services. It is also performed any time the client attempts to launch a supported title within `PlayOnline` that makes use of the in-house versioning system, such as `Final Fantasy XI`.

### Ask Newest Request

| Direction | OpCode | Notes |
| :---: | :---: | --- |
| `C -> S` | `N/A`      | _Client connects to the `patch` server._ |
| `C -> S` | `0x0007`   | _Client sends an 'ask newest' request._ |
| `S -> C` | `0x0008`   | _Server sends an 'ask newest' response._ |
| `C -> S` | `N/A`      | _Client disconnects from the `patch` server._ |

This exchange begins with the client first connecting to the `patch` server and then sending an 'ask newest' request packet. This packet will contain the clients current hardware and application id's, telling the server which title they wish to obtain the latest information of. Assuming the packet is properly constructed and the information is valid, the server will then send back an 'ask newest' response containing the information about the requested title. The client will then disconnect from the server and process the information, determining if a version update will be needed.

If no update is required, the client will continue connecting to the desired service. Otherwise, the client will begin the version update process.

### Version Status Request

| Direction | OpCode | Notes |
| :---: | :---: | --- |
| `C -> S` | `N/A`      | _Client connects to the `patch` server._ |
| `C -> S` | `0x0001`   | _Client sends a 'status' request._ |
| `S -> C` | `0x0002`   | _Server sends a 'status' response._ |
| `C -> S` | `N/A`      | _Client disconnects from the `patch` server._ |

This exchange works similar to the 'ask newest' request above. The client will send a 'status' request packet containing the hardware and application id's, telling the server which title they wish to obtain the latest information of. Rather than responding with just a basic version string like in the 'ask newest' response, the server will instead send a 'status' response which contains the given titles `patch.cfg` file back to the client.

The `patch.cfg` file is a custom formatted file that tells the patch system within the client how it should handle updating the various files available for the requested title. Upon receiving this file, the client will disconnect from the server and process the file. During this processing, the client will determine which local files are outdated _(or missing)_ and need to be updated. Once the file has been processed and the list of local files that need to be updated is processed and built, the client will reconnect to the `patch` server again and begin requesting files from the server.

### Download Requests

| Direction | OpCode | Notes |
| :---: | :---: | --- |
| `C -> S` | `N/A`      | _Client connects to the `patch` server._ |
| `C -> S` | `0x0003`   | _Client sends a 'download' request._ |
| `S -> C` | `0x0004`   | _Server sends a 'download' response._ |

During this exchange, the client will send file 'download' requests to the server asking to download parts of a file that are located on the remote server, based on the information that was built using the `patch.cfg` file data. _(The manner in which this exchange works is designed to work with much older internet technologies, back when dial-up internet was the common means of connecting to the game. Due to this, the manner in which files are downloaded is a bit slow.)_ The client will generate multiple requests for a single file, breaking it into chunks that will be requested from the server in order to download the entire file. For example, if a file is 4096 bytes long, the client can create multiple requests to ask for parts of the file in smaller byte chunks _(ie. 512 bytes)_. The request packets that are sent by the client contains the starting offset into the file and the amount of data it wishes to receive in each chunk. The server will then respond with a single response packet for each requested chunk, followed by additional data packets containing the remaining data to fulfil the request.

> [!NOTE]
> Please see the information below regarding packet sizes and how larger size packets are handled!

The client will generally reuse the same socket during this step of the `patch` server process, sending additional requests for more files/chunks as things are received. It is possible for the client to disconnect and reconnect during this phase in the event of any kind of issues, such as timeouts or other server side errors.

_If it was not already obvious, this is the reason why the patching process through `PlayOnline` is so slow. A single socket is being used to download files in parts, only allowing a single part to be downloaded at a time. There is no kind of multithreading or multi-file downloading happening during this process._

## Response Packet Handling

When requests are being made to the `patch` server the client will handle reading responses by performing some error checking and initial buffer size reading in a special manner. Rather than blindly reading the receive buffer from the socket, it will instead 'peek' at the first four bytes of the buffer _(which contains the packet header `size` field value)_ to determine how much information should be read. This allows the client to prepare the buffer for the expected size of data that will be read as well as allows it to know when the full file has been received and the next request can begin.

When the server sends a response, the response can be broken into parts based on multiple factors such as the buffer limitation as well as the standard MTU limitations of the protocol. In the event that a packet needs to be broken into parts, only the first part of the packet will contain a header. The rest of the packets following will simply be the continuation of the packet data that is used to rebuild the full packet on the client.

After the client sends a request, it will then check for any errors on the socket and call 'select' to wait for readability. Once the socket is ready for reading, the client will 'peek' the first four bytes in the buffer to determine the total size of the incoming response. The below pseudo code demonstrates what this looks like. _(Please keep in mind, this is heavily stripped down, with error handling removed and the state machine it is within flattened.)_

```cpp
int32_t val = 0;
int32_t len = 4;
if (::getsockopt(sock, SOL_SOCKET, SO_ERROR, reinterpret_cast<char*>(&val), &len) == SOCKET_ERROR)
{
    // snipped error handling..
}

// Obtain the packet size from the buffer, using peek flag to not remove the data from the buffer..
const auto ret = ::recv(sock, reinterpret_cast<char*>(len), 4, MSG_PEEK);
if (ret == 0)
{
    // handle server disconnect..
}
else if (ret < 4)
{
    // wait for more data..
}

if (ret < 8 || ret > 93440)
{
    // handle unexpected data length..
}

// Obtain the available size of data that can be read from the socket..
if (::ioctlsocket(sock, FIONREAD, &len) == -1)
{
    // snipped error handling..
}

// Read the packet from the buffer until the full packet has been received..
do
{
    ret = ::recv(sock, buffer + offset, len, 0);
    if (ret <= 0)
    {
        // handle server disconnect and errors..
    }

    offset += ret;
    len -= ret;

} while (len > 0);
```

## Error Responses

In the event that the client has made an invalid request to the server, or a server-side issue has occurred, the server will generally respond with an error response. This response is a simple packet that only contains the packet header shown above. It does not contain any kind of additional information or specific error code. Instead, it is up to the client to determine the error based on where in the patching process it has failed and received the error. It is also possible that the server will not send any error response and will instead simply disconnect to the client by force.

The below tables contain information on how the server responds to different error conditions for each request packet.

### `Status` Request [OpCode: 0x0001]

| Modification | Notes |
| --- | --- |
| `Invalid Request Size`            | _Server disconnects the client with no response._ |
| `Invalid Request Hash`            | _Server disconnects the client with no response._ |
| `Invalid Request Signature`       | _Server disconnects the client with no response._ |
| `Invalid Request Application Id`  | _Server disconnects the client with an error response packet._ |
| `Invalid Request Hardware Id`     | _Server disconnects the client with an error response packet._ |

### `Download` Request [OpCode: 0x0003]

| Modification | Notes |
| --- | --- |
| `Invalid Request Size`            | _Server disconnects the client with no response._ |
| `Invalid Request Hash`            | _Server disconnects the client with no response._ |
| `Invalid Request Signature`       | _Server disconnects the client with no response._ |
| `Invalid Request Application Id`  | _Server disconnects the client with an error response packet._ |
| `Invalid Request Hardware Id`     | _Server disconnects the client with an error response packet._ |
| `Invalid Request Chunk Offset`    | _Server disconnects the client with an error response packet._ |
| `Invalid Request Chunk Size`      | _Server disconnects the client with an error response packet._ |
| `Invalid Request File Name`       | _Server disconnects the client with an error response packet._ |

_**Note:** It is possible for an invalid chunk offset or size to cause the server to simply disconnect the client as well._

### `Shutdown` Request [OpCode: 0x0006]

| Modification | Notes |
| --- | --- |
| `Invalid Request Size`            | _Server disconnects the client with no response._ |
| `Invalid Request Hash`            | _Server disconnects the client with no response._ |
| `Invalid Request Signature`       | _Server disconnects the client with no response._ |

### `Ask Newest` Request [OpCode: 0x0007]

| Modification | Notes |
| --- | --- |
| `Invalid Request Size`            | _Server disconnects the client with no response._ |
| `Invalid Request Hash`            | _Server disconnects the client with no response._ |
| `Invalid Request Signature`       | _Server disconnects the client with no response._ |
| `Invalid Request Application Id`  | _Server disconnects the client with an error response packet._ |
| `Invalid Request Hardware Id`     | _Server disconnects the client with an error response packet._ |

_**Note:** The server appears to be very 'relaxed' about accepting random junk data with this request, such as an invalid or garbage version string in the request. The version string sent from the client does not appear to have any condition that will cause an error response from the server._