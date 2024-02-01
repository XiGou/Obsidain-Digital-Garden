---
dg-publish: true
---


This is a useful `iscsiadm` command cheat-sheet：

**Target（Discovery）**

| 命令                                             | 描述                     |
| ---------------------------------------------- | ---------------------- |
| `iscsiadm -m discovery -t st -p <IP>`          | 发现指定 IP 地址的 iSCSI 目标   |
| `iscsiadm -m discovery -t sendtargets`         | 发现所有可用的 iSCSI 目标       |
| `iscsiadm -m discovery -t sendtargets -p <IP>` | 发现指定 IP 地址的所有 iSCSI 目标 |

**（Login/Logout）**

| 命令                                             | 描述                   |
| ---------------------------------------------- | -------------------- |
| `iscsiadm -m node -T <target_name> -p <IP>`    | 登录指定 IP 地址的 iSCSI 目标 |
| `iscsiadm -m node -T <target_name> -p <IP> -u` | 登出指定 IP 地址的 iSCSI 目标 |
| `iscsiadm -m node --logoutall=all`             | 登出所有已登录的 iSCSI 目标    |

**Session Managemen**

| 命令                                          | 描述                    |
| ------------------------------------------- | --------------------- |
| `iscsiadm -m session`                       | 显示当前活动的 iSCSI 会话信息    |
| `iscsiadm -m session -r <session_id>`       | 移除指定会话 ID 的 iSCSI 会话  |
| `iscsiadm -m session --rescan`              | 重新扫描所有 iSCSI 会话       |
| `iscsiadm -m session --rescan-target=<IQN>` | 重新扫描指定 IQN 的 iSCSI 会话 |

**Target Management**

| 命令                                                      | 描述                  |
| ------------------------------------------------------- | ------------------- |
| `iscsiadm -m node`                                      | 显示当前已配置的 iSCSI 目标信息 |
| `iscsiadm -m node -T <target_name> -p <IP> --op=new`    | 添加新的 iSCSI 目标       |
| `iscsiadm -m node -T <target_name> -p <IP> --op=delete` | 删除指定 iSCSI 目标       |
| `iscsiadm -m node -T <target_name> -p <IP> --op=update` | 更新指定 iSCSI 目标的属性    |

**Other Usual Commands  **

| 命令                                  | 描述                          |
| ------------------------------------- |:----------------------------- |
| `iscsiadm -m iface`                   | 显示当前的网络接口信息        |
| `iscsiadm -m fw`                      | 显示 iSCSI 适配器固件信息     |
| `iscsiadm -m fw --op=update`          | 更新 iSCSI 适配器固件         |
| `iscsiadm -m sessionstats`            | 显示 iSCSI 会话统计信息       |
| `iscsiadm -m statistics`              | 显示 iSCSI 统计信息           |
| `iscsiadm -m node -p <IP> -l`         | 使用 CHAP 认证登录 iSCSI 目标 |
| `iscsiadm -m node -p <IP> -I <iface>` | 指定网络接口登录 iSCSI 目标   |
| `iscsiadm -m session -P 3  `          | 每个lun的本地路径             |
