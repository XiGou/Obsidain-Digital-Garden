[root@e9af7f162d7f zbs]# uname -a
Linux e9af7f162d7f 5.10.124-linuxkit #1 SMP PREEMPT Thu Jun 30 08:18:26 UTC 2022 aarch64 aarch64 aarch64 GNU/Linux
[root@e9af7f162d7f zbs]# rpm -qa | grep perf-too
zbs-perf-tools-1.0.1-rc6.0.gfec3b72.el7.centos.aarch64


```
[root@a3207ece510a /]# yum install /zbs/build/rpm/aarch64/zbs-perf-tools-1.0.1-rc6.0.gfec3b72.oe1.aarch64.rpm
smartx iso common                                                                                                                                                  3.9 MB/s | 2.7 MB     00:00
smartx iso common                                                                                                                                                  1.1 MB/s | 415 kB     00:00
smartxos base                                                                                                                                                      6.2 MB/s |  38 MB     00:06
smartxos base                                                                                                                                                       40 kB/s | 7.2 kB     00:00
smartxos base zbs                                                                                                                                                  2.5 MB/s | 2.5 MB     00:00
smartxzbs base zbs                                                                                                                                                 2.8 MB/s | 2.5 MB     00:00
smartxos oem hygon                                                                                                                                                 645  B/s | 257  B     00:00
smartxzbs oem hygon                                                                                                                                                1.3 kB/s | 257  B     00:00
smartxos oem ali                                                                                                                                                   1.2 kB/s | 257  B     00:00
smartxos oem ali hygon                                                                                                                                             1.2 kB/s | 257  B     00:00
smartxzbs oem ali                                                                                                                                                  1.2 kB/s | 257  B     00:00
smartxzbs oem ali hygon                                                                                                                                            1.1 kB/s | 257  B     00:00
CentOS-20.03LTS_SP3 - Base                                                                                                                                         1.4 kB/s | 257  B     00:00
smartxos kernel and driver                                                                                                                                         7.8 MB/s |  20 MB     00:02
smartx iso common                                                                                                                                                  1.6 MB/s | 764 kB     00:00
Dependencies resolved.
===================================================================================================================================================================================================
 Package                                            Architecture                          Version                                                Repository                                   Size
===================================================================================================================================================================================================
Installing:
 zbs-perf-tools                                     aarch64                               1.0.1-rc6.0.gfec3b72.oe1                               @commandline                                3.5 M
Upgrading:
 libbabeltrace2                                     aarch64                               2.0.4-4                                                smartxos-base                               540 k
 libbabeltrace2-devel                               aarch64                               2.0.4-4                                                smartxos-base                               138 k
Installing dependencies:
 babeltrace2                                        aarch64                               2.0.4-4                                                smartxos-base                               173 k
 lttng-tools                                        aarch64                               2.10.0-2.oe1                                           smartxos-base                               691 k

Transaction Summary
===================================================================================================================================================================================================
Install  3 Packages
Upgrade  2 Packages

Total size: 5.0 M
Total download size: 1.5 M
Is this ok [y/N]: y
Downloading Packages:
(1/4): babeltrace2-2.0.4-4.aarch64.rpm                                                                                                                             714 kB/s | 173 kB     00:00
(2/4): libbabeltrace2-devel-2.0.4-4.aarch64.rpm                                                                                                                    2.8 MB/s | 138 kB     00:00
(3/4): libbabeltrace2-2.0.4-4.aarch64.rpm                                                                                                                          1.0 MB/s | 540 kB     00:00
(4/4): lttng-tools-2.10.0-2.oe1.aarch64.rpm                                                                                                                        1.0 MB/s | 691 kB     00:00
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                                              2.2 MB/s | 1.5 MB     00:00
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                                           1/1
  Upgrading        : libbabeltrace2-2.0.4-4.aarch64                                                                                                                                            1/7
  Installing       : babeltrace2-2.0.4-4.aarch64                                                                                                                                               2/7
  Running scriptlet: lttng-tools-2.10.0-2.oe1.aarch64                                                                                                                                          3/7
  Installing       : lttng-tools-2.10.0-2.oe1.aarch64                                                                                                                                          3/7
  Running scriptlet: lttng-tools-2.10.0-2.oe1.aarch64                                                                                                                                          3/7
Created symlink /etc/systemd/system/multi-user.target.wants/lttng-sessiond.service → /usr/lib/systemd/system/lttng-sessiond.service.

  Installing       : zbs-perf-tools-1.0.1-rc6.0.gfec3b72.oe1.aarch64                                                                                                                           4/7
  Upgrading        : libbabeltrace2-devel-2.0.4-4.aarch64                                                                                                                                      5/7
  Cleanup          : libbabeltrace2-devel-2.0.4-3.oe1.aarch64                                                                                                                                  6/7
  Cleanup          : libbabeltrace2-2.0.4-3.oe1.aarch64                                                                                                                                        7/7
  Running scriptlet: libbabeltrace2-2.0.4-3.oe1.aarch64                                                                                                                                        7/7
  Verifying        : babeltrace2-2.0.4-4.aarch64                                                                                                                                               1/7
  Verifying        : lttng-tools-2.10.0-2.oe1.aarch64                                                                                                                                          2/7
  Verifying        : zbs-perf-tools-1.0.1-rc6.0.gfec3b72.oe1.aarch64                                                                                                                           3/7
  Verifying        : libbabeltrace2-2.0.4-4.aarch64                                                                                                                                            4/7
  Verifying        : libbabeltrace2-2.0.4-3.oe1.aarch64                                                                                                                                        5/7
  Verifying        : libbabeltrace2-devel-2.0.4-4.aarch64                                                                                                                                      6/7
  Verifying        : libbabeltrace2-devel-2.0.4-3.oe1.aarch64                                                                                                                                  7/7

Upgraded:
  libbabeltrace2-2.0.4-4.aarch64                                                                libbabeltrace2-devel-2.0.4-4.aarch64

Installed:
  babeltrace2-2.0.4-4.aarch64                             lttng-tools-2.10.0-2.oe1.aarch64                             zbs-perf-tools-1.0.1-rc6.0.gfec3b72.oe1.aarch64

Complete!
[root@a3207ece510a /]# cat /etc/os-release
NAME="openEuler"
VERSION="20.03 (LTS-SP3)"
ID="openEuler"
VERSION_ID="20.03"
PRETTY_NAME="openEuler 20.03 (LTS-SP3)"
ANSI_COLOR="0;31"

[root@a3207ece510a /]# uname -a
Linux a3207ece510a 5.10.124-linuxkit #1 SMP PREEMPT Thu Jun 30 08:18:26 UTC 2022 aarch64 aarch64 aarch64 GNU/Linux


```