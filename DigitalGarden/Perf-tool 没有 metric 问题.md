
```bash  grep total_iops
sudo zbs-perf-tools --chunk-addr  192.168.24.6:10200 chunk access summary
sudo zbs-perf-tools --chunk-addr  192.168.31.241:10200 chunk access summary
sudo zbs-perf-tools --chunk-addr  192.168.27.195:10200 chunk access summary
```

`./perf.sh | grep total_iops`


```bash
  total_iops              1491.00                
  local_total_iops        0.00                   
  total_iops              0.00                  
  local_total_iops        0.00                  
  total_iops              0.00                   
  local_total_iops        715.00 
```
现象： 有 local，没total