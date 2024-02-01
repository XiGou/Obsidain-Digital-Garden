Thu 2023-11-16 14:24:30 [PID:4372235] INFO ## reroute target check finished, the status is local scvm normal, route target ip now is 10.170.177.85                
Thu 2023-11-16 14:24:30 [PID:4372235] WARNING send heartbeat to 10.170.177.85 failed                                                                              
Thu 2023-11-16 14:24:32 [PID:4372235] INFO ## reroute target check finished, the status is local scvm normal, route target ip now is 10.170.177.85                
Thu 2023-11-16 14:24:32 [PID:4372235] WARNING send heartbeat to 10.170.177.85 failed   



Thu 2023-11-16 11:00:00 [PID:4372235] INFO ## reroute target check finished, the status is local scvm normal, route target ip now is 10.170.177.85
Thu 2023-11-16 11:00:00 [PID:4372235] WARNING send heartbeat to 10.170.177.85 failed
Thu 2023-11-16 11:00:03 [PID:4372235] INFO ## reroute target check finished, the status is local scvm normal, route target ip now is 10.170.177.85
Thu 2023-11-16 11:00:03 [PID:4372235] WARNING send heartbeat to 10.170.177.85 failed
Thu 2023-11-16 11:00:06 [PID:4372235] INFO ## reroute target check finished, the status is local scvm normal, route target ip now is 10.170.177.85
Thu 2023-11-16 11:00:10 [PID:4372235] WARNING send heartbeat to 10.170.177.85 failed
Thu 2023-11-16 11:00:13 [PID:4372235] INFO ## reroute target check finished, the statu

相当于 zbs 的 19:00

I1116 14:14:04.280752 13779 zookeeper.cc:136] Done destructing zookeeper.
I1116 14:14:04.280773 13779 meta.cc:1900] [LIBMETA ADDR UPDATE]  old meta ip: 10.170.177.85 old meta port: 10100 new meta ip: 10.170.177.85 new meta port: 10100
W1116 14:14:04.280793 13779 event_loop.cc:181] del non-existing event for fd 31
W1116 14:14:04.280841 13779 event_loop.cc:189]  [SYSCALL]: ::epoll_ctl(efd_, EPOLL_CTL_DEL, fd, nullptr). [ARG LIST]: fd: 31 .[SYSMSG]: No such file or directory [2].
I1116 14:14:04.280884 13779 proto_async_client.h:180] Connect to 0.0.0.0:0-->10.170.177.85:10100 with timeout_ms 200
I1116 14:14:04.281118 13779 proto_async_client.h:366] Connected to 10.170.177.85:58186-->10.170.177.85:10100
W1116 18:22:02.362740 13779 proto_async_client.h:493] EPOLLRDHUP received.
W1116 18:22:02.362841 13779 proto_async_client.h:505] Failed to recv msg: [ENIOError]
W1116 18:22:05.249965 13779 event_loop.cc:181] del non-existing event for fd 31


E1117 13:03:39.622771 78268 main.cc:83] [ZOO] handle_socket_error_msg:2473 2023-11-17 13:03:39,622:71098(0x7f1dbbcb2700):ZOO_ERROR@handle_socket_error_msg@2473: Socket [10.170.177.84:2181] zk retcode=-4, errno=111(Connection refused): server refused to accept the client
E1117 13:03:39.632719 78268 main.cc:83] [ZOO] handle_socket_error_msg:2498 2023-11-17 13:03:39,632:71098(0x7f1dbbcb2700):ZOO_ERROR@handle_socket_error_msg@2498: Socket [10.170.177.82:2181] zk retcode=-4, errno=112(Host is down): failed while receiving a server response
E1117 13:03:39.702683 78268 main.cc:83] [ZOO] handle_socket_error_msg:2498 2023-11-17 13:03:39,702:71098(0x7f1dbbcb2700):ZOO_ERROR@handle_socket_error_msg@2498: Socket [10.170.177.80:2181] zk retcode=-4, errno=112(Host is down): failed while receiving a server response
E1117 13:03:39.704126 78268 main.cc:83] [ZOO] handle_socket_error_msg:2498 2023-11-17 13:03:39,704:71098(0x7f1dbbcb2700):ZOO_ERROR@handle_socket_error_msg@2498: Socket [10.170.177.85:2181] zk retcode=-4, errno=112(Host is down): failed while receiving a server response
E1117 13:03:39.708768 78268 main.cc:83] [ZOO] handle_socket_error_msg:2498 2023-11-17 13:03:39,708:71098(0x7f1dbbcb2700):ZOO_ERROR@handle_socket_error_msg@2498: Socket [10.170.177.81:2181] zk retcode=-4, errno=112(Host is down): failed while receiving a server response
E1117 13:03:39.711922 78268 main.cc:83] [ZOO] handle_socket_error_msg:2473 2023-11-17 13:03:39,711:71098(0x7f1dbbcb2700):ZOO_ERROR@handle_socket_error_msg@2473: Socket [10.170.177.84:2181] zk retcode=-4, errno=111(Connection refused): server refused to accept the client

日志缺失

服务端也没有痕迹，客户端也没状态码


[smartx@vm-aa-600X20230905112008X1 17:15:43 ~]$ cat /etc/zbs/zbs.conf 
[network]
data_ip = 10.170.177.81
heartbeat_ip = 10.170.177.81
vm_ip = 192.168.79.167
web_ip = 192.168.79.167

[cluster]
role = master
members = 10.170.177.81,10.170.177.82,10.170.177.84,10.170.177.85
zookeeper = 10.170.177.81:2181,10.170.177.82:2181,10.170.177.84:2181,10.170.177.85:2181,10.170.177.80:2181
mongo = 10.170.177.81:27017,10.170.177.82:27017,10.170.177.84:27017,10.170.177.85:27017,10.170.177.80:27017
cluster_mgt_ips = 192.168.78.171,192.168.78.172,192.168.78.173,192.168.79.167,192.168.79.168,192.168.79.169
cluster_storage_ips = 10.170.177.84,10.170.177.85,10.170.177.86,10.170.177.81,10.170.177.82,10.170.177.83

[zone]
zone_ip_map = [{"zone_id": "8a6497e0-30ef-46b3-a700-5b9f88db2bad", "member_ips": ["10.170.177.83", "10.170.177.86"]}, {"zone_id": "default", "member_ips": ["10.170.177.81", "10.170.177.82", "10.170.177.84", "10.170.177.85"]}]


sudo zbs-tool  service list
Name     Members                                                                                                                                     Leader               Specified members                                                                               Priority    Current priority
-------  ------------------------------------------------------------------------------------------------------------------------------------------  -------------------  ----------------------------------------------------------------------------------------------  ----------  ------------------
chunk    ['10.170.177.84:10201', '10.170.177.86:10201', '10.170.177.83:10201', '10.170.177.82:10201', '10.170.177.85:10201', '10.170.177.81:10201']  None                 ['']
insight  []                                                                                                                                          None                 ['']
meta     ['10.170.177.82:10100', '10.170.177.81:10100', '10.170.177.85:10100', '10.170.177.84:10100']                                                10.170.177.84:10100  ['[10.170.177.81:10100', '10.170.177.82:10100', '10.170.177.84:10100', '10.170.177.85:10100]']
taskd    ['10.170.177.82:10601', '10.170.177.83:10601', '10.170.177.85:10601', '10.170.177.86:10601', '10.170.177.81:10601', '10.170.177.84:10601']  10.170.177.81:10601  ['']
[smartx@vm-aa-600X20230905112008X1 09:30:17 ~]$ tail -f /var/log/zbs/zbs-inspectord.INFO 
W1123 08:55:12.800443 93632 zookeeper.cc:190] Stop the session timeout timer.
I1123 08:55:12.800453 93632 zookeeper.cc:136] Done destructing zookeeper.
I1123 08:55:12.800480 93632 meta.cc:1900] [LIBMETA ADDR UPDATE]  old meta ip: 10.170.177.84 old meta port: 10100 new meta ip: 10.170.177.84 new meta port: 10100
W1123 08:55:12.800559 93632 event_loop.cc:181] del non-existing event for fd 30
W1123 08:55:12.800575 93632 event_loop.cc:189]  [SYSCALL]: ::epoll_ctl(efd_, EPOLL_CTL_DEL, fd, nullptr). [ARG LIST]: fd: 30 .[SYSMSG]: No such file or directory [2].
I1123 08:55:12.800637 93632 proto_async_client.h:180] Connect to 0.0.0.0:0-->10.170.177.84:10100 with timeout_ms 200
I1123 08:55:12.802786 93632 proto_async_client.h:366] Connected to 10.170.177.81:57942-->10.170.177.84:10100
E1123 08:55:12.819922 93632 inspector_server.cc:74]  List extent failed : 
Traceback:
[ENotReady]: 


```bash
#!/bin/bash

start_ip="10.170.177.81"
end_ip="10.170.177.86"

port=2181

current_ip=$start_ip

while [[ $(IFS=.; printf '%03d' $current_ip | tr '.' ' ') -le $(IFS=.; printf '%03d' $end_ip | tr '.' ' ') ]]
do
  timeout 3 telnet $current_ip $port > /dev/null 2>&1

  if [ $? -eq 0 ]; then
    echo "IP地址 $current_ip 的端口 $port 开放"
  else
    echo "IP地址 $current_ip 的端口 $port 未开放"
  fi

  current_ip=$(echo $current_ip | awk -F. '{print $1"."$2"."$3"."$4+1}')
done
```

```
timeout 1 telnet 10.170.177.81 2181
timeout 1 telnet 10.170.177.82 2181
timeout 1 telnet 10.170.177.83 2181
timeout 1 telnet 10.170.177.84 2181
timeout 1 telnet 10.170.177.85 2181
timeout 1 telnet 10.170.177.86 2181
```
timeout 
[smartx@vm-aa-600X20230905112008X6 10:34:51 ~]$ sudo zbs-tool zk list
10.170.177.81:2181          leader      0x445000018e5
10.170.177.82:2181        follower      0x445000018e5
10.170.177.84:2181        follower      0x445000018e5
10.170.177.85:2181      not reachable: [Errno 104] Connection reset by peer
10.170.177.80:2181        follower      0x445000018e9
[smartx@vm-aa-600X20230905112008X6 10:34:55 ~]$ sudo zbs-tool service list
Name     Members                                                                                                                                     Leader               Specified members                                                                               Priority    Current priority
-------  ------------------------------------------------------------------------------------------------------------------------------------------  -------------------  ----------------------------------------------------------------------------------------------  ----------  ------------------
chunk    ['10.170.177.82:10201', '10.170.177.81:10201', '10.170.177.83:10201', '10.170.177.86:10201', '10.170.177.84:10201', '10.170.177.85:10201']  None                 ['']
insight  []                                                                                                                                          None                 ['']
meta     ['10.170.177.84:10100', '10.170.177.82:10100', '10.170.177.81:10100', '10.170.177.85:10100']                                                10.170.177.85:10100  ['[10.170.177.81:10100', '10.170.177.82:10100', '10.170.177.84:10100', '10.170.177.85:10100]']
taskd    ['10.170.177.81:10601', '10.170.177.86:10601', '10.170.177.85:10601', '10.170.177.83:10601', '10.170.177.82:10601', '10.170.177.84:10601']  10.170.177.81:10601  ['']


insight 联系不上 meta 就无法工作

scvm_service_health{_service="zookeeper",state_str="[leader]"}


一个 zk 不work导致选举失败

```bash
I1124 03:18:11.914839 49247 data_insight_server.cc:264] data insight server start service done, is_leader: 0
I1124 03:18:12.628240 49247 data_insight_server.cc:264] data insight server start service done, is_leader: 1
I1124 03:19:12.553414 50269 data_insight_server.cc:264] data insight server start service done, is_leader: 0
I1124 03:22:11.825757 53473 data_insight_server.cc:264] data insight server start service done, is_leader: 0
I1124 03:22:43.618471 53473 data_insight_server.cc:264] data insight server start service done, is_leader: 1
I1124 03:27:04.563635 56898 data_insight_server.cc:264] data insight server start service done, is_leader: 0
I1124 03:27:34.338367 57342 data_insight_server.cc:264] data insight server start service done, is_leader: 1
I1124 03:28:07.718294 57547 data_insight_server.cc:264] data insight server start service done, is_leader: 0
I1124 03:29:21.482244 58621 data_insight_server.cc:264] data insight server start service done, is_leader: 0
I1124 03:30:31.182492 60150 data_insight_server.cc:264] data insight server start service done, is_leader: 0
I1124 03:31:15.743140 60981 data_insight_server.cc:264] data insight server start service done, is_leader: 1
```

curl -x PUT http://%s/api/v1/insight/ioreroute


```python
#! /usr/bin/python3

import sys
import logging
from urllib import request

def http_request(url, method, data="", content_type="application/json"):
    """
    send http request

    args:
        url: request url
        method: http method: "GET", "POST", etc
        data: json string
    """
    logger = logging.getLogger()
    data = str(data).replace("'", '"')
    req = request.Request(url, data=data.encode("utf-8"))
    if data:
        req.add_header("Content-Type", content_type)
    if ResourceConfig.bearer_token:
        req.add_header(AUTH_TOKEN_NAME, ResourceConfig.bearer_token)
    # overwrite the get method function
    req.get_method = lambda: method
    try:
        resp_data = []
        connection = request.urlopen(req)
        resp_code = connection.code
        for line in connection.readlines():
            resp_data.append(_bytes(line).decode("utf-8"))
        return resp_code, resp_data
    except Exception:
        err_type, err, err_trace = sys.exc_info()[:3]
        err_msg_list = traceback.format_exception(err_type, err, err_trace)
        logger.debug("call url: %s with error message: %s", url, "".join(err_msg_list))
        if err_type is HTTPError:
            http_error_code = err.code
            if http_error_code == 401:
                ResourceConfig.bearer_token = None
            return http_error_code, err_msg_list
        return -1, err_msg_list


logger = logging.getLogger()
target_ip = sys.argv[1]


hyper_ip_list = ["1.1.1.1"]
url = "http://%s/api/v1/insight/ioreroute" % target_ip
resp_code, resp = http_request(
    url,
    "PUT",
    "hyperip=%s&dataip=%s&manageip=%s&targetip=%s"
    % (
        ",".join(hyper_ip_list),
        "1.1.1.1",
        "1.1.1.1",
        target_ip,
    ),
    "application/x-www-form-urlencoded",
)

if resp_code == 200:
    print("200")
print(resp)
logger.warning("send heartbeat to %s failed", target_ip, )

```