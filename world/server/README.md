<div align="center">
    <img width="200" src="https://github.com/atom0s/XiPackets/raw/main/repo/icon.png" alt="XiPackets">
    </br>
</div>

# Server To Client Packets

The following list is the various packets that are currently sent from the server to the client:

| Packet Id | Packet Command Name | Client Handler Name |
|---|---|---|
| [`0x0005`](/world/server/0x0005/README.md) | `GP_SERV_COMMAND_PACKETCONTROL`    | `RecvPacketControl`       |
| [`0x0006`](/world/server/0x0006/README.md) | `GP_SERV_COMMAND_NARAKU`           | `RecvNaraku`              |
| [`0x0008`](/world/server/0x0008/README.md) | `GP_SERV_COMMAND_ENTERZONE`        | `RecvEnterZone`           |
| [`0x0009`](/world/server/0x0009/README.md) | `GP_SERV_COMMAND_MESSAGE`          | `RecvMessage`             |
| [`0x000A`](/world/server/0x000A/README.md) | `GP_SERV_COMMAND_LOGIN`            | `RecvLogIn`               |
| [`0x000B`](/world/server/0x000B/README.md) | `GP_SERV_COMMAND_LOGOUT`           | `RecvLogOut`              |
| [`0x000D`](/world/server/0x000D/README.md) | `GP_SERV_COMMAND_CHAR_PC`          | `RecvCharPc`              |
| [`0x000E`](/world/server/0x000E/README.md) | `GP_SERV_COMMAND_CHAR_NPC`         | `RecvCharNpc`             |
| [`0x0012`](/world/server/0x0012/README.md) | `GP_SERV_COMMAND_GM`               | `RecvGm`                  |
| [`0x0013`](/world/server/0x0013/README.md) | `GP_SERV_COMMAND_GMCOMMAND`        | `RecvGmCommand`           |
| [`0x0014`](/world/server/0x0014/README.md) | `GP_SERV_COMMAND_TELL`             | `RecvMessageTell`         |
| [`0x0016`](/world/server/0x0016/README.md) | `GP_SERV_COMMAND_TALK`             | `RecvMessageTalk`         |
| [`0x0017`](/world/server/0x0017/README.md) | `GP_SERV_COMMAND_CHAT_STD`         | `RecvStdChat`             |
| [`0x001B`](/world/server/0x001B/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x001C`](/world/server/0x001C/README.md) | `GP_SERV_COMMAND_ITEM_MAX`         | `RecvItemMax`             |
| [`0x001D`](/world/server/0x001D/README.md) | `GP_SERV_COMMAND_ITEM_SAME`        | `RecvItemSame`            |
| [`0x001E`](/world/server/0x001E/README.md) | `GP_SERV_COMMAND_ITEM_NUM`         | `RecvItemNum`             |
| [`0x001F`](/world/server/0x001F/README.md) | `GP_SERV_COMMAND_ITEM_LIST`        | `RecvItemList`            |
| [`0x0020`](/world/server/0x0020/README.md) | `GP_SERV_COMMAND_ITEM_ATTR`        | `RecvItemAttr`            |
| [`0x0021`](/world/server/0x0021/README.md) | `GP_SERV_COMMAND_ITEM_TRADE_REQ`   | `RecvItemTradeReq`        |
| [`0x0022`](/world/server/0x0022/README.md) | `GP_SERV_COMMAND_ITEM_TRADE_RES`   | `RecvItemTradeRes`        |
| [`0x0023`](/world/server/0x0023/README.md) | `GP_SERV_COMMAND_ITEM_TRADE_LIST`  | `RecvItemTradeList`       |
| [`0x0024`](/world/server/0x0024/README.md) | `GP_SERV_COMMAND_ITEM_PRESENT`     | `RecvItemPresent`         |
| [`0x0025`](/world/server/0x0025/README.md) | `GP_SERV_COMMAND_ITEM_TRADE_MYLIST`| `RecvItemTradeMyList`     |
| [`0x0026`](/world/server/0x0026/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0027`](/world/server/0x0027/README.md) | `GP_SERV_COMMAND_TALKNUMWORK2`     | `RecvMessageTalkNumWork2` |
| [`0x0028`](/world/server/0x0028/README.md) | `GP_SERV_COMMAND_BATTLE2`          | `RecvBattleCalc2`         |
| [`0x0029`](/world/server/0x0029/README.md) | `GP_SERV_COMMAND_BATTLE_MESSAGE`   | `RecvBattleMessage`       |
| [`0x002A`](/world/server/0x002A/README.md) | `GP_SERV_COMMAND_TALKNUMWORK`      | `RecvMessageTalkNumWork`  |
| [`0x002B`](/world/server/0x002B/README.md) | `GP_SERV_COMMAND_CHANNEL_ITEM`     | `RecvChannelItem`         |
| [`0x002C`](/world/server/0x002C/README.md) | `GP_SERV_COMMAND_CHANNEL_STATE`    | `RecvChannelState`        |
| [`0x002D`](/world/server/0x002D/README.md) | `GP_SERV_COMMAND_BATTLE_MESSAGE2`  | `RecvBattleMessage2`      |
| [`0x002E`](/world/server/0x002E/README.md) | `GP_SERV_COMMAND_OPENMOGMENU`      | `RecvOpenMogMenu`         |
| [`0x002F`](/world/server/0x002F/README.md) | `GP_SERV_COMMAND_DIG`              | `RecvDig`                 |
| [`0x0030`](/world/server/0x0030/README.md) | `GP_SERV_COMMAND_EFFECT`           | `RecvEffect`              |
| [`0x0031`](/world/server/0x0031/README.md) | `GP_SERV_COMMAND_RECIPE`           | `RecvRecipe`              |
| [`0x0032`](/world/server/0x0032/README.md) | `GP_SERV_COMMAND_EVENT`            | `RecvEventCalc`           |
| [`0x0033`](/world/server/0x0033/README.md) | `GP_SERV_COMMAND_EVENTSTR`         | `RecvEventCalcStr`        |
| [`0x0034`](/world/server/0x0034/README.md) | `GP_SERV_COMMAND_EVENTNUM`         | `RecvEventCalcNum`        |
| [`0x0036`](/world/server/0x0036/README.md) | `GP_SERV_COMMAND_TALKNUM`          | `RecvMessageTalkNum`      |
| [`0x0037`](/world/server/0x0037/README.md) | `GP_SERV_COMMAND_SERVERSTATUS`     | `RecvServerStatus`        |
| [`0x0038`](/world/server/0x0038/README.md) | `GP_SERV_COMMAND_SCHEDULOR`        | `RecvSchedulor`           |
| [`0x0039`](/world/server/0x0039/README.md) | `GP_SERV_COMMAND_MAPSCHEDULOR`     | `RecvMapSchedulor`        |
| [`0x003A`](/world/server/0x003A/README.md) | `GP_SERV_COMMAND_MAGICSCHEDULOR`   | `RecvMagicSchedulor`      |
| [`0x003B`](/world/server/0x003B/README.md) | `GP_SERV_COMMAND_EVENTMES`         | `RecvEventMes`            |
| [`0x003C`](/world/server/0x003C/README.md) | `GP_SERV_COMMAND_SHOP_LIST`        | `RecvShopList`            |
| [`0x003D`](/world/server/0x003D/README.md) | `GP_SERV_COMMAND_SHOP_SELL`        | `RecvShopSell`            |
| [`0x003E`](/world/server/0x003E/README.md) | `GP_SERV_COMMAND_SHOP_OPEN`        | `RecvShopOpen`            |
| [`0x003F`](/world/server/0x003F/README.md) | `GP_SERV_COMMAND_SHOP_BUY`         | `RecvShopBuy`             |
| [`0x0040`](/world/server/0x0040/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0041`](/world/server/0x0041/README.md) | `GP_SERV_COMMAND_BLACK_LIST`       | `RecvBlacklist`           |
| [`0x0042`](/world/server/0x0042/README.md) | `GP_SERV_COMMAND_BLACK_EDIT`       | `RecvBlackEdit`           |
| [`0x0043`](/world/server/0x0043/README.md) | `GP_SERV_COMMAND_TALKNUMNAME`      | `RecvMessageTalkNumName`  |
| [`0x0044`](/world/server/0x0044/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0047`](/world/server/0x0047/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0048`](/world/server/0x0048/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0049`](/world/server/0x0049/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x004B`](/world/server/0x004B/README.md) | `GP_SERV_COMMAND_PBX_RESULT`       | `RecvReqPostReplyCommon`  |
| [`0x004C`](/world/server/0x004C/README.md) | `GP_SERV_COMMAND_AUC`              | `RecvAucCommon`           |
| [`0x004D`](/world/server/0x004D/README.md) | `GP_SERV_COMMAND_FRAGMENTS`        | `RecvFragments`           |
| [`0x004F`](/world/server/0x004F/README.md) | `GP_SERV_COMMAND_EQUIP_CLEAR`      | `RecvEquipClear`          |
| [`0x0050`](/world/server/0x0050/README.md) | `GP_SERV_COMMAND_EQUIP_LIST`       | `RecvEquipList`           |
| [`0x0051`](/world/server/0x0051/README.md) | `GP_SERV_COMMAND_GRAP_LIST`        | `RecvGrapList`            |
| [`0x0052`](/world/server/0x0052/README.md) | `GP_SERV_COMMAND_EVENTUCOFF`       | `RecvUcOff`               |
| [`0x0053`](/world/server/0x0053/README.md) | `GP_SERV_COMMAND_SYSTEMMES`        | `RecvSystemMessage`       |
| [`0x0054`](/world/server/0x0054/README.md) | `GP_SERV_COMMAND_DEBUGPRINT`       | `RecvDebufPrint`          |
| [`0x0055`](/world/server/0x0055/README.md) | `GP_SERV_COMMAND_SCENARIOITEM`     | `RecvScenarioItem`        |
| [`0x0056`](/world/server/0x0056/README.md) | `GP_SERV_COMMAND_MISSION`          | `RecvMissionItem`         |
| [`0x0057`](/world/server/0x0057/README.md) | `GP_SERV_COMMAND_WEATHER`          | `RecvWeather`             |
| [`0x0058`](/world/server/0x0058/README.md) | `GP_SERV_COMMAND_ASSIST`           | `RecvAssist`              |
| [`0x0059`](/world/server/0x0059/README.md) | `GP_SERV_COMMAND_FRIENDPASS`       | `RecvFriendPass`          |
| [`0x005A`](/world/server/0x005A/README.md) | `GP_SERV_COMMAND_MOTIONMES`        | `RecvEmotionMes`          |
| [`0x005B`](/world/server/0x005B/README.md) | `GP_SERV_COMMAND_WPOS`             | `RecvWpos`                |
| [`0x005C`](/world/server/0x005C/README.md) | `GP_SERV_COMMAND_PENDINGNUM`       | `RecvPendingNum`          |
| [`0x005D`](/world/server/0x005D/README.md) | `GP_SERV_COMMAND_AUCTIONHOUSE`     | `RecvAuctionHouse`        |
| [`0x005E`](/world/server/0x005E/README.md) | `GP_SERV_COMMAND_CONQUEST`         | `RecvConquest`            |
| [`0x005F`](/world/server/0x005F/README.md) | `GP_SERV_COMMAND_MUSIC`            | `RecvMusic`               |
| [`0x0060`](/world/server/0x0060/README.md) | `GP_SERV_COMMAND_MUSICVOLUME`      | `RecvMusicVolume`         |
| [`0x0061`](/world/server/0x0061/README.md) | `GP_SERV_COMMAND_CLISTATUS`        | `RecvCliStatus`           |
| [`0x0062`](/world/server/0x0062/README.md) | `GP_SERV_COMMAND_CLISTATUS2`       | `RecvCliStatus2`          |
| [`0x0063`](/world/server/0x0063/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0064`](/world/server/0x0064/README.md) | `GP_SERV_COMMAND_PREFERENCE_DATA`  | `receivePreferenceData`   |
| [`0x0065`](/world/server/0x0065/README.md) | `GP_SERV_COMMAND_WPOS2`            | `RecvWpos2`               |
| [`0x0067`](/world/server/0x0067/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0068`](/world/server/0x0068/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0069`](/world/server/0x0069/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x006F`](/world/server/0x006F/README.md) | `GP_SERV_COMMAND_COMBINE_ANS`      | `gcRecvCombine`           |
| [`0x0070`](/world/server/0x0070/README.md) | `GP_SERV_COMMAND_COMBINE_INF`      | `gcRecvCombineInfo`       |
| [`0x0071`](/world/server/0x0071/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0072`](/world/server/0x0072/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0073`](/world/server/0x0073/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0074`](/world/server/0x0074/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0075`](/world/server/0x0075/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0076`](/world/server/0x0076/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0077`](/world/server/0x0077/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0078`](/world/server/0x0078/README.md) | `GP_SERV_COMMAND_SWITCH_START`     | `RecvStart`               |
| [`0x0079`](/world/server/0x0079/README.md) | `GP_SERV_COMMAND_SWITCH_PROC`      | `RecvProc`                |
| [`0x0081`](/world/server/0x0081/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0082`](/world/server/0x0082/README.md) | `GP_SERV_COMMAND_GUILD_BUY`        | `gcRecvGuildBuy`          |
| [`0x0083`](/world/server/0x0083/README.md) | `GP_SERV_COMMAND_GUILD_BUYLIST`    | `gcRecvGuildBuyList`      |
| [`0x0084`](/world/server/0x0084/README.md) | `GP_SERV_COMMAND_GUILD_SELL`       | `gcRecvGuildSell`         |
| [`0x0085`](/world/server/0x0085/README.md) | `GP_SERV_COMMAND_GUILD_SELLLIST`   | `gcRecvGuildSellList`     |
| [`0x0086`](/world/server/0x0086/README.md) | `GP_SERV_COMMAND_GUILD_OPEN`       | `gcRecvGuildOpen`         |
| [`0x008C`](/world/server/0x008C/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x008D`](/world/server/0x008D/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0096`](/world/server/0x0096/README.md) | `GP_SERV_COMMAND_MYROOM_ENTER`     | `RecvMyroomEnter`         |
| [`0x0097`](/world/server/0x0097/README.md) | `GP_SERV_COMMAND_MYROOM_EXIT`      | `RecvMyroomExit`          |
| [`0x0098`](/world/server/0x0098/README.md) | `GP_SERV_COMMAND_MYROOM_IS`        | `RecvMyroomIs`            |
| [`0x0099`](/world/server/0x0099/README.md) | `GP_SERV_COMMAND_MYROOM_EXIST`     | `RecvMyroomExist`         |
| [`0x009A`](/world/server/0x009A/README.md) | `GP_SERV_COMMAND_MYROOM_PLANT`     | `RecvMyroomPlant`         |
| [`0x009B`](/world/server/0x009B/README.md) | `GP_SERV_COMMAND_MYROOM_RAISE`     | `RecvMyroomRaise`         |
| [`0x009C`](/world/server/0x009C/README.md) | `GP_SERV_COMMAND_MYROOM_HARVEST`   | `RecvMyroomHarvest`       |
| [`0x009D`](/world/server/0x009D/README.md) | `GP_SERV_COMMAND_MYROOM_DIARY`     | `RecvMyroomDiary`         |
| [`0x009E`](/world/server/0x009E/README.md) | `GP_SERV_COMMAND_MYROOM_PLACE`     | `RecvMyroomJob`           |
| [`0x00A0`](/world/server/0x00A0/README.md) | `GP_SERV_COMMAND_MAP_GROUP`        | `RecvMapGroup`            |
| [`0x00AA`](/world/server/0x00AA/README.md) | `GP_SERV_COMMAND_MAGIC_DATA`       | `RecvMagicData`           |
| [`0x00AB`](/world/server/0x00AB/README.md) | `GP_SERV_COMMAND_FEAT_DATA`        | `RecvFeatData`            |
| [`0x00AC`](/world/server/0x00AC/README.md) | `GP_SERV_COMMAND_COMMAND_DATA`     | `RecvCommandData`         |
| [`0x00AD`](/world/server/0x00AD/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x00AE`](/world/server/0x00AE/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x00B4`](/world/server/0x00B4/README.md) | `GP_SERV_COMMAND_CONFIG`           | `RecvConf`                |
| [`0x00B5`](/world/server/0x00B5/README.md) | `GP_SERV_COMMAND_FAQ_GMPARAM`      | `FDtRecvGmParam`          |
| [`0x00B6`](/world/server/0x00B6/README.md) | `GP_SERV_COMMAND_SET_GMMSG`        | `FDtRecvGmNotice`         |
| [`0x00B7`](/world/server/0x00B7/README.md) | `GP_SERV_COMMAND_GMSCITEM`         | `RecvGmScitem`            |
| [`0x00BF`](/world/server/0x00BF/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x00C8`](/world/server/0x00C8/README.md) | `GP_SERV_COMMAND_GROUP_TBL`        | `RecvGroupTbl`            |
| [`0x00C9`](/world/server/0x00C9/README.md) | `GP_SERV_COMMAND_EQUIP_INSPECT`    | `RecvEquipInspect`        |
| [`0x00CA`](/world/server/0x00CA/README.md) | `GP_SERV_COMMAND_INSPECT_MESSAGE`  | `RecvInspectMessage`      |
| [`0x00CC`](/world/server/0x00CC/README.md) | `GP_SERV_COMMAND_LINKSHELL_MESSAGE`| `RecvComlinkMessage`      |
| [`0x00D2`](/world/server/0x00D2/README.md) | `GP_SERV_COMMAND_TROPHY_LIST`      | `RecvTrophyList`          |
| [`0x00D3`](/world/server/0x00D3/README.md) | `GP_SERV_COMMAND_TROPHY_SOLUTION`  | `RecvTrophySolution`      |
| [`0x00DC`](/world/server/0x00DC/README.md) | `GP_SERV_COMMAND_GROUP_SOLICIT_REQ`| `RecvGroupSolicitreq`     |
| [`0x00DD`](/world/server/0x00DD/README.md) | `GP_SERV_COMMAND_GROUP_LIST`       | `RecvGroupList`           |
| [`0x00DE`](/world/server/0x00DE/README.md) | `GP_SERV_COMMAND_GROUP_SOLICIT_NO` | `RecvGroupSolicitNo`      |
| [`0x00DF`](/world/server/0x00DF/README.md) | `GP_SERV_COMMAND_GROUP_ATTR`       | `RecvGroupAttr`           |
| [`0x00E0`](/world/server/0x00E0/README.md) | `GP_SERV_COMMAND_GROUP_COMLINK`    | `RecvComlink`             |
| [`0x00E1`](/world/server/0x00E1/README.md) | `GP_SERV_COMMAND_GROUP_CHECKID`    | `RecvCheckID`             |
| [`0x00E2`](/world/server/0x00E2/README.md) | `GP_SERV_COMMAND_GROUP_LIST2`      | `RecvGroupList2`          |
| [`0x00E6`](/world/server/0x00E6/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x00F4`](/world/server/0x00F4/README.md) | `GP_SERV_COMMAND_TRACKING_LIST`    | `RecvList`                |
| [`0x00F5`](/world/server/0x00F5/README.md) | `GP_SERV_COMMAND_TRACKING_POS`     | `RecvPos`                 |
| [`0x00F6`](/world/server/0x00F6/README.md) | `GP_SERV_COMMAND_TRACKING_STATE`   | `RecvState`               |
| [`0x00F9`](/world/server/0x00F9/README.md) | `GP_SERV_COMMAND_RES`              | `RecvServRes`             |
| [`0x00FA`](/world/server/0x00FA/README.md) | `GP_SERV_COMMAND_MYROOM_OPERATION` | `RecvOperation`           |
| [`0x0105`](/world/server/0x0105/README.md) | `GP_SERV_COMMAND_BAZAAR_LIST`      | `RecvList`                |
| [`0x0106`](/world/server/0x0106/README.md) | `GP_SERV_COMMAND_BAZAAR_BUY`       | `RecvBuy`                 |
| [`0x0107`](/world/server/0x0107/README.md) | `GP_SERV_COMMAND_BAZAAR_CLOSE`     | `RecvClose`               |
| [`0x0108`](/world/server/0x0108/README.md) | `GP_SERV_COMMAND_BAZAAR_SHOPPING`  | `RecvShopping`            |
| [`0x0109`](/world/server/0x0109/README.md) | `GP_SERV_COMMAND_BAZAAR_SELL`      | `RecvSell`                |
| [`0x010A`](/world/server/0x010A/README.md) | `GP_SERV_COMMAND_BAZAAR_SALE`      | `RecvSale`                |
| [`0x010E`](/world/server/0x010E/README.md) | `GP_SERV_COMMAND_REQSUBMAPNUM`     | `RecvSubMapNum`           |
| [`0x010F`](/world/server/0x010F/README.md) | `GP_SERV_COMMAND_REQLOGOUTINFO`    | `RecvLogoutInfo`          |
| [`0x0110`](/world/server/0x0110/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0111`](/world/server/0x0111/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0112`](/world/server/0x0112/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0113`](/world/server/0x0113/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0115`](/world/server/0x0115/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0116`](/world/server/0x0116/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0117`](/world/server/0x0117/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0118`](/world/server/0x0118/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x0119`](/world/server/0x0119/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x011A`](/world/server/0x011A/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x011B`](/world/server/0x011B/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x011C`](/world/server/0x011C/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x011D`](/world/server/0x011D/README.md) | `(Unknown)`                        | `(Unknown)`               |
| [`0x011E`](/world/server/0x011E/README.md) | `(Unknown)`                        | `(Unknown)`               |

_**Note:** The names for the above packet commands and client handler functions are real names taken from the PS2 Beta debug information/symbols. Packets that are marked as `(Unknown)` either did not have a named command or handler in the PS2 beta, or they were added after the PS2 beta. Only real valid names are listed regardless if the packet purpose is understood._

_**Note:** The client allows every packet id to have two handlers. The above list is limited to only showing the main handler that is used to save space. Any packet that has a secondary handler will be listed inside of the individual packets page instead._
