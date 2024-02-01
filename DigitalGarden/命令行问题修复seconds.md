(zbs-client-py) [root@8600e54d78dc zbs-client-py]#  zbs-task -vv -fjson --taskd_ip 192.168.23.103  --taskd_port 10600  task  list_by_status finished 
[<zbs.lib.utils.proto_option_handler.ProtoStrWrap object at 0x7f53007a6fb0>]
**1704165282.595**

修改后

(zbs-client-py) [root@8600e54d78dc zbs-client-py]#  zbs-task  --taskd_ip 172.17.0.1  --taskd_port 10600  task  list_by_status finished 
id                                    state    progress    runner        create_time          finished_time        last_fail_time    error_code
------------------------------------  -------  ----------  ------------  -------------------  -------------------  ----------------  ------------
e63096c4-6e34-4edc-a0d1-bfc2f21cb8cc  SUCCEED  100.00%     10.132.79.51  2024-01-02 03:14:42  2024-01-02 03:16:47  None              UOK