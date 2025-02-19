# Lobby Server Protocol

The `lobby` server is used as the middle-man layer between PlayOnline and Final Fantasy XI.

It is responsible for several functions such as:

  - Game service account login; connecting the player to Final Fantasy's services.
  - Returning the available character list.
  - Returning the available world list.
  - Processing character selection requests.
  - Processing character creation requests.
  - Processing character deletion requests.
  - Processing character renaming requests.

_The lobby server has a few other functions in relation to GM client tooling, however only a single packet is left in the client related to it. (`RequestMoveGMChr`) While it is possible to force the client to try and send this packet, it is not generally intended to be done through any normal means._

## Network Usage

The `lobby` server processes its workflow through a TCP based socket, on port `54001`.

When Final Fantasy XI first starts, several game objects are initialized, one of them being the `GcMainSys`. This object is responsible for holding several key pointers to other objects as well as information related to some of those objects. For example, the `lobby` servers TCP object is held within this structure, along with the current set of IP addresses and ports being used to connect to the different services.

The current `ZoneIP`, `WorldIP`, `LobbyIP` and their respective ports are held in the parent `GcMainSys` object, while the actual created TCP socket used to do all `lobby` work is held within `GcMainSys->pTcpObject`. The search server _(referred to internally as the `cache` server)_ is also stored in this main structure as `GcMainSys->pCacheClient` with its IP address and port also stored under `cacheServ` and `cachePort`.

When the client initializes the `GcMainSys` object, it will prepare the address information for the main `lobby` server by resolving the domain: `ffxi00.pol.com`. This will return the current available `lobby` server IP. _(Square Enix makes use of load balancing, so the IP address you obtain when this resolving happens may not always be the same.)_ The port used is set through the games launch arguments, and is generally always: `-net 3 -port 54001`

After the needed objects are created and the server address is resolved, the client will begin its state machine loop to process the `lobby` transmissions.

## Socket Creation and Options

The `lobby` server socket is created and prepared with a few set features enabled and settings adjusted.

Pseudo code of how that creation looks like would be: _(excluding any error checking)_

```cpp
// Resolve the lobby server address..
const auto host = ::gethostbyname("ffxi00.pol.com");

// Prepare the socket address..
sockaddr_in addr{};
std::memcpy(&addr.sin_addr, host->h_addr_list[0], host->h_length);

// Create a TCP socket..
const auto sock = ::socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);

// Set the socket to be non-blocking..
u_long arg = 1;
::ioctlsocket(sock, FIONBIO, &arg);

// Set additional socket settings..
::setsockopt(sock, SOL_SOCKET, SO_DEBUG, reinterpret_cast<const char*>(&arg), 4);
::setsockopt(sock, SOL_SOCKET, SO_REUSEADDR, reinterpret_cast<const char*>(&arg), 4);

// Set the socket buffer sizes..
arg = 0x10000;
::setsockopt(sock, SOL_SOCKET, SO_RCVBUF, reinterpret_cast<const char*>(&arg), 4);
::setsockopt(sock, SOL_SOCKET, SO_SNDBUF, reinterpret_cast<const char*>(&arg), 4);

// Connect to the lobby server..
::connect(sock, reinterpret_cast<const struct sockaddr*>(&addr), sizeof(sockaddr_in));
```

## General Packet Flow

After the socket is created, the actual communication flow will begin. The communication between the client and the server are done in a normal request / response pattern, where the client will make a request and the server will respond. In the current context and known information, there are no requests made from the server to the client. All communications expect an initial request to be made by the client first.

Below is a list of normal traffic flows between the client and server under the given conditions:

### Client Login (Successful)

| Direction | Packet | Notes |
|--- |--- |--- |
| `C -> S` | `RequestLobbyLogin`    | The client sends the initial login request. This packet contains the clients authentication ticket/session, the client version, and flags that signify which expansions are locally installed. |
| `S -> C` | `ResponseKey`          | The server responds telling the client their login was successful. This packet contains the MD5 key for the client to use, information on what expansions are available on the server, and what extra features the client has unlocked. _(security token, wardrobes, etc.)_ |
| `C -> S` | `RequestGetChr`        | The client sends a request to obtain a list of their available content ids (characters). |
| `S -> C` | `ResponseChrInfo2`     | The server responds with the list of the clients available content ids. |
| `C -> S` | `RequestQueryWorldList`| The client sends a request to obtain a list of the available servers list. |
| `S -> C` | `ResponseWorldList`    | The server responds with the list of available servers. |

### Client Character Creation (Successful)

| Direction | Packet | Notes |
|--- |--- |--- |
| `C -> S` | `RequestCreateChrPre`  | The client sends the pre-creation request packet. This packet is used to validate the chosen name and gold world pass. _(If one was given.)_ |
| `S -> C` | `ResponseOk`           | The server responds with 'ok' to tell the client its request was valid and completed. |
| `C -> S` | `RequestCreateChr`     | The client sends the final creation request packet. This packet contains their other creation choices. _(ie. race, face, size, job, nation, etc.)_ |
| `S -> C` | `ResponseOk`           | The server responds with 'ok' to tell the client its request was valid and completed. |
| `C -> S` | `RequestGetChr`        | The client sends a request to obtain a list of their available content ids (characters). |
| `S -> C` | `ResponseChrInfo2`     | The server responds with the list of the clients available content ids. |

### Client Character Selection (Successful)

| Direction | Packet | Notes |
|--- |--- |--- |
| `C -> S` | `RequestSelectChr`     | The client sends a request with the selected character they'd like to play. |
| `S -> C` | `ResponseNextLogin`    | The server responds with the needed information for the client to properly connect to the game server the character is registered to. _This packet also contains the proper search server IP address and port._ |

### Client Character Deletion (Successful)

| Direction | Packet | Notes |
|--- |--- |--- |
| `C -> S` | `RequestDeleteChr`     | The client sends a request to delete the selected character. |
| `S -> C` | `ResponseOk`           | The server responds with 'ok' to tell the client its request was valid and completed. |
| `C -> S` | `RequestGetChr`        | The client sends a request to obtain a list of their available content ids (characters). |
| `S -> C` | `ResponseChrInfo2`     | The server responds with the list of the clients available content ids. |

### Exiting FFXI Back To Main Menu (Successful)

When the client exits FFXI gameplay back to the games main menu _(Select Character, Create Character, etc.)_, it will perform the same actions as the normal **Client Login** steps.

## General Packet Flow (Errors)

If any of the clients requests are invalid, the server will respond with an `ResponseError` packet instead of any other normal packet seen in the flows above. This packet contains the error code that should be translated and displayed to the client, informing them of what went wrong.

Like the flows above, here are some examples of errors happening during the different client requests:

### Client Character Creation (Error)

_Client requests a name that is already taken, invalid due to vulgar words, etc. or the given world pass is invalid:_

| Direction | Packet | Notes |
|--- |--- |--- |
| `C -> S` | `RequestCreateChrPre`  | The client sends the pre-creation request packet. This packet is used to validate the chosen name and gold world pass. _(If one was given.)_ |
| `S -> C` | `ResponseError`        | The server responds with an error code. |

_Client requests to create a character with invalid information, such as an invalid/unavailable job id:_

| Direction | Packet | Notes |
|--- |--- |--- |
| `C -> S` | `RequestCreateChrPre`  | The client sends the pre-creation request packet. This packet is used to validate the chosen name and gold world pass. _(If one was given.)_ |
| `S -> C` | `ResponseOk`           | The server responds with 'ok' to tell the client its request was valid and completed. |
| `C -> S` | `RequestCreateChr`     | The client sends the final creation request packet. This packet contains their other creation choices. _(ie. race, face, size, job, nation, etc.)_ |
| `S -> C` | `ResponseError`        | The server responds with an error code. |

### Client Character Selection (Error)

_Client requests an invalid content id to play: (ie. invalid slot, non-owned id, locked character (for non-payment), etc.)_

| Direction | Packet | Notes |
|--- |--- |--- |
| `C -> S` | `RequestSelectChr`     | The client sends a request with the selected character they'd like to play. |
| `S -> C` | `ResponseError`        | The server responds with an error code. |

### Client Character Deletion (Error)

_Client requests an invalid content id to delete: (ie. invalid slot, non-owned id, locked character (for non-payment), etc.)_

| Direction | Packet | Notes |
|--- |--- |--- |
| `C -> S` | `RequestDeleteChr`     | The client sends a request to delete the selected character. |
| `S -> C` | `ResponseError`        | The server responds with an error code. |

## Message Strings & Error Codes

When the client attempts to request something that is invalid, or otherwise causes an error, the server will respond with the `ResponseError` packet. This packet contains an error code _(via the `err_code` field)_ which is used to translate a numerical value to a readable string containing what went wrong. _Not all error codes are useful in regards to being translated to a full error string, some are just shown as a `FFXI-%d` style error code with no message._

When `lobby` server errors are being processed, they are handled in a specific manner. First, the client will add `3000` to the error code and perform the lookup using a series of internal function calls. Next, a few errors are looked for specifically, in relation to invalid character name. These errors are:

  - `313` _(internally `3313`)_
  - `322` _(internally `3322`)_
  - `335` _(internally `3335`)_

_These errors are used to mark that a character name is already in-use and cannot be used when creating a new character or during a character rename._

When the error is one of the above values, then a special case happens that translates and returns the error string via a call made through `polcore.dll`. _(The call is made to: `sqPlayOnlineGetFixedFormResourceString`)_

Next, if the error is not one of those values, a static lookup table is iterated to check for several more specific protocol errors. This table has two purposes, one is to hold an error to table and string index lookup, but also to handle a callback type which handles how the game client should treat the error. There are 3 error modes that can be used:

  - `XiErrDisposeReboot` - _The error causes the client to terminate._
  - `XiErrDisposeLogout` - _The error causes the client to logout._
  - `XiErrDisposeDialog` - _The error causes the client to display a simple dialog._

The error codes that are used with this lookup table are as follows:

| Error Code | Message |
|--- |--- |
| `2001` | `Connection to server lost. (PlayOnline)\nReturning to the PlayOnline FINAL FANTASY XI top page.` |
| `2002` | `Unable to reserve required memory. Press OK to shut down.` |
| `2003` | `Failed to read data.` |
| `2100` | `(null)` |
| `3001` | `No response from the FINAL FANTASY XI server.\nConnection timed out.\nPress OK to shut down.` |
| `3100` | `Could not connect to lobby server.` |
| `3101` | `Transmission error with lobby server.` |
| `3102` | `Transmission packet error #1.` |
| `3103` | `Transmission packet error #2。` |
| `3109` | `Connection to the PlayOnline server lost.\nReturning to the PlayOnline FINAL FANTASY XI top page.` |
| `3110` | `Character names must be at least three letters in length.` |
| `3111` | `Gold World Pass too short to be valid.` |
| `3112` | `Unless you enter a Gold World Pass,\nyou can only specify a world server\nwhere you already have a character.` |
| `3113` | `Protocol time out error.` |
| `3114` | `Inconsistency occurred with handle information.` |
| `3115` | `Connection to the FINAL FANTASY XI server lost.\nReturning to the PlayOnline FINAL FANTASY XI top page.` |
| `3116` | `Cannot select this character.\nCharacter's content ID is no longer valid.\nPlease restore the content ID to select this character.` |
| `3117` | `Failed to carry out user file operations.` |
| `3118` | `The world server you specified does not exist.\nPlease check the spelling of the world name\nbefore trying again.` |
| `3119` | `World name not specified.` |
| `3120` | `Content ID for selected character not found.\nPlease reboot PlayOnline.\n` |
| `3201` | `Same character is already logged in.\nPlease wait a few minutes before trying to reconnect.` |
| `3202` | `Your character data may be damaged.\nPlease contact PlayOnline support services.` |
| `3203` | `Unable to connect to area server.` |
| `3204` | `Failed to carry out data creation #1 on world server` |
| `3205` | `Failed to carry out data creation #2 on world server` |
| `3206` | `Unable to log on to area server.\nServer currently congested.` |
| `3207` | `Unable to connect to area server.` |
| `3208` | `Unable to log on to world server.\nServer currently congested.` |
| `3214` | `Expansion pack area not registered.` |
| `3215` | `Expansion pack area not installed.` |
| `3300` | `This protocol is not supported.` |
| `3301` | `There has been a communication error with the FINAL FANTASY XI server.\nPlease log out from PlayOnline and wait several\nminutes before logging in again.` |
| `3302` | `Authentication failed. Password incorrect.` |
| `3303` | `Unable to find character.` |
| `3304` | `Unable to find content ID.` |
| `3305` | `Unable to connect to world server.\nSpecified operation failed.` |
| `3306` | `Login search server returned an error.` |
| `3307` | `Same character is already logged in.\nPlease wait a few minutes before trying to reconnect.` |
| `3308` | `Could not connect to world server.\nPlease check this title's news for announcements.` |
| `3309` | `Registration with lobby server failed.` |
| `3310` | `Registration #1 with world server failed.` |
| `3311` | `Registration #2 with world server failed.` |
| `3312` | `Failed to delete from world server.` |
| `3313` | `入力した名前は、\nすでに使用されているなどの理由で登録できません。\nほかの名前を入力してください。` |
| `3314` | `Failed to register with name server.` |
| `3315` | `Internal error #1.` |
| `3316` | `Incorrect Gold World Pass.` |
| `3317` | `Gold World Pass expired.` |
| `3318` | `Failed to delete character.` |
| `3319` | `Internal error #2.` |
| `3320` | `Internal error #3.` |
| `3321` | `Character's parameters are incorrect.` |
| `3322` | `入力した名前は、\nすでに使用されているなどの理由で登録できません。\nほかの名前を入力してください。` |
| `3324` | `Your Gold World Pass is not valid for\nthe world you have specified.` |
| `3325` | `Internal error #4. Please try to create a new character again.` |
| `3326` | `Failed to cancel character reservation.` |
| `3327` | `Unable to create a character on the specified world server.\nServer population limit reached.` |
| `3329` | `Transmission packet error #3.` |
| `3330` | `Unable to delete.\nSpecified character is currently logged in.` |
| `3331` | `The game's data has been updated.\nPlease return to the PlayOnline FINAL FANTASY XI\ntop page and download the latest version.` |
| `3332` | `Could not connect to lobby server.\nPlease check this title's news for announcements.` |
| `3334` | `Internal error #6.` |
| `3335` | `入力した名前は、\nすでに使用されているなどの理由で登録できません。\nほかの名前を入力してください。` |
| `3336` | `Registration #3 with world server failed.` |
| `3337` | `Internal error #5.` |
| `3338` | `Internal error #7.` |
| `3339` | `A Gold World Pass is valid\nfor your first character only.` |
| `4001` | `No response from the FINAL FANTASY XI server.\nConnection timed out.\nPress OK to shut down.` |
| `4002` | `Time out reported by the FINAL FANTASY XI server.\nPress OK to shut down.` |
| `4003` | `Connection to the FINAL FANTASY XI server lost. Press OK to shut down.` |
| `4004` | `Connection to the FINAL FANTASY XI server lost. Press OK to shut down.` |
| `4005` | `Connection to the FINAL FANTASY XI server lost. Press OK to shut down.` |
| `4006` | `Connection to the FINAL FANTASY XI server lost. Press OK to shut down.` |

_**Note:** As a reminder, these values have `3000` added to them. Meaning if the server sent you the error code `321`, it is then translated to `3321` before being processed._

Other general errors are able to happen while working with the `lobby` server. These are looked up through preloaded string tables using the following two DAT files:

### English DAT Files

| File Id | File Path | Purpose |
|--- |--- |--- |
| `48`      | `ROM\97\36.DAT`   | Contains `lobby` server general status messages. |
| `55646`   | `ROM\165\70.DAT`  | Contains general error messages used for both the `lobby` server and in-game conditions. |

### Japanese DAT Files

| File Id | File Path | Purpose |
|--- |--- |--- |
| `30`      | `ROM\97\8.DAT`    | Contains `lobby` server general status messages. |
| `55526`   | `ROM\165\54.DAT`  | Contains general error messages used for both the `lobby` server and in-game conditions. |
