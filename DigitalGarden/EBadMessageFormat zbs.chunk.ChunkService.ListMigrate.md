[EBadMessageFormat] bad response when responsing method: zbs.chunk.ChunkService.ListMigrate
BadResponse: speed: 76808192


在Protocol Buffers中,如果解析后的message对象调用IsInitialized()返回false,表示该消息没有被正确初始化,处理方法可以有:1. 检查required字段是否设置查看消息定义,是否缺少必填(required)字段的设置。需要确认所有required字段都有设置。


```c++ 5.5.1-rc4
static RecoverInfo RecoverCmdToRecoverInfo(const RecoverCmd& cmd) {

RecoverInfo info;

info.set_pid(cmd.pid());

info.set_state(RecoverState::INIT);

// always be kInvalidBlockNumber

info.set_cur_block(kInvalidBlockNumber);

info.set_src_cid(cmd.src_chunk());

info.set_dst_cid(cmd.dst_chunk());

info.set_is_migrate(cmd.is_migrate());

if (cmd.has_replace_chunk()) {

info.set_replace_cid(cmd.replace_chunk());

}

if (cmd.has_epoch()) {

info.set_epoch(cmd.epoch());

}

return info;

}
```


```c++
static RecoverInfo RecoverCmdToRecoverInfo(const RecoverCmd& cmd) {

RecoverInfo info;

info.set_pid(cmd.pid());

info.set_state(RecoverState::INIT);

// always be kInvalidBlockNumber

info.set_cur_block(kInvalidBlockNumber);

info.set_src_cid(cmd.src_chunk());

info.set_dst_cid(cmd.dst_chunk());

info.set_is_migrate(cmd.is_migrate());

if (cmd.has_replace_chunk()) {

info.set_replace_cid(cmd.replace_chunk());

}

if (cmd.has_epoch()) {

info.set_epoch(cmd.epoch());

}

return info;

}
```


ctx->set_silence_ms(0);

std::deque<RecoverCmd> pending_recover_cmds_;
std::unordered_map<pid_t, RecoverContext*> in_recover_;

```
ListRecoverResponse response;

handler->ListRecoverInfo(&response);

LOG(ERROR) << response.DebugString();

ASSERT_TRUE(response.IsInitialized());
```