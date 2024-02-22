<div align="center">
    <img width="200" src="https://github.com/atom0s/XiPackets/raw/main/repo/icon.png" alt="XiPackets">
    </br>
</div>

# Server To Client Packets

The following list is the various packets that are currently sent from the server to the client:

  - :x: - _Initial documentation incomplete._
  - :warning: - _Initial documentation partially finished._
  - :white_check_mark: = _Initial documentation done._

| Packet Id | Packet Command Name | Client Handler Name | First Pass Documented |
| --- | --- | --- | --- |
| [`0x0005`](/world/server/0x0005/README.md) | `GP_SERV_COMMAND_PACKETCONTROL`      | `RecvPacketControl`       | :white_check_mark: |
| [`0x0006`](/world/server/0x0006/README.md) | `GP_SERV_COMMAND_NARAKU`             | `RecvNaraku`              | :white_check_mark: |
| [`0x0008`](/world/server/0x0008/README.md) | `GP_SERV_COMMAND_ENTERZONE`          | `RecvEnterZone`           | :white_check_mark: |
| [`0x0009`](/world/server/0x0009/README.md) | `GP_SERV_COMMAND_MESSAGE`            | `RecvMessage`             | :white_check_mark: |
| [`0x000A`](/world/server/0x000A/README.md) | `GP_SERV_COMMAND_LOGIN`              | `RecvLogIn`               | :white_check_mark: |
| [`0x000B`](/world/server/0x000B/README.md) | `GP_SERV_COMMAND_LOGOUT`             | `RecvLogOut`              | :white_check_mark: |
| [`0x000D`](/world/server/0x000D/README.md) | `GP_SERV_COMMAND_CHAR_PC`            | `RecvCharPc`              | :x: |
| [`0x000E`](/world/server/0x000E/README.md) | `GP_SERV_COMMAND_CHAR_NPC`           | `RecvCharNpc`             | :x: |
| [`0x0012`](/world/server/0x0012/README.md) | `GP_SERV_COMMAND_GM`                 | `RecvGm`                  | :white_check_mark: |
| [`0x0013`](/world/server/0x0013/README.md) | `GP_SERV_COMMAND_GMCOMMAND`          | `RecvGmCommand`           | :white_check_mark: |
| [`0x0014`](/world/server/0x0014/README.md) | `GP_SERV_COMMAND_TELL`               | `RecvMessageTell`         | :white_check_mark: |
| [`0x0016`](/world/server/0x0016/README.md) | `GP_SERV_COMMAND_TALK`               | `RecvMessageTalk`         | :white_check_mark: |
| [`0x0017`](/world/server/0x0017/README.md) | `GP_SERV_COMMAND_CHAT_STD`           | `RecvStdChat`             | :white_check_mark: |
| [`0x001B`](/world/server/0x001B/README.md) | `(Unknown)`                          | `(Unknown)`               | :white_check_mark: |
| [`0x001C`](/world/server/0x001C/README.md) | `GP_SERV_COMMAND_ITEM_MAX`           | `RecvItemMax`             | :white_check_mark: |
| [`0x001D`](/world/server/0x001D/README.md) | `GP_SERV_COMMAND_ITEM_SAME`          | `RecvItemSame`            | :white_check_mark: |
| [`0x001E`](/world/server/0x001E/README.md) | `GP_SERV_COMMAND_ITEM_NUM`           | `RecvItemNum`             | :white_check_mark: |
| [`0x001F`](/world/server/0x001F/README.md) | `GP_SERV_COMMAND_ITEM_LIST`          | `RecvItemList`            | :white_check_mark: |
| [`0x0020`](/world/server/0x0020/README.md) | `GP_SERV_COMMAND_ITEM_ATTR`          | `RecvItemAttr`            | :white_check_mark: |
| [`0x0021`](/world/server/0x0021/README.md) | `GP_SERV_COMMAND_ITEM_TRADE_REQ`     | `RecvItemTradeReq`        | :white_check_mark: |
| [`0x0022`](/world/server/0x0022/README.md) | `GP_SERV_COMMAND_ITEM_TRADE_RES`     | `RecvItemTradeRes`        | :white_check_mark: |
| [`0x0023`](/world/server/0x0023/README.md) | `GP_SERV_COMMAND_ITEM_TRADE_LIST`    | `RecvItemTradeList`       | :white_check_mark: |
| [`0x0024`](/world/server/0x0024/README.md) | `GP_SERV_COMMAND_ITEM_PRESENT`       | `RecvItemPresent`         | :white_check_mark: |
| [`0x0025`](/world/server/0x0025/README.md) | `GP_SERV_COMMAND_ITEM_TRADE_MYLIST`  | `RecvItemTradeMyList`     | :white_check_mark: |
| [`0x0026`](/world/server/0x0026/README.md) | `(Unknown)`                          | `(Unknown)`               | :white_check_mark: |
| [`0x0027`](/world/server/0x0027/README.md) | `GP_SERV_COMMAND_TALKNUMWORK2`       | `RecvMessageTalkNumWork2` | :white_check_mark: |
| [`0x0028`](/world/server/0x0028/README.md) | `GP_SERV_COMMAND_BATTLE2`            | `RecvBattleCalc2`         | :white_check_mark: |
| [`0x0029`](/world/server/0x0029/README.md) | `GP_SERV_COMMAND_BATTLE_MESSAGE`     | `RecvBattleMessage`       | :white_check_mark: |
| [`0x002A`](/world/server/0x002A/README.md) | `GP_SERV_COMMAND_TALKNUMWORK`        | `RecvMessageTalkNumWork`  | :white_check_mark: |
| [`0x002B`](/world/server/0x002B/README.md) | `GP_SERV_COMMAND_CHANNEL_ITEM`       | `RecvChannelItem`         | :white_check_mark: |
| [`0x002C`](/world/server/0x002C/README.md) | `GP_SERV_COMMAND_CHANNEL_STATE`      | `RecvChannelState`        | :white_check_mark: |
| [`0x002D`](/world/server/0x002D/README.md) | `GP_SERV_COMMAND_BATTLE_MESSAGE2`    | `RecvBattleMessage2`      | :white_check_mark: |
| [`0x002E`](/world/server/0x002E/README.md) | `GP_SERV_COMMAND_OPENMOGMENU`        | `RecvOpenMogMenu`         | :white_check_mark: |
| [`0x002F`](/world/server/0x002F/README.md) | `GP_SERV_COMMAND_DIG`                | `RecvDig`                 | :white_check_mark: |
| [`0x0030`](/world/server/0x0030/README.md) | `GP_SERV_COMMAND_EFFECT`             | `RecvEffect`              | :white_check_mark: |
| [`0x0031`](/world/server/0x0031/README.md) | `GP_SERV_COMMAND_RECIPE`             | `RecvRecipe`              | :white_check_mark: |
| [`0x0032`](/world/server/0x0032/README.md) | `GP_SERV_COMMAND_EVENT`              | `RecvEventCalc`           | :white_check_mark: |
| [`0x0033`](/world/server/0x0033/README.md) | `GP_SERV_COMMAND_EVENTSTR`           | `RecvEventCalcStr`        | :white_check_mark: |
| [`0x0034`](/world/server/0x0034/README.md) | `GP_SERV_COMMAND_EVENTNUM`           | `RecvEventCalcNum`        | :white_check_mark: |
| [`0x0036`](/world/server/0x0036/README.md) | `GP_SERV_COMMAND_TALKNUM`            | `RecvMessageTalkNum`      | :white_check_mark: |
| [`0x0037`](/world/server/0x0037/README.md) | `GP_SERV_COMMAND_SERVERSTATUS`       | `RecvServerStatus`        | :white_check_mark: |
| [`0x0038`](/world/server/0x0038/README.md) | `GP_SERV_COMMAND_SCHEDULOR`          | `RecvSchedulor`           | :white_check_mark: |
| [`0x0039`](/world/server/0x0039/README.md) | `GP_SERV_COMMAND_MAPSCHEDULOR`       | `RecvMapSchedulor`        | :white_check_mark: |
| [`0x003A`](/world/server/0x003A/README.md) | `GP_SERV_COMMAND_MAGICSCHEDULOR`     | `RecvMagicSchedulor`      | :white_check_mark: |
| [`0x003B`](/world/server/0x003B/README.md) | `GP_SERV_COMMAND_EVENTMES`           | `RecvEventMes`            | :white_check_mark: |
| [`0x003C`](/world/server/0x003C/README.md) | `GP_SERV_COMMAND_SHOP_LIST`          | `RecvShopList`            | :white_check_mark: |
| [`0x003D`](/world/server/0x003D/README.md) | `GP_SERV_COMMAND_SHOP_SELL`          | `RecvShopSell`            | :white_check_mark: |
| [`0x003E`](/world/server/0x003E/README.md) | `GP_SERV_COMMAND_SHOP_OPEN`          | `RecvShopOpen`            | :white_check_mark: |
| [`0x003F`](/world/server/0x003F/README.md) | `GP_SERV_COMMAND_SHOP_BUY`           | `RecvShopBuy`             | :white_check_mark: |
| [`0x0040`](/world/server/0x0040/README.md) | `(Unknown)`                          | `(Unknown)`               | :white_check_mark: |
| [`0x0041`](/world/server/0x0041/README.md) | `GP_SERV_COMMAND_BLACK_LIST`         | `RecvBlacklist`           | :white_check_mark: |
| [`0x0042`](/world/server/0x0042/README.md) | `GP_SERV_COMMAND_BLACK_EDIT`         | `RecvBlackEdit`           | :white_check_mark: |
| [`0x0043`](/world/server/0x0043/README.md) | `GP_SERV_COMMAND_TALKNUMNAME`        | `RecvMessageTalkNumName`  | :white_check_mark: |
| [`0x0044`](/world/server/0x0044/README.md) | `(Unknown)`                          | `(Unknown)`               | :white_check_mark: |
| [`0x0047`](/world/server/0x0047/README.md) | `(Unknown)`                          | `(Unknown)`               | :white_check_mark: |
| [`0x0048`](/world/server/0x0048/README.md) | `(Unknown)`                          | `(Unknown)`               | :warning: |
| [`0x0049`](/world/server/0x0049/README.md) | `(Unknown)`                          | `(Unknown)`               | :white_check_mark: |
| [`0x004B`](/world/server/0x004B/README.md) | `GP_SERV_COMMAND_PBX_RESULT`         | `RecvReqPostReplyCommon`  | :warning: |
| [`0x004C`](/world/server/0x004C/README.md) | `GP_SERV_COMMAND_AUC`                | `RecvAucCommon`           | :warning: |
| [`0x004D`](/world/server/0x004D/README.md) | `GP_SERV_COMMAND_FRAGMENTS`          | `RecvFragments`           | :white_check_mark: |
| [`0x004F`](/world/server/0x004F/README.md) | `GP_SERV_COMMAND_EQUIP_CLEAR`        | `RecvEquipClear`          | :white_check_mark: |
| [`0x0050`](/world/server/0x0050/README.md) | `GP_SERV_COMMAND_EQUIP_LIST`         | `RecvEquipList`           | :white_check_mark: |
| [`0x0051`](/world/server/0x0051/README.md) | `GP_SERV_COMMAND_GRAP_LIST`          | `RecvGrapList`            | :white_check_mark: |
| [`0x0052`](/world/server/0x0052/README.md) | `GP_SERV_COMMAND_EVENTUCOFF`         | `RecvUcOff`               | :white_check_mark: |
| [`0x0053`](/world/server/0x0053/README.md) | `GP_SERV_COMMAND_SYSTEMMES`          | `RecvSystemMessage`       | :white_check_mark: |
| [`0x0054`](/world/server/0x0054/README.md) | `GP_SERV_COMMAND_DEBUGPRINT`         | `RecvDebufPrint`          | :white_check_mark: |
| [`0x0055`](/world/server/0x0055/README.md) | `GP_SERV_COMMAND_SCENARIOITEM`       | `RecvScenarioItem`        | :white_check_mark: |
| [`0x0056`](/world/server/0x0056/README.md) | `GP_SERV_COMMAND_MISSION`            | `RecvMissionItem`         | :white_check_mark: |
| [`0x0057`](/world/server/0x0057/README.md) | `GP_SERV_COMMAND_WEATHER`            | `RecvWeather`             | :white_check_mark: |
| [`0x0058`](/world/server/0x0058/README.md) | `GP_SERV_COMMAND_ASSIST`             | `RecvAssist`              | :white_check_mark: |
| [`0x0059`](/world/server/0x0059/README.md) | `GP_SERV_COMMAND_FRIENDPASS`         | `RecvFriendPass`          | :white_check_mark: |
| [`0x005A`](/world/server/0x005A/README.md) | `GP_SERV_COMMAND_MOTIONMES`          | `RecvEmotionMes`          | :white_check_mark: |
| [`0x005B`](/world/server/0x005B/README.md) | `GP_SERV_COMMAND_WPOS`               | `RecvWpos`                | :white_check_mark: |
| [`0x005C`](/world/server/0x005C/README.md) | `GP_SERV_COMMAND_PENDINGNUM`         | `RecvPendingNum`          | :white_check_mark: |
| [`0x005D`](/world/server/0x005D/README.md) | `(Unknown)`                          | `(Unknown)`               | :white_check_mark: |
| [`0x005E`](/world/server/0x005E/README.md) | `GP_SERV_COMMAND_CONQUEST`           | `RecvConquest`            | :x: |
| [`0x005F`](/world/server/0x005F/README.md) | `GP_SERV_COMMAND_MUSIC`              | `RecvMusic`               | :white_check_mark: |
| [`0x0060`](/world/server/0x0060/README.md) | `GP_SERV_COMMAND_MUSICVOLUME`        | `RecvMusicVolume`         | :white_check_mark: |
| [`0x0061`](/world/server/0x0061/README.md) | `GP_SERV_COMMAND_CLISTATUS`          | `RecvCliStatus`           | :white_check_mark: |
| [`0x0062`](/world/server/0x0062/README.md) | `GP_SERV_COMMAND_CLISTATUS2`         | `RecvCliStatus2`          | :white_check_mark: |
| [`0x0063`](/world/server/0x0063/README.md) | `(Unknown)`                          | `(Unknown)`               | :white_check_mark: |
| [`0x0064`](/world/server/0x0064/README.md) | `GP_SERV_COMMAND_PREFERENCE_DATA`    | `receivePreferenceData`   | :white_check_mark: |
| [`0x0065`](/world/server/0x0065/README.md) | `GP_SERV_COMMAND_WPOS2`              | `RecvWpos2`               | :white_check_mark: |
| [`0x0067`](/world/server/0x0067/README.md) | `(Unknown)`                          | `(Unknown)`               | :white_check_mark: |
| [`0x0068`](/world/server/0x0068/README.md) | `(Unknown)`                          | `(Unknown)`               | :white_check_mark: |
| [`0x0069`](/world/server/0x0069/README.md) | `(Unknown)`                          | `(Unknown)`               | :white_check_mark: |
| [`0x006F`](/world/server/0x006F/README.md) | `GP_SERV_COMMAND_COMBINE_ANS`        | `gcRecvCombine`           | :white_check_mark: |
| [`0x0070`](/world/server/0x0070/README.md) | `GP_SERV_COMMAND_COMBINE_INF`        | `gcRecvCombineInfo`       | :white_check_mark: |
| [`0x0071`](/world/server/0x0071/README.md) | `(Unknown)`                          | `(Unknown)`               | :white_check_mark: |
| [`0x0072`](/world/server/0x0072/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |
| [`0x0073`](/world/server/0x0073/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |
| [`0x0074`](/world/server/0x0074/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |
| [`0x0075`](/world/server/0x0075/README.md) | `(Unknown)`                          | `(Unknown)`               | :white_check_mark: |
| [`0x0076`](/world/server/0x0076/README.md) | `(Unknown)`                          | `(Unknown)`               | :white_check_mark: |
| [`0x0077`](/world/server/0x0077/README.md) | `(Unknown)`                          | `(Unknown)`               | :white_check_mark: |
| [`0x0078`](/world/server/0x0078/README.md) | `GP_SERV_COMMAND_SWITCH_START`       | `RecvStart`               | :white_check_mark: |
| [`0x0079`](/world/server/0x0079/README.md) | `GP_SERV_COMMAND_SWITCH_PROC`        | `RecvProc`                | :white_check_mark: |
| [`0x0081`](/world/server/0x0081/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |
| [`0x0082`](/world/server/0x0082/README.md) | `GP_SERV_COMMAND_GUILD_BUY`          | `gcRecvGuildBuy`          | :white_check_mark: |
| [`0x0083`](/world/server/0x0083/README.md) | `GP_SERV_COMMAND_GUILD_BUYLIST`      | `gcRecvGuildBuyList`      | :white_check_mark: |
| [`0x0084`](/world/server/0x0084/README.md) | `GP_SERV_COMMAND_GUILD_SELL`         | `gcRecvGuildSell`         | :white_check_mark: |
| [`0x0085`](/world/server/0x0085/README.md) | `GP_SERV_COMMAND_GUILD_SELLLIST`     | `gcRecvGuildSellList`     | :white_check_mark: |
| [`0x0086`](/world/server/0x0086/README.md) | `GP_SERV_COMMAND_GUILD_OPEN`         | `gcRecvGuildOpen`         | :white_check_mark: |
| [`0x008C`](/world/server/0x008C/README.md) | `(Unknown)`                          | `(Unknown)`               | :white_check_mark: |
| [`0x008D`](/world/server/0x008D/README.md) | `(Unknown)`                          | `(Unknown)`               | :white_check_mark: |
| [`0x0096`](/world/server/0x0096/README.md) | `GP_SERV_COMMAND_MYROOM_ENTER`       | `RecvMyroomEnter`         | :x: |
| [`0x0097`](/world/server/0x0097/README.md) | `GP_SERV_COMMAND_MYROOM_EXIT`        | `RecvMyroomExit`          | :x: |
| [`0x0098`](/world/server/0x0098/README.md) | `GP_SERV_COMMAND_MYROOM_IS`          | `RecvMyroomIs`            | :x: |
| [`0x0099`](/world/server/0x0099/README.md) | `GP_SERV_COMMAND_MYROOM_EXIST`       | `RecvMyroomExist`         | :x: |
| [`0x009A`](/world/server/0x009A/README.md) | `GP_SERV_COMMAND_MYROOM_PLANT`       | `RecvMyroomPlant`         | :x: |
| [`0x009B`](/world/server/0x009B/README.md) | `GP_SERV_COMMAND_MYROOM_RAISE`       | `RecvMyroomRaise`         | :x: |
| [`0x009C`](/world/server/0x009C/README.md) | `GP_SERV_COMMAND_MYROOM_HARVEST`     | `RecvMyroomHarvest`       | :x: |
| [`0x009D`](/world/server/0x009D/README.md) | `GP_SERV_COMMAND_MYROOM_DIARY`       | `RecvMyroomDiary`         | :x: |
| [`0x009E`](/world/server/0x009E/README.md) | `GP_SERV_COMMAND_MYROOM_PLACE`       | `RecvMyroomJob`           | :x: |
| [`0x00A0`](/world/server/0x00A0/README.md) | `GP_SERV_COMMAND_MAP_GROUP`          | `RecvMapGroup`            | :x: |
| [`0x00AA`](/world/server/0x00AA/README.md) | `GP_SERV_COMMAND_MAGIC_DATA`         | `RecvMagicData`           | :x: |
| [`0x00AB`](/world/server/0x00AB/README.md) | `GP_SERV_COMMAND_FEAT_DATA`          | `RecvFeatData`            | :x: |
| [`0x00AC`](/world/server/0x00AC/README.md) | `GP_SERV_COMMAND_COMMAND_DATA`       | `RecvCommandData`         | :x: |
| [`0x00AD`](/world/server/0x00AD/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |
| [`0x00AE`](/world/server/0x00AE/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |
| [`0x00B4`](/world/server/0x00B4/README.md) | `GP_SERV_COMMAND_CONFIG`             | `RecvConf`                | :x: |
| [`0x00B5`](/world/server/0x00B5/README.md) | `GP_SERV_COMMAND_FAQ_GMPARAM`        | `FDtRecvGmParam`          | :x: |
| [`0x00B6`](/world/server/0x00B6/README.md) | `GP_SERV_COMMAND_SET_GMMSG`          | `FDtRecvGmNotice`         | :x: |
| [`0x00B7`](/world/server/0x00B7/README.md) | `GP_SERV_COMMAND_GMSCITEM`           | `RecvGmScitem`            | :x: |
| [`0x00BF`](/world/server/0x00BF/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |
| [`0x00C8`](/world/server/0x00C8/README.md) | `GP_SERV_COMMAND_GROUP_TBL`          | `RecvGroupTbl`            | :x: |
| [`0x00C9`](/world/server/0x00C9/README.md) | `GP_SERV_COMMAND_EQUIP_INSPECT`      | `RecvEquipInspect`        | :x: |
| [`0x00CA`](/world/server/0x00CA/README.md) | `GP_SERV_COMMAND_INSPECT_MESSAGE`    | `RecvInspectMessage`      | :x: |
| [`0x00CC`](/world/server/0x00CC/README.md) | `GP_SERV_COMMAND_LINKSHELL_MESSAGE`  | `RecvComlinkMessage`      | :x: |
| [`0x00D2`](/world/server/0x00D2/README.md) | `GP_SERV_COMMAND_TROPHY_LIST`        | `RecvTrophyList`          | :x: |
| [`0x00D3`](/world/server/0x00D3/README.md) | `GP_SERV_COMMAND_TROPHY_SOLUTION`    | `RecvTrophySolution`      | :x: |
| [`0x00DC`](/world/server/0x00DC/README.md) | `GP_SERV_COMMAND_GROUP_SOLICIT_REQ`  | `RecvGroupSolicitreq`     | :x: |
| [`0x00DD`](/world/server/0x00DD/README.md) | `GP_SERV_COMMAND_GROUP_LIST`         | `RecvGroupList`           | :x: |
| [`0x00DE`](/world/server/0x00DE/README.md) | `GP_SERV_COMMAND_GROUP_SOLICIT_NO`   | `RecvGroupSolicitNo`      | :x: |
| [`0x00DF`](/world/server/0x00DF/README.md) | `GP_SERV_COMMAND_GROUP_ATTR`         | `RecvGroupAttr`           | :x: |
| [`0x00E0`](/world/server/0x00E0/README.md) | `GP_SERV_COMMAND_GROUP_COMLINK`      | `RecvComlink`             | :x: |
| [`0x00E1`](/world/server/0x00E1/README.md) | `GP_SERV_COMMAND_GROUP_CHECKID`      | `RecvCheckID`             | :x: |
| [`0x00E2`](/world/server/0x00E2/README.md) | `GP_SERV_COMMAND_GROUP_LIST2`        | `RecvGroupList2`          | :x: |
| [`0x00E6`](/world/server/0x00E6/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |
| [`0x00F4`](/world/server/0x00F4/README.md) | `GP_SERV_COMMAND_TRACKING_LIST`      | `RecvList`                | :x: |
| [`0x00F5`](/world/server/0x00F5/README.md) | `GP_SERV_COMMAND_TRACKING_POS`       | `RecvPos`                 | :x: |
| [`0x00F6`](/world/server/0x00F6/README.md) | `GP_SERV_COMMAND_TRACKING_STATE`     | `RecvState`               | :x: |
| [`0x00F9`](/world/server/0x00F9/README.md) | `GP_SERV_COMMAND_RES`                | `RecvServRes`             | :x: |
| [`0x00FA`](/world/server/0x00FA/README.md) | `GP_SERV_COMMAND_MYROOM_OPERATION`   | `RecvOperation`           | :x: |
| [`0x0105`](/world/server/0x0105/README.md) | `GP_SERV_COMMAND_BAZAAR_LIST`        | `RecvList`                | :x: |
| [`0x0106`](/world/server/0x0106/README.md) | `GP_SERV_COMMAND_BAZAAR_BUY`         | `RecvBuy`                 | :x: |
| [`0x0107`](/world/server/0x0107/README.md) | `GP_SERV_COMMAND_BAZAAR_CLOSE`       | `RecvClose`               | :x: |
| [`0x0108`](/world/server/0x0108/README.md) | `GP_SERV_COMMAND_BAZAAR_SHOPPING`    | `RecvShopping`            | :x: |
| [`0x0109`](/world/server/0x0109/README.md) | `GP_SERV_COMMAND_BAZAAR_SELL`        | `RecvSell`                | :x: |
| [`0x010A`](/world/server/0x010A/README.md) | `GP_SERV_COMMAND_BAZAAR_SALE`        | `RecvSale`                | :x: |
| [`0x010E`](/world/server/0x010E/README.md) | `GP_SERV_COMMAND_REQSUBMAPNUM`       | `RecvSubMapNum`           | :x: |
| [`0x010F`](/world/server/0x010F/README.md) | `GP_SERV_COMMAND_REQLOGOUTINFO`      | `RecvLogoutInfo`          | :x: |
| [`0x0110`](/world/server/0x0110/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |
| [`0x0111`](/world/server/0x0111/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |
| [`0x0112`](/world/server/0x0112/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |
| [`0x0113`](/world/server/0x0113/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |
| [`0x0115`](/world/server/0x0115/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |
| [`0x0116`](/world/server/0x0116/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |
| [`0x0117`](/world/server/0x0117/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |
| [`0x0118`](/world/server/0x0118/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |
| [`0x0119`](/world/server/0x0119/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |
| [`0x011A`](/world/server/0x011A/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |
| [`0x011B`](/world/server/0x011B/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |
| [`0x011C`](/world/server/0x011C/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |
| [`0x011D`](/world/server/0x011D/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |
| [`0x011E`](/world/server/0x011E/README.md) | `(Unknown)`                          | `(Unknown)`               | :x: |

_**Note:** The names for the above packet commands and client handler functions are real names taken from the PS2 Beta debug information/symbols. Packets that are marked as `(Unknown)` either did not have a named command or handler in the PS2 beta, or they were added after the PS2 beta. Only real valid names are listed regardless if the packet purpose is understood._

_**Note:** The client allows every packet id to have two handlers. The above list is limited to only showing the main handler that is used to save space. Any packet that has a secondary handler will be listed inside of the individual packets page instead._
