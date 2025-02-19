<div align="center">
    <img width="200" src="https://github.com/atom0s/XiPackets/raw/main/repo/icon.png" alt="XiPackets">
    </br>
</div>

# Patch Server Packets

The following list is the various packets that are currently sent between the client and `patch` server:

  - :x: - _Initial documentation incomplete._
  - :warning: - _Initial documentation partially finished._
  - :white_check_mark: = _Initial documentation done._

| Id | Direction | Packet Function Name | First Pass Documented |
| --- | --- | --- | --- |
| [`0x0001`](/patch/packets/0x0001/README.md) | `C -> S` | `_sqPatchPkt_MakeStatusRequest`  | :white_check_mark: |
| [`0x0002`](/patch/packets/0x0002/README.md) | `S -> C` | `_sqPatchPkt_CnvStatusReply`     | :white_check_mark: |
| [`0x0003`](/patch/packets/0x0003/README.md) | `C -> S` | `_sqPatchPkt_MakeDownLoadRequest`| :white_check_mark: |
| [`0x0004`](/patch/packets/0x0004/README.md) | `S -> C` | `_sqPatchPkt_CnvSendFile`        | :white_check_mark: |
| [`0x0005`](/patch/packets/0x0005/README.md) | `S -> C` | `_sqPatchPkt_CnvError`           | :white_check_mark: |
| [`0x0006`](/patch/packets/0x0006/README.md) | `C -> S` | `_sqPatchPkt_MakeShutDown`       | :white_check_mark: |
| [`0x0007`](/patch/packets/0x0007/README.md) | `C -> S` | `_sqPatchPkt_MakeAskNewest`      | :white_check_mark: |
| [`0x0008`](/patch/packets/0x0008/README.md) | `S -> C` | `_sqPatchPkt_CnvReplyNewest`     | :white_check_mark: |
