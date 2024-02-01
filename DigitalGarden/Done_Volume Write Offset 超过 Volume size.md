
```bash
 sudo zbs-chunk volume write default test  1073741824 1 -i /dev/zero
default/test N/A% |#                                                                             |   0.0 s/B ETA:  --:--:--2024-01-24 22:56:12,520:48504(0x7fd412543700):ZOO_INFO@zookeeper_init_internal@1191: Initiating client connection, host=10.11.131.56:2181 sessionTimeout=4000 watcher=0x7fd4194204c0 sessionId=0 sessionPasswd=<null> context=0x7fd408021680 flags=2
2024-01-24 22:56:12,521:48504(0x7fd41138f700):ZOO_INFO@check_events@2480: initiated connection to server [10.11.131.56:2181]
2024-01-24 22:56:12,524:48504(0x7fd41138f700):ZOO_INFO@check_events@2532: session establishment complete on server [10.11.131.56:2181], sessionId=0x20007abde050015, negotiated timeout=4000 
2024-01-24 22:56:12,623:48504(0x7fd412543700):ZOO_INFO@zookeeper_close@3338: Closing zookeeper sessionId=0x20007abde050015 to [10.11.131.56:2181]

2024-01-24 22:56:12,626:48504(0x7fd412543700):ZOO_INFO@zookeeper_close@3360: Freeing zookeeper resources for sessionId=0x20007abde050015

2024-01-24 22:56:12,641, ERROR, cmd.py:1172, Failed to write 
Traceback:
[EBadRequest]: Not a resize request vextent_no: 4 num_vextents: 4

default/test 100% |##############################################################################|   3.1 B/s Time:  0:00:00
```

```c++
    // from now on, vextent_no >= num_vextents. This case occurs when handling some overflow writes, such as NFS File
    // writes.
    if (UNLIKELY(request.no_alloc())) {
        return Status(ENotAlloc);
    }

    if (!request.resize()) {
        return Status(EBadRequest) << "Not a resize request. vextent_no: " << vextent_no
                                   << " num_vextents: " << num_vextents;
    }
```

```bash
2024-01-24 23:10:43,407, ERROR, cmd.py:1174, Failed to write b'\nTraceback:\n[EBadRequest]: Not a resize request vextent_no: 4 num_vextents: 4\n'
default/test 100% |##############################################################################|   0.4 B/s Time:  0:00:02
```
问题可复现， 怀疑是 Golang 的问题
zbs-v5.4.3-rc7

```c++
Status MetaRpcServer::GetLeaseForWrite(const GetVExtentLeaseRequest& request,
                                       vextent_t *vextent,
                                       SessionInfo *owner) {
        if (vextent_no >= num_vextents) {
            if (!request.resize())
                return Status(EBadRequest) << "Not a resize request"
                                           << " vextent_no: " << vextent_no
                                           << " num_vextents: " << num_vextents;
            num_vextents = AlignedUp(vextent_no + 1, volume.stripe_num());
        }
```
只要给 resize 置为 True 就能做到

```protobuf master
message GetVExtentLeaseRequest {
    required bytes vtable_id = 1 [(zbs.labels).as_str = true];
    required uint32 vextent_no = 2;
    optional bytes preferred_session = 3 [default = "", (zbs.labels).as_str = true];

    // if it is a write request
    optional bool is_write = 4 [default = false];

    // if resize as needed
    optional bool resize = 5 [default = false];
    optional uint32 preferred_cid = 6;
    optional bool no_alloc = 7 [default = false];
}
```

```protobuf v5.4.x
message GetVExtentLeaseRequest {
    required bytes vtable_id = 1 [(zbs.labels).as_str = true];
    required uint32 vextent_no = 2;
    optional bytes preferred_session = 3 [default = "", (zbs.labels).as_str = true];

    // if it is a write request
    optional bool is_write = 4 [default = false];

    // if resize as needed
    optional bool resize = 5 [default = false];
    optional uint32 preferred_cid = 6;
    optional bool no_alloc = 7 [default = false];
}
```


```c++
void IOHandler::HandleVolumeWrite(CSReq* req) {
    Status st;
    IOController* ctrl           = req->arena->CreateObject<IOController>();
    const MessageHeader* req_hdr = req->req_hdr;
    // TODO(Wenhao): Now we only consider the usage for backup snapshots. It is
    // not suitable for general volume i/o yet. I/O Contention should be
    // considered if it is used for general volume usage. The best place to
    // support this is in the access io handler. This is left for future
    // improvement.
    if (zbs::chunk::is_delta_write(req_hdr->volume_info.flags)) {
        st = zbs_rsync_->PatchDelta(zbs_client_,
                                    req_hdr->volume_info.volume_id,
                                    req_hdr->volume_info.data_offset,
                                    req->buf,
                                    req_hdr->buf_len,
                                    req_hdr->volume_info.preferred_cid);
        ctrl->SetStatus(st);
        WriteDone(ctrl, req);
    } else {
        struct io_option* option = ctrl->GetIOOption();
        option->flags         = req_hdr->volume_info.flags;
        option->preferred_cid = req_hdr->volume_info.preferred_cid;
        option->resize        = true;

        zbs_client_->Write(req_hdr->volume_info.volume_id, req->buf, req_hdr->volume_info.data_len,
                           req_hdr->volume_info.data_offset, ctrl, BIND(&IOHandler::WriteDone, this, ctrl, req),
                           nullptr);
    }
}
```


```go zbs-client-go
const (
	NO_PROMOTION IOFlags = 1 << iota
	DELTA_WRITE
	NO_STRIPE
	RECOVER_IO
	MIGRATE_IO
	USE_COMPRESS
	ALLOW_SLOW_IO
)
```