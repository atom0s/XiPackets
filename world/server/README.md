<div align="center">
    <img width="200" src="https://github.com/atom0s/XiPackets/raw/main/repo/icon.png" alt="XiPackets">
    </br>
</div>

# Server To Client Packets

The following list is the various packets that are currently sent from the server to the client:

  - :x: - _Initial documentation incomplete._
  - :warning: - _Initial documentation partially finished._
  - :white_check_mark: = _Initial documentation done._

| Id | Packet Command Name | Handler Name (1) | Handler Name (2) | First Pass Documented |
| --- | --- | --- | --- | --- |
| [`0x0005`](/world/server/0x0005/README.md) | `GP_SERV_COMMAND_PACKETCONTROL`      | `RecvPacketControl`           | _None._                               | :white_check_mark: |
| [`0x0006`](/world/server/0x0006/README.md) | `GP_SERV_COMMAND_NARAKU`             | `RecvNaraku`                  | _None._                               | :white_check_mark: |
| [`0x0008`](/world/server/0x0008/README.md) | `GP_SERV_COMMAND_ENTERZONE`          | `RecvEnterZone`               | _None._                               | :white_check_mark: |
| [`0x0009`](/world/server/0x0009/README.md) | `GP_SERV_COMMAND_MESSAGE`            | `RecvMessage`                 | _None._                               | :white_check_mark: |
| [`0x000A`](/world/server/0x000A/README.md) | `GP_SERV_COMMAND_LOGIN`              | `RecvLogIn`                   | _None._                               | :white_check_mark: |
| [`0x000B`](/world/server/0x000B/README.md) | `GP_SERV_COMMAND_LOGOUT`             | `RecvLogOut`                  | _None._                               | :white_check_mark: |
| [`0x000D`](/world/server/0x000D/README.md) | `GP_SERV_COMMAND_CHAR_PC`            | `RecvDebugPc`                 | `RecvCharPc`                          | :x: |
| [`0x000E`](/world/server/0x000E/README.md) | `GP_SERV_COMMAND_CHAR_NPC`           | `RecvDebugNpc`                | `RecvCharNpc`                         | :x: |
| [`0x0011`](/world/server/0x0011/README.md) | `GP_SERV_COMMAND_CHAR_DEL`           | _None._                       | `RecvCharDel`                         | :white_check_mark: |
| [`0x0012`](/world/server/0x0012/README.md) | `GP_SERV_COMMAND_GM`                 | `RecvGm`                      | _None._                               | :white_check_mark: |
| [`0x0013`](/world/server/0x0013/README.md) | `GP_SERV_COMMAND_GMCOMMAND`          | `RecvGmCommand`               | _None._                               | :white_check_mark: |
| [`0x0014`](/world/server/0x0014/README.md) | `GP_SERV_COMMAND_TELL`               | _None._                       | `RecvMessageTell`                     | :white_check_mark: |
| [`0x0016`](/world/server/0x0016/README.md) | `GP_SERV_COMMAND_TALK`               | _None._                       | `RecvMessageTalk`                     | :white_check_mark: |
| [`0x0017`](/world/server/0x0017/README.md) | `GP_SERV_COMMAND_CHAT_STD`           | `RecvStdChat`                 | _None._                               | :white_check_mark: |
| [`0x001B`](/world/server/0x001B/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :white_check_mark: |
| [`0x001C`](/world/server/0x001C/README.md) | `GP_SERV_COMMAND_ITEM_MAX`           | `RecvItemMax`                 | _None._                               | :white_check_mark: |
| [`0x001D`](/world/server/0x001D/README.md) | `GP_SERV_COMMAND_ITEM_SAME`          | `RecvItemSame`                | _None._                               | :white_check_mark: |
| [`0x001E`](/world/server/0x001E/README.md) | `GP_SERV_COMMAND_ITEM_NUM`           | `RecvItemNum`                 | `RecvItemChange`                      | :white_check_mark: |
| [`0x001F`](/world/server/0x001F/README.md) | `GP_SERV_COMMAND_ITEM_LIST`          | `RecvItemList`                | `RecvItemChange`                      | :white_check_mark: |
| [`0x0020`](/world/server/0x0020/README.md) | `GP_SERV_COMMAND_ITEM_ATTR`          | `RecvItemAttr`                | `RecvItemAttrChange`                  | :white_check_mark: |
| [`0x0021`](/world/server/0x0021/README.md) | `GP_SERV_COMMAND_ITEM_TRADE_REQ`     | `RecvItemTradeReq`            | `CTkTrade::TkRecvItemTradeReq`        | :white_check_mark: |
| [`0x0022`](/world/server/0x0022/README.md) | `GP_SERV_COMMAND_ITEM_TRADE_RES`     | `RecvItemTradeRes`            | `CTkTrade::TkRecvItemTradeRes`        | :white_check_mark: |
| [`0x0023`](/world/server/0x0023/README.md) | `GP_SERV_COMMAND_ITEM_TRADE_LIST`    | `RecvItemTradeList`           | `CTkTrade::TkRecvItemTradeList`       | :white_check_mark: |
| [`0x0024`](/world/server/0x0024/README.md) | `GP_SERV_COMMAND_ITEM_PRESENT`       | `RecvItemPresent`             | _None._                               | :white_check_mark: |
| [`0x0025`](/world/server/0x0025/README.md) | `GP_SERV_COMMAND_ITEM_TRADE_MYLIST`  | `RecvItemTradeMyList`         | _None._                               | :white_check_mark: |
| [`0x0026`](/world/server/0x0026/README.md) | `(UnknownName)`                      | `(UnknownName)`               | _None._                               | :white_check_mark: |
| [`0x0027`](/world/server/0x0027/README.md) | `GP_SERV_COMMAND_TALKNUMWORK2`       | _None._                       | `RecvMessageTalkNumWork2`             | :white_check_mark: |
| [`0x0028`](/world/server/0x0028/README.md) | `GP_SERV_COMMAND_BATTLE2`            | _None._                       | `RecvBattleCalc2`                     | :white_check_mark: |
| [`0x0029`](/world/server/0x0029/README.md) | `GP_SERV_COMMAND_BATTLE_MESSAGE`     | _None._                       | `RecvBattleMessage`                   | :white_check_mark: |
| [`0x002A`](/world/server/0x002A/README.md) | `GP_SERV_COMMAND_TALKNUMWORK`        | _None._                       | `RecvMessageTalkNumWork`              | :white_check_mark: |
| [`0x002B`](/world/server/0x002B/README.md) | `GP_SERV_COMMAND_CHANNEL_ITEM`       | `RecvChannelItem`             | _None._                               | :white_check_mark: |
| [`0x002C`](/world/server/0x002C/README.md) | `GP_SERV_COMMAND_CHANNEL_STATE`      | `RecvChannelState`            | _None._                               | :white_check_mark: |
| [`0x002D`](/world/server/0x002D/README.md) | `GP_SERV_COMMAND_BATTLE_MESSAGE2`    | _None._                       | `RecvBattleMessage2`                  | :white_check_mark: |
| [`0x002E`](/world/server/0x002E/README.md) | `GP_SERV_COMMAND_OPENMOGMENU`        | _None._                       | `RecvOpenMogMenu`                     | :white_check_mark: |
| [`0x002F`](/world/server/0x002F/README.md) | `GP_SERV_COMMAND_DIG`                | _None._                       | `RecvDig`                             | :white_check_mark: |
| [`0x0030`](/world/server/0x0030/README.md) | `GP_SERV_COMMAND_EFFECT`             | _None._                       | `RecvEffect`                          | :white_check_mark: |
| [`0x0031`](/world/server/0x0031/README.md) | `GP_SERV_COMMAND_RECIPE`             | _None._                       | `RecvRecipe`                          | :white_check_mark: |
| [`0x0032`](/world/server/0x0032/README.md) | `GP_SERV_COMMAND_EVENT`              | _None._                       | `RecvEventCalc`                       | :white_check_mark: |
| [`0x0033`](/world/server/0x0033/README.md) | `GP_SERV_COMMAND_EVENTSTR`           | _None._                       | `RecvEventCalcStr`                    | :white_check_mark: |
| [`0x0034`](/world/server/0x0034/README.md) | `GP_SERV_COMMAND_EVENTNUM`           | _None._                       | `RecvEventCalcNum`                    | :white_check_mark: |
| [`0x0036`](/world/server/0x0036/README.md) | `GP_SERV_COMMAND_TALKNUM`            | _None._                       | `RecvMessageTalkNum`                  | :white_check_mark: |
| [`0x0037`](/world/server/0x0037/README.md) | `GP_SERV_COMMAND_SERVERSTATUS`       | _None._                       | `RecvServerStatus`                    | :white_check_mark: |
| [`0x0038`](/world/server/0x0038/README.md) | `GP_SERV_COMMAND_SCHEDULOR`          | _None._                       | `RecvSchedulor`                       | :white_check_mark: |
| [`0x0039`](/world/server/0x0039/README.md) | `GP_SERV_COMMAND_MAPSCHEDULOR`       | _None._                       | `RecvMapSchedulor`                    | :white_check_mark: |
| [`0x003A`](/world/server/0x003A/README.md) | `GP_SERV_COMMAND_MAGICSCHEDULOR`     | _None._                       | `RecvMagicSchedulor`                  | :white_check_mark: |
| [`0x003B`](/world/server/0x003B/README.md) | `GP_SERV_COMMAND_EVENTMES`           | _None._                       | `RecvEventMes`                        | :white_check_mark: |
| [`0x003C`](/world/server/0x003C/README.md) | `GP_SERV_COMMAND_SHOP_LIST`          | `RecvShopList`                | `(UnknownName)`                       | :white_check_mark: |
| [`0x003D`](/world/server/0x003D/README.md) | `GP_SERV_COMMAND_SHOP_SELL`          | `RecvShopSell`                | `YkWndShop::RecvShopSell`             | :white_check_mark: |
| [`0x003E`](/world/server/0x003E/README.md) | `GP_SERV_COMMAND_SHOP_OPEN`          | `RecvShopOpen`                | `YkWndShopMain::RecvShopOpen`         | :white_check_mark: |
| [`0x003F`](/world/server/0x003F/README.md) | `GP_SERV_COMMAND_SHOP_BUY`           | `RecvShopBuy`                 | `(UnknownName)`                       | :white_check_mark: |
| [`0x0040`](/world/server/0x0040/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :white_check_mark: |
| [`0x0041`](/world/server/0x0041/README.md) | `GP_SERV_COMMAND_BLACK_LIST`         | `RecvBlacklist`               | _None._                               | :white_check_mark: |
| [`0x0042`](/world/server/0x0042/README.md) | `GP_SERV_COMMAND_BLACK_EDIT`         | `RecvBlackEdit`               | _None._                               | :white_check_mark: |
| [`0x0043`](/world/server/0x0043/README.md) | `GP_SERV_COMMAND_TALKNUMNAME`        | _None._                       | `RecvMessageTalkNumName`              | :white_check_mark: |
| [`0x0044`](/world/server/0x0044/README.md) | `(UnknownName)`                      | `(UnknownName)`               | _None._                               | :white_check_mark: |
| [`0x0047`](/world/server/0x0047/README.md) | `(UnknownName)`                      | `(UnknownName)`               | _None._                               | :white_check_mark: |
| [`0x0048`](/world/server/0x0048/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :warning: |
| [`0x0049`](/world/server/0x0049/README.md) | `(UnknownName)`                      | `(UnknownName)`               | _None._                               | :white_check_mark: |
| [`0x004B`](/world/server/0x004B/README.md) | `GP_SERV_COMMAND_PBX_RESULT`         | _None._                       | `RecvReqPostReplyCommon`              | :warning: |
| [`0x004C`](/world/server/0x004C/README.md) | `GP_SERV_COMMAND_AUC`                | _None._                       | `RecvAucCommon`                       | :warning: |
| [`0x004D`](/world/server/0x004D/README.md) | `GP_SERV_COMMAND_FRAGMENTS`          | _None._                       | `RecvFragments`                       | :white_check_mark: |
| [`0x004F`](/world/server/0x004F/README.md) | `GP_SERV_COMMAND_EQUIP_CLEAR`        | `RecvEquipClear`              | _None._                               | :white_check_mark: |
| [`0x0050`](/world/server/0x0050/README.md) | `GP_SERV_COMMAND_EQUIP_LIST`         | `RecvEquipList`               | _None._                               | :white_check_mark: |
| [`0x0051`](/world/server/0x0051/README.md) | `GP_SERV_COMMAND_GRAP_LIST`          | _None._                       | `RecvGrapList`                        | :white_check_mark: |
| [`0x0052`](/world/server/0x0052/README.md) | `GP_SERV_COMMAND_EVENTUCOFF`         | _None._                       | `RecvUcOff`                           | :white_check_mark: |
| [`0x0053`](/world/server/0x0053/README.md) | `GP_SERV_COMMAND_SYSTEMMES`          | _None._                       | `RecvSystemMessage`                   | :white_check_mark: |
| [`0x0054`](/world/server/0x0054/README.md) | `GP_SERV_COMMAND_DEBUGPRINT`         | _None._                       | `RecvDebufPrint`                      | :white_check_mark: |
| [`0x0055`](/world/server/0x0055/README.md) | `GP_SERV_COMMAND_SCENARIOITEM`       | _None._                       | `RecvScenarioItem`                    | :white_check_mark: |
| [`0x0056`](/world/server/0x0056/README.md) | `GP_SERV_COMMAND_MISSION`            | _None._                       | `RecvMissionItem`                     | :white_check_mark: |
| [`0x0057`](/world/server/0x0057/README.md) | `GP_SERV_COMMAND_WEATHER`            | _None._                       | `RecvWeather`                         | :white_check_mark: |
| [`0x0058`](/world/server/0x0058/README.md) | `GP_SERV_COMMAND_ASSIST`             | _None._                       | `RecvAssist`                          | :white_check_mark: |
| [`0x0059`](/world/server/0x0059/README.md) | `GP_SERV_COMMAND_FRIENDPASS`         | _None._                       | `RecvFriendPass`                      | :white_check_mark: |
| [`0x005A`](/world/server/0x005A/README.md) | `GP_SERV_COMMAND_MOTIONMES`          | _None._                       | `RecvEmotionMes`                      | :white_check_mark: |
| [`0x005B`](/world/server/0x005B/README.md) | `GP_SERV_COMMAND_WPOS`               | _None._                       | `RecvWpos`                            | :white_check_mark: |
| [`0x005C`](/world/server/0x005C/README.md) | `GP_SERV_COMMAND_PENDINGNUM`         | _None._                       | `RecvPendingNum`                      | :white_check_mark: |
| [`0x005D`](/world/server/0x005D/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :white_check_mark: |
| [`0x005E`](/world/server/0x005E/README.md) | `GP_SERV_COMMAND_CONQUEST`           | _None._                       | `RecvConquest`                        | :x: |
| [`0x005F`](/world/server/0x005F/README.md) | `GP_SERV_COMMAND_MUSIC`              | _None._                       | `RecvMusic`                           | :white_check_mark: |
| [`0x0060`](/world/server/0x0060/README.md) | `GP_SERV_COMMAND_MUSICVOLUME`        | _None._                       | `RecvMusicVolume`                     | :white_check_mark: |
| [`0x0061`](/world/server/0x0061/README.md) | `GP_SERV_COMMAND_CLISTATUS`          | _None._                       | `RecvCliStatus`                       | :white_check_mark: |
| [`0x0062`](/world/server/0x0062/README.md) | `GP_SERV_COMMAND_CLISTATUS2`         | _None._                       | `RecvCliStatus2`                      | :white_check_mark: |
| [`0x0063`](/world/server/0x0063/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :white_check_mark: |
| [`0x0064`](/world/server/0x0064/README.md) | `GP_SERV_COMMAND_PREFERENCE_DATA`    | _None._                       | `receivePreferenceData`               | :white_check_mark: |
| [`0x0065`](/world/server/0x0065/README.md) | `GP_SERV_COMMAND_WPOS2`              | _None._                       | `RecvWpos2`                           | :white_check_mark: |
| [`0x0067`](/world/server/0x0067/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :white_check_mark: |
| [`0x0068`](/world/server/0x0068/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :white_check_mark: |
| [`0x0069`](/world/server/0x0069/README.md) | `(UnknownName)`                      | `(UnknownName)`               | _None._                               | :white_check_mark: |
| [`0x006F`](/world/server/0x006F/README.md) | `GP_SERV_COMMAND_COMBINE_ANS`        | `gcRecvCombine`               | _None._                               | :white_check_mark: |
| [`0x0070`](/world/server/0x0070/README.md) | `GP_SERV_COMMAND_COMBINE_INF`        | `gcRecvCombineInfo`           | _None._                               | :white_check_mark: |
| [`0x0071`](/world/server/0x0071/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :white_check_mark: |
| [`0x0072`](/world/server/0x0072/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :x: |
| [`0x0073`](/world/server/0x0073/README.md) | `(UnknownName)`                      | `(UnknownName)`               | _None._                               | :x: |
| [`0x0074`](/world/server/0x0074/README.md) | `(UnknownName)`                      | `(UnknownName)`               | _None._                               | :x: |
| [`0x0075`](/world/server/0x0075/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :white_check_mark: |
| [`0x0076`](/world/server/0x0076/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :white_check_mark: |
| [`0x0077`](/world/server/0x0077/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :white_check_mark: |
| [`0x0078`](/world/server/0x0078/README.md) | `GP_SERV_COMMAND_SWITCH_START`       | `RecvStart`                   | _None._                               | :white_check_mark: |
| [`0x0079`](/world/server/0x0079/README.md) | `GP_SERV_COMMAND_SWITCH_PROC`        | `RecvProc`                    | _None._                               | :white_check_mark: |
| [`0x0081`](/world/server/0x0081/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :x: |
| [`0x0082`](/world/server/0x0082/README.md) | `GP_SERV_COMMAND_GUILD_BUY`          | `gcRecvGuildBuy`              | `RecvGuildBuy`                        | :white_check_mark: |
| [`0x0083`](/world/server/0x0083/README.md) | `GP_SERV_COMMAND_GUILD_BUYLIST`      | `gcRecvGuildBuyList`          | _None._                               | :white_check_mark: |
| [`0x0084`](/world/server/0x0084/README.md) | `GP_SERV_COMMAND_GUILD_SELL`         | `gcRecvGuildSell`             | `RecvGuildSell`                       | :white_check_mark: |
| [`0x0085`](/world/server/0x0085/README.md) | `GP_SERV_COMMAND_GUILD_SELLLIST`     | `gcRecvGuildSellList`         | _None._                               | :white_check_mark: |
| [`0x0086`](/world/server/0x0086/README.md) | `GP_SERV_COMMAND_GUILD_OPEN`         | `gcRecvGuildOpen`             | `YkWndGuildMain::RecvGuildOpen`       | :white_check_mark: |
| [`0x008C`](/world/server/0x008C/README.md) | `(UnknownName)`                      | `(UnknownName)`               | _None._                               | :white_check_mark: |
| [`0x008D`](/world/server/0x008D/README.md) | `(UnknownName)`                      | `(UnknownName)`               | _None._                               | :white_check_mark: |
| [`0x0096`](/world/server/0x0096/README.md) | `GP_SERV_COMMAND_MYROOM_ENTER`       | _None._                       | `RecvMyroomEnter`                     | :white_check_mark: |
| [`0x0097`](/world/server/0x0097/README.md) | `GP_SERV_COMMAND_MYROOM_EXIT`        | _None._                       | `RecvMyroomExit`                      | :white_check_mark: |
| [`0x0098`](/world/server/0x0098/README.md) | `GP_SERV_COMMAND_MYROOM_IS`          | _None._                       | `RecvMyroomIs`                        | :white_check_mark: |
| [`0x0099`](/world/server/0x0099/README.md) | `GP_SERV_COMMAND_MYROOM_EXIST`       | _None._                       | `RecvMyroomExist`                     | :white_check_mark: |
| [`0x009A`](/world/server/0x009A/README.md) | `GP_SERV_COMMAND_MYROOM_PLANT`       | _None._                       | `RecvMyroomPlant`                     | :white_check_mark: |
| [`0x009B`](/world/server/0x009B/README.md) | `GP_SERV_COMMAND_MYROOM_RAISE`       | _None._                       | `RecvMyroomRaise`                     | :white_check_mark: |
| [`0x009C`](/world/server/0x009C/README.md) | `GP_SERV_COMMAND_MYROOM_HARVEST`     | _None._                       | `RecvMyroomHarvest`                   | :white_check_mark: |
| [`0x009D`](/world/server/0x009D/README.md) | `GP_SERV_COMMAND_MYROOM_DIARY`       | _None._                       | `RecvMyroomDiary`                     | :white_check_mark: |
| [`0x009E`](/world/server/0x009E/README.md) | `GP_SERV_COMMAND_MYROOM_PLACE`       | _None._                       | `RecvMyroomJob`                       | :white_check_mark: |
| [`0x00A0`](/world/server/0x00A0/README.md) | `GP_SERV_COMMAND_MAP_GROUP`          | _None._                       | `RecvMapGroup`                        | :white_check_mark: |
| [`0x00AA`](/world/server/0x00AA/README.md) | `GP_SERV_COMMAND_MAGIC_DATA`         | `RecvMagicData`               | _None._                               | :white_check_mark: |
| [`0x00AB`](/world/server/0x00AB/README.md) | `GP_SERV_COMMAND_FEAT_DATA`          | `RecvFeatData`                | _None._                               | :white_check_mark: |
| [`0x00AC`](/world/server/0x00AC/README.md) | `GP_SERV_COMMAND_COMMAND_DATA`       | `RecvCommandData`             | _None._                               | :white_check_mark: |
| [`0x00AD`](/world/server/0x00AD/README.md) | `(UnknownName)`                      | `(UnknownName)`               | _None._                               | :white_check_mark: |
| [`0x00AE`](/world/server/0x00AE/README.md) | `(UnknownName)`                      | `(UnknownName)`               | _None._                               | :white_check_mark: |
| [`0x00B4`](/world/server/0x00B4/README.md) | `GP_SERV_COMMAND_CONFIG`             | `FsChatFilterServerCallback`  | `RecvConf`                            | :white_check_mark: |
| [`0x00B5`](/world/server/0x00B5/README.md) | `GP_SERV_COMMAND_FAQ_GMPARAM`        | `FDtRecvGmParam`              | _None._                               | :white_check_mark: |
| [`0x00B6`](/world/server/0x00B6/README.md) | `GP_SERV_COMMAND_SET_GMMSG`          | `FDtRecvGmNotice`             | _None._                               | :white_check_mark: |
| [`0x00B7`](/world/server/0x00B7/README.md) | `GP_SERV_COMMAND_GMSCITEM`           | _None._                       | `RecvGmScitem`                        | :white_check_mark: |
| [`0x00BF`](/world/server/0x00BF/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :white_check_mark: |
| [`0x00C8`](/world/server/0x00C8/README.md) | `GP_SERV_COMMAND_GROUP_TBL`          | `RecvGroupTbl`                | _None._                               | :white_check_mark: |
| [`0x00C9`](/world/server/0x00C9/README.md) | `GP_SERV_COMMAND_EQUIP_INSPECT`      | `RecvEquipInspect`            | _None._                               | :white_check_mark: |
| [`0x00CA`](/world/server/0x00CA/README.md) | `GP_SERV_COMMAND_INSPECT_MESSAGE`    | `RecvInspectMessage`          | `CTkInspect::RecvBazar`               | :white_check_mark: |
| [`0x00CC`](/world/server/0x00CC/README.md) | `GP_SERV_COMMAND_LINKSHELL_MESSAGE`  | `RecvComlinkMessage`          | _None._                               | :white_check_mark: |
| [`0x00D2`](/world/server/0x00D2/README.md) | `GP_SERV_COMMAND_TROPHY_LIST`        | `RecvTrophyList`              | _None._                               | :white_check_mark: |
| [`0x00D3`](/world/server/0x00D3/README.md) | `GP_SERV_COMMAND_TROPHY_SOLUTION`    | `RecvTrophySolution`          | _None._                               | :white_check_mark: |
| [`0x00DC`](/world/server/0x00DC/README.md) | `GP_SERV_COMMAND_GROUP_SOLICIT_REQ`  | `RecvGroupSolicitreq`         | `YkWndPartyList::RecvGroupSolicitReq` | :white_check_mark: |
| [`0x00DD`](/world/server/0x00DD/README.md) | `GP_SERV_COMMAND_GROUP_LIST`         | `RecvGroupList`               | _None._                               | :white_check_mark: |
| [`0x00DE`](/world/server/0x00DE/README.md) | `GP_SERV_COMMAND_GROUP_SOLICIT_NO`   | `RecvGroupSolicitNo`          | _None._                               | :white_check_mark: |
| [`0x00DF`](/world/server/0x00DF/README.md) | `GP_SERV_COMMAND_GROUP_ATTR`         | `RecvGroupAttr`               | _None._                               | :white_check_mark: |
| [`0x00E0`](/world/server/0x00E0/README.md) | `GP_SERV_COMMAND_GROUP_COMLINK`      | `RecvComlink`                 | _None._                               | :white_check_mark: |
| [`0x00E1`](/world/server/0x00E1/README.md) | `GP_SERV_COMMAND_GROUP_CHECKID`      | `RecvCheckID`                 | _None._                               | :white_check_mark: |
| [`0x00E2`](/world/server/0x00E2/README.md) | `GP_SERV_COMMAND_GROUP_LIST2`        | `RecvGroupList2`              | _None._                               | :white_check_mark: |
| [`0x00E6`](/world/server/0x00E6/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :white_check_mark: |
| [`0x00F4`](/world/server/0x00F4/README.md) | `GP_SERV_COMMAND_TRACKING_LIST`      | `RecvList`                    | _None._                               | :white_check_mark: |
| [`0x00F5`](/world/server/0x00F5/README.md) | `GP_SERV_COMMAND_TRACKING_POS`       | `RecvPos`                     | _None._                               | :white_check_mark: |
| [`0x00F6`](/world/server/0x00F6/README.md) | `GP_SERV_COMMAND_TRACKING_STATE`     | `RecvState`                   | `YkWndTrack::RecvListEnd`             | :white_check_mark: |
| [`0x00F9`](/world/server/0x00F9/README.md) | `GP_SERV_COMMAND_RES`                | _None._                       | `RecvServRes`                         | :x: |
| [`0x00FA`](/world/server/0x00FA/README.md) | `GP_SERV_COMMAND_MYROOM_OPERATION`   | _None._                       | `RecvOperation`                       | :x: |
| [`0x0105`](/world/server/0x0105/README.md) | `GP_SERV_COMMAND_BAZAAR_LIST`        | `RecvList`                    | _None._                               | :x: |
| [`0x0106`](/world/server/0x0106/README.md) | `GP_SERV_COMMAND_BAZAAR_BUY`         | `RecvBuy`                     | `RecvBazarBuy`                        | :x: |
| [`0x0107`](/world/server/0x0107/README.md) | `GP_SERV_COMMAND_BAZAAR_CLOSE`       | `RecvClose`                   | `RecvBazarClose`                      | :x: |
| [`0x0108`](/world/server/0x0108/README.md) | `GP_SERV_COMMAND_BAZAAR_SHOPPING`    | `RecvShopping`                | `RecvBazarShop`                       | :x: |
| [`0x0109`](/world/server/0x0109/README.md) | `GP_SERV_COMMAND_BAZAAR_SELL`        | `RecvSell`                    | `RecvBazarSell`                       | :x: |
| [`0x010A`](/world/server/0x010A/README.md) | `GP_SERV_COMMAND_BAZAAR_SALE`        | `RecvSale`                    | _None._                               | :x: |
| [`0x010E`](/world/server/0x010E/README.md) | `GP_SERV_COMMAND_REQSUBMAPNUM`       | _None._                       | `RecvSubMapNum`                       | :x: |
| [`0x010F`](/world/server/0x010F/README.md) | `GP_SERV_COMMAND_REQLOGOUTINFO`      | _None._                       | `RecvLogoutInfo`                      | :x: |
| [`0x0110`](/world/server/0x0110/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :x: |
| [`0x0111`](/world/server/0x0111/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :x: |
| [`0x0112`](/world/server/0x0112/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :x: |
| [`0x0113`](/world/server/0x0113/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :x: |
| [`0x0115`](/world/server/0x0115/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :x: |
| [`0x0116`](/world/server/0x0116/README.md) | `(UnknownName)`                      | `(UnknownName)`               | _None._                               | :x: |
| [`0x0117`](/world/server/0x0117/README.md) | `(UnknownName)`                      | `(UnknownName)`               | _None._                               | :x: |
| [`0x0118`](/world/server/0x0118/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :x: |
| [`0x0119`](/world/server/0x0119/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :x: |
| [`0x011A`](/world/server/0x011A/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :x: |
| [`0x011B`](/world/server/0x011B/README.md) | `(UnknownName)`                      | `(UnknownName)`               | _None._                               | :x: |
| [`0x011C`](/world/server/0x011C/README.md) | `(UnknownName)`                      | `(UnknownName)`               | _None._                               | :x: |
| [`0x011D`](/world/server/0x011D/README.md) | `(UnknownName)`                      | `(UnknownName)`               | `(UnknownName)`                       | :x: |
| [`0x011E`](/world/server/0x011E/README.md) | `(UnknownName)`                      | _None._                       | `(UnknownName)`                       | :x: |

_**Note:** The names for the above packet commands and client handler functions are real names taken from the PS2 Beta debug information/symbols. Packets that are marked as `(UnknownName)` either did not have a named command or handler in the PS2 beta, or they were added after the PS2 beta. Only real valid names are listed regardless if the packet purpose is understood._

Every incoming packet sent from the server can have up to two callback handlers registered to be invoked when the packet is received. Not all packets use this system and only register a single handler. The client handles packet registration via the `gcZoneRecvCallback` and `gcZoneRecvCallback2` functions. These functions register the given function pointer to the given packet id within the respective command array tables:

  - `gcZoneRecvCallback` - Registers packets to the internal command pointer array: `pZoneSys->pCommandRunTbl`
  - `gcZoneRecvCallback2` - Registers packets to the internal command pointer array: `pZoneSys->pCommandRunTbl2`

When the client receives a packet chunk from the server, it will first decompress and decrypt the chunk. It will then begin looping over the chunks data to process each packet within the chunk. The client will perform some basic validation on the chunk _(ie. checking the sync count, validating the id and size of the packet, etc.)_ before attempting to process any individual packet from the chunk. Once validated, the client will begin looping over the chunks data to process the inner packets. The client will then check to see if a valid packet handler has been registered via `gcZoneRecvCallback2`. If this handler does exist, it will be invoked. The client then checks for a certain zone flag conditions and if a second handler has been registered with `gcZoneRecvCallback`. If this handler is also valid the client will then also invoke it.

This dual-packet handler system is used by the client to allow separation of the client state updates and the user interface updates. Generally, when two handlers are registered for a given packet id, it is done to allow the first handler _(registered with `gcZoneRecvCallback2`)_ to update the client state/memory. Then the second handler _(registered with `gcZoneRecvCallback`)_ is used to update the user interface in some manner, generally by either opening, closing, or transitioning a window in response to the packet.

The table above will show the handlers in the following order:

  - `Handler Name (1)` - The main callback handler. _(Registered via `gcZoneRecvCallback2`.)_
  - `Handler Name (2)` - The secondary callback handler. _(Registered via `gcZoneRecvCallback`.)_
