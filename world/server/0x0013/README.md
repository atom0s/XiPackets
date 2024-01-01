# `GP_SERV_COMMAND_GMCOMMAND`

| Information               | Notes |
|---                        |---    |
| **Command Name**          | `GP_SERV_COMMAND_GMCOMMAND` |
| **Client Handler**        | `RecvGmCommand` |
| **Direction**             | `S -> C` |
| **OpCode**                | `0x0013` |
| **Size**                  | `(varies)` |

## Description

> [!NOTE]
> This is a special GM-related packet!

This packet is sent by the server when a GM command is being executed on a client.

## Packet Layout

The layout of this packet is the following:

```cpp
// PS2: GP_SERV_GMCOMMAND
struct GP_SERV_GMCOMMAND
{
    uint16_t    id: 9;
    uint16_t    size: 7;
    uint16_t    sync;
    uint32_t    GMUniqueNo; // PS2: GMUniqueNo
    uint8_t     Mes[];      // PS2: Mes
};
```

## Packet Fields

The following information describes the structures outlined above.

## Structure Fields (`GP_SERV_GMCOMMAND`)

### `id`, `size`, `sync`

_These fields are part of the packet header._

_You can find more information about the header fields here: [**Header**](/world/server/Header.md)_

### `GMUniqueNo`

_The server id of the GM that executed the command._

### `Mes`

_The buffer containing the command to be executed._

> [!WARNING]
> This string value is not guaranteed to be null-terminated!

## Additional Information

This packet is used to allow GMs to execute commands on a players client. The game client only has several built-in functions that this is designed for, while other GM commands are executed on the server.

The following commands are built into the client:

| Command | Description |
| ---: | :--- |
| `reqchar`     | _`It is an inquiry to Searver about the UniqueNo.`_ |
| `poslist`     | _`Pos list get.`_ |
| `eventinfo`   | _`Event info get.`_ |

_Find more detailed information about each command below!_

If the client receives a command that is not available locally, it will instead echo back to the GM which commands are available on the client. This is used as a way for GMs to query a client for available commands if they ever supported multiple different clients / versions at the same time.

When the client receives this packet, it 'attaches' itself to the GM that sent the packet. It does this by setting its `zone->GMUniqueNo` value which is then used in future responses to be sent back to that specific GM.

When a matching valid command is found, the client will execute the command handler attached to it. _(See below.)_ When no valid matching command is found, the client echos back to the GM a list of available commands that they can use. This is done by the client sending `0x1F` packets back to the server via `gcGmPrintSend` which are then forwarded to the GM whos `GMUniqueNo` was given.

The client sends back this data in the following format:

```cpp
// First packet: (Sends a command header.)
gcGmPrintSend("-- GM Client Command Help -----");

// Additional packets: (One for each command available.)
gcGmPrintSend("%s---\n%s", cmd->sCommand, cmd->sHelp);
```

Below are the current available GM commands on the latest retail client:

### `reqchar`

_Requests the client to echo back general information to the requesting GM._

Based on what `FUNC_DebugReqCharSend` appears to be coded for, it seems like this command is incomplete and not properly coded. Instead of echoing back actual information inside of the client, it just echos back hardcoded values and a parameter passed by the GM when sending the `reqchar` command.

```cpp
bool __cdecl FUNC_DebugReqCharSend(uint16_t ActIndex, uint32_t UniqueNo2, uint32_t UniqueNo3, uint16_t Flg, uint16_t Flg2)
{
    const auto pkt = FUNC_gcZoneSendQueSearch(0x17, 0, 0);
    if ( !pkt )
        return 0;

    *(uint16_t*)&pkt->Data[0x00] = ActIndex;
    *(uint32_t*)&pkt->Data[0x04] = UniqueNo2;
    *(uint32_t*)&pkt->Data[0x08] = UniqueNo3;
    *(uint16_t*)&pkt->Data[0x0C] = Flg;
    *(uint16_t*)&pkt->Data[0x0E] = Flg2;
    FUNC_enQueSet(pkt, 20, 0);

    return 1;
}
void __cdecl FUNC_ReqChar(const char *pCommand)
{
    FUNC_gcGmPrintSend("serv input[%s]", pCommand);

    const uint16_t uid = ::atoi(pCommand);
    FUNC_DebugReqCharSend(512u, uid, 1000u, 0, 0);
}
```

### `poslist`

_Requests the client to echo back its movement collision data to the requesting GM._

This is used by GMs to monitor a players movement throughout the world. It can be used to determine if a player is cheating/hacking such as:

  - Clipping through walls / terrain.
  - Position warping / hacking.
  - Speed hacking.

As the client moves around in the world, it is constantly doing collision checks with the environment. This mainly involves checking the client characters collision with the floor to make sure they are on 'stable ground'. FFXI is coded to not allow vertical movement _(ie. jumping, falling, etc.)_ and will constantly try and keep/put the player back onto the floor if they are ever caught off it. _(For example, when you walk off a bridge or cliff edge you will immediately get pushed to the floor below due to this.)_ This collision data is constantly stored in a rotating buffer of 4096 entries.

When a GM sends a client the `poslist` command, it will put the client into a state that causes it to echo back parts of this collision cache buffer to the GM. This allows the GM to monitor the players movement for the above reasons. When first received, the client will set itself to be in a `poslist` echo state, store the current collision cache index and adjust for which part of the cache to send to the GM. IT will then set a current timestamp of when the request was made and prepare the client to begin echoing the collision data back to the GM.

Once this mode is enabled, the client will then begin echoing back its collision cache to the GM, sending multiple chunks of entries per echo. This data is echoed in the following format:

```cpp
const auto cache_entry = PTR_PlayerCollisionCache[idx];
::sprintf(buffer, "@%d,%d,%d,%d", cache_entry[0] * 1000.0f, cache_entry[1] * 1000.0f, cache_entry[2] * 1000.0f, cache_entry[3] * 1000.0f);
```

The client will append entries together until it reaches 150 characters then echos the message back to the GM. It will do this until the stored index is reached or the message no longer has data to append.

### `eventinfo`

_Requests the client to echo back its current event status information to the requesting GM._

This command is used to allow GMs to check on a client during an event, which cna be used to debug the clients current event state. When this command is received, the client will check to see if the local player is valid and echo back its event information if true. The format of this message that is echoed back is as follows:

```cpp
const auto status_srv   = entity->StatusServer;                     // The client entity server status.
const auto status_cli   = entity->Status;                           // The client entity local status.
const auto task_name    = &PTR_EventTaskName[PTR_EventTaskCounter]; // The current event task name.
const auto status_lck   = FUNC_XiActor_IsLockedStatus(actor);       // The client entity lock status.

FUNC_gcGmPrintSend("EventInfo:CSTAT=%s,SSTAT=%s,UC=%d,EF=%d,Slock=%d,ETask=%s\n", PTR_TestStatsuName[status_cli].Name, PTR_TestStatsuName[status_srv].Name, PTR_CliEventUcFlag, PTR_EventExecFlag, status_lck, task_name->Name);
```

The `task_name` will return one of the following values based on the current event task counter:

  - `NONE`
  - `EVEFILE1READ`
  - `EVEFILE2READ`
  - `SETUP1`
  - `SETUP2`
  - `SETUPEND`
  - `WAITRECVCHAR`
  - `WAITCASTMOTION`
  - `WAITLOCKSTATUS`
  - `NONE`
  - `NONE`
  - `NONE`

The `status_srv` and `status_cli` values will return one of the following strings based on their values:

  - `(N_IDLE)`
  - `(B_IDLE)`
  - `(N_DEAD)`
  - `(B_DEAD)`
  - `(EVT)`
  - `(CHOCOBO)`
  - `(FISHING)`
  - `(POL)`
  - `(D_OPEN)`
  - `(D_CLOSE)`
  - `(L1)`
  - `(L2)`
  - `(L3)`
  - `(L4)`
  - `(L5)`
  - `(L6)`
  - `(L7)`
  - `(L8)`
  - `(M1)`
  - `(M2)`
  - `(M3)`
  - `(M4)`
  - `(M5)`
  - `(M6)`
  - `(M7)`
  - `(M8)`
  - `(ES0)`
  - `(ES1)`
  - `(ES2)`
  - `(ES3)`
  - `(ES4)`
  - `(ES5)`
  - `(RLOG)`
  - `(CAMP)`
  - `(EFF0)`
  - `(EFF1)`
  - `(EFF2)`
  - `(EFF3)`
  - `(FISHING1)`
  - `(FISHING2)`
  - `(FISHING3)`
  - `(FISHING4)`
  - `(FISHING5)`
  - `(FISHING6)`
  - `(ITEMMAKE)`
  - `(D_OPEN2)`
  - `(D_CLOSE2)`
  - `(SIT)`
  - `(ICRYSTALL)`
  - `(MANNEQUIN)`
  - `(FISH_2)`
  - `(FISHF)`
  - `(FISHR)`
  - `(FISHL)`
  - `(GEOM0)`
  - `(FURNITURE0)`
  - `(FISH_3)`
  - `(FISH_31)`
  - `(FISH_32)`
  - `(FISH_33)`
  - `(FISH_34)`
  - `(FISH_35)`
  - `(FISH_36)`
  - `(CHAIR00)`
  - `(CHAIR01)`
  - `(CHAIR02)`
  - `(CHAIR03)`
  - `(CHAIR04)`
  - `(CHAIR05)`
  - `(CHAIR06)`
  - `(CHAIR07)`
  - `(CHAIR08)`
  - `(CHAIR09)`
  - `(CHAIR10)`
  - `(CHAIR11)`
  - `(CHAIR12)`
  - `(CHAIR13)`
  - `(CHAIR14)`
  - `(CHAIR15)`
  - `(CHAIR16)`
  - `(CHAIR17)`
  - `(CHAIR18)`
  - `(CHAIR19)`
  - `(CHAIR20)`
  - `(RANGE)`
  - `(MOUNT)`
