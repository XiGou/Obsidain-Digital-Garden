sudo ./fio/fio --direct=1 --ioengine=external:/home/smartx/fio/fio-zbs-e^Co   --iodepth=1 --thread=1 --group_reporting=1 --time_based=1 --runtime=10000  --zk_hosts='127.0.0.1:218' --pool=test --volume=test_volume --name=write --numjobs=1 --iodepth=64 --bsrange=512-64k --rw=randwrite --size=100M 


sudo fio --direct=0 --ioengine=libaio   --iodepth=1 --thread=1 --group_reporting=1 --time_based=1 --runtime=15  --name=write --numjobs=1 --iodepth=64 --bsrange=512-64k --rw=randwrite --size=50M --filename=/dev/disk/by-path/ip-127.0.0.1\:3260-iscsi-iqn.2016-02.com.smartx\:system\:smartx-benchmark-test-lun-1 

```bash
]$ /usr/local/venv/zbs-client-py/bin/python zbs_volume_benchmark.py 
lun 1 in smartx-benchmark-test already existed, delete it
127.0.0.1:3260,1 iqn.2016-02.com.smartx:system:smartx-benchmark-test
127.0.0.1:3260,1 iqn.2016-02.com.smartx:system:test
Rescanning session [sid: 1, target: iqn.2016-02.com.smartx:system:smartx-benchmark-test, portal: 127.0.0.1,3261]
========== 4k   rw ==========
Read IOPS: 16324.711686 Read BW: 65298 Read LAT: 1.930912529657
Write IOPS: 16334.044397 Write BW: 65336 Write LAT: 1.916978213699
========== 4k   randrw ==========
Read IOPS: 55.592455 Read BW: 222 Read LAT: 446.99683182795695
Write IOPS: 58.846971 Write BW: 235 Write LAT: 457.55455867268597
========== 256k   rw ==========
Read IOPS: 510.095507 Read BW: 130584 Read LAT: 59.922669205328006
Write IOPS: 512.274854 Write BW: 131142 Write LAT: 58.142333814565006
========== 256k   randrw ==========
Read IOPS: 34.189171 Read BW: 8752 Read LAT: 687.9495502256809
Write IOPS: 36.517228 Write BW: 9348 Write LAT: 597.252762063752
```
优先级低