# `GP_SERV_COMMAND_LOGOUT`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_LOGOUT` |
| **Client Handler**        | `RecvLogOut` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x000B` |
| **Size**                  | `0x001C` |

## Description

This packet is sent by the server to respond to a client zone request. The client sends different kinds of requests related to this packet. For example, when walking between zones, the client will send a zone line request (`0x5B`) packet which can trigger this response. The client will also send a logout request when exiting the game _(ie. via `/shutdown`)_ which will also see this packet response.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_GAME_LOGOUT_STATE
enum GP_GAME_LOGOUT_STATE
{
    GP_GAME_LOGOUT_STATE_NONE,
    GP_GAME_LOGOUT_STATE_LOGOUT,
    GP_GAME_LOGOUT_STATE_ZONECHANGE,
    GP_GAME_LOGOUT_STATE_MYROOM,
    GP_GAME_LOGOUT_STATE_CANCELL,
    GP_GAME_LOGOUT_STATE_POLEXIT,
    GP_GAME_LOGOUT_STATE_JOBEXIT,
    GP_GAME_LOGOUT_STATE_POLEXIT_MYROOM,
    GP_GAME_LOGOUT_STATE_TIMEOUT,
    GP_GAME_LOGOUT_STATE_GMLOGOUT,
    GP_GAME_LOGOUT_STATE_END
};

// PS2: GP_GAME_ECODE
enum GP_GAME_ECODE
{
    GP_GAME_ECODE_NOERR,
    GP_GAME_ECODE_RESERR,
    GP_GAME_ECODE_ZONEDOWN,
    GP_GAME_ECODE_REGERR,
    GP_GAME_ECODE_CLIVERSERR,
    GP_GAME_ECODE_CLINOEXERR,
    GP_GAME_ECODE_UNKNOWN,
    GP_GAME_ECODE_MAX
};

// PS2: GP_SERV_LOGOUT
struct GP_SERV_LOGOUT
{
    uint16_t                id: 9;
    uint16_t                size: 7;
    uint16_t                sync;
    GP_GAME_LOGOUT_STATE    LogoutState;
    uint8_t                 Iwasaki[16];
    GP_GAME_ECODE           cliErrCode;
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_LOGOUT`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `LogoutState`

_The logout state type that triggered this response._

### `Iwasaki`

_Buffer containing data based on the `LogoutState` value._

### `cliErrCode`

_The client error code, if set._

## Additional Information

As stated above, this response can be triggered by multiple causes. It generally depends on how the client has requested to 'logout' of a given zone, or if they are being removed by force. _(ie. Disconnection, Error, Timeout, etc.)_

_**Please note:** The enumeration values shared for `GP_GAME_LOGOUT_STATE` and `GP_GAME_ECODE` are not up to date with current retail. These have likely changed since then and now include additional entries!_

The `LogoutState` value is used to determine how the client is being logged out. This response is named in a potentially misleading way for those not familiar with the games server setup. Because zones are clustered, when you request to zone to another area, you have to be approved and then handed off to the next server. This causes your client to fully disconnect from one server and reconnect to another. Thus the phrase 'logout' is used to explain this handoff as well as when fully logging out of the game.

The client currently only checks for the following `LogoutState` values:

  - `1` - `GP_GAME_LOGOUT_STATE_LOGOUT` - _The client is being logged out and not handed off to another zone._
  - `4` - `GP_GAME_LOGOUT_STATE_CANCELL` - _The client logout request has been cancelled; the client will remain in the same zone._
  - `8` - `GP_GAME_LOGOUT_STATE_TIMEOUT` - _The client has timed out, or an error occurred on the server side._

The values stored within `Iwasaki` and `cliErrCode` will depend on the current `LogoutState` value.

### `GP_GAME_LOGOUT_STATE_LOGOUT`

When this `LogoutState` type is received, the values for both `Iwasaki` and `cliErrCode` should both be all 0's.

### `GP_GAME_LOGOUT_STATE_CANCELL`

When this `LogoutState` type is received, the values for both `Iwasaki` and `cliErrCode` should both be all 0's.

### `GP_GAME_LOGOUT_STATE_TIMEOUT`

When this `LogoutState` type is received, the `Iwasaki` value should be all 0's. The `cliErrCode` should be set to a given value for the cause of the error/timeout.

The client has a special case/check for error `GP_GAME_ECODE_UNKNOWN`. If the `cliErrCode` is set to this value, the error the client will be prompted with is error code `4006`. This is seen commonly when the client is disconnected for unusable movement or speed hacking. Otherwise, the error code will be `4002`.

### Default Case: All Other Logout States

When any other logout state is seen, then values inside of the `Iwasaki` buffer are set. This buffer is used to hold the IP address and port of the next server the client should connect to in order to properly 'zone'. These values are stored inside of `Iwasaki` in the following format:

```cpp
class GP_SERV_LOGOUTSUB
{
    uint32_t ip;
    uint32_t port;
};
```
