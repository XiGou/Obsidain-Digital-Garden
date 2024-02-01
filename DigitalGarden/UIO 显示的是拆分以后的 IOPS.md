```bash
[root@xi-smtxos-5-1-0X20230926112710X1 16:29:52 ~]$ zbs-perf-tools chunk uio  summary
-------------------------------------------------
  host                 127.0.0.1:10200           
  total_iops           9.00                      
  total_bw             1.02 MB/s(1000.00 KiB/s)  
  total_latency        3.29 MS                   
  read_iops            9.00                      
  read_bw              1.02 MB/s(1000.00 KiB/s)  
  read_latency         3.29 MS                   
  write_iops           0.00                      
  write_bw             0.00 B/s(0.00 B/s)        
  write_latency        0.00 NS                   
  local_io_ratio       1.00 (9.00 / 9.00)        
  local_io_latency     3.29 MS                   
  local_io_bw          1.02 MB/s(1000.00 KiB/s)  
  local_read_ratio     1.00 (9.00 / 9.00)        
  local_read_latency   3.29 MS                   
  local_read_bw        1.02 MB/s(1000.00 KiB/s)  
  local_write_ratio    0.00                      
  local_write_latency  0.00 NS                   
  local_write_bw       0.00 B/s(0.00 B/s)        
  failed_io_ratio      0.00                      
  retry_io_ratio       0.00                      
  retry_queue_size     0                         
  active_extents       5                         
  active_volumes       2                         
  waiting_queue_size   0                         
-------------------------------------------------
[root@xi-smtxos-5-1-0X20230926112710X1 16:29:53 ~]$ zbs-perf-tools volume show a5d3261d-c1e5-4756-8dbf-ec85d0f928d6
Aggregated Data:
---------------------------------------------------------------------
  volume_id                    a5d3261d-c1e5-4756-8dbf-ec85d0f928d6  
  read_iops                    5.00                                  
  read_avgrq                   204.80 KB(200.00 KiB)                 
  read_bw                      1.02 MB/s(1000.00 KiB/s)              
  read_latency                 3.01 MS                               
  splited_read_iops            8.00                                  
  splited_read_latency         3.06 MS                               
  splited_local_read_ratio     1.00 (8.00 / 8.00)                    
  splited_local_read_bw        1.02 MB/s(1000.00 KiB/s)              
  splited_local_read_latency   3.06 MS                               
  write_iops                   0.00                                  
  write_avgrq                  0.00 B(0.00 B)                        
  write_bw                     0.00 B/s(0.00 B/s)                    
  write_latency                0.00 NS                               
  splited_write_iops           0.00                                  
  splited_write_latency        0.00 NS                               
  splited_local_write_ratio    0.00                                  
  splited_local_write_bw       0.00 B/s(0.00 B/s)                    
  splited_local_write_latency  0.00 NS                               
  total_iops                   5.00                                  
  total_avgrq                  204.80 KB(200.00 KiB)                 
  total_bw                     1.02 MB/s(1000.00 KiB/s)              
  total_latency                3.01 MS                               
  total_iop30s                 0.00                                  
---------------------------------------------------------------------
Chunk-Specific Data:
----------------------------------------------------------------------------------------------
  CHUNK IP        TOTAL IOPS  TOTAL AVGRQ            TOTAL BW                  TOTAL LATENCY  
----------------------------------------------------------------------------------------------
  10.168.100.111  0.00        0.00 B(0.00 B)         0.00 B/s(0.00 B/s)        0.00 NS        
  10.168.100.110  0.00        0.00 B(0.00 B)         0.00 B/s(0.00 B/s)        0.00 NS        
  10.168.100.109  5.00        204.80 KB(200.00 KiB)  1.02 MB/s(1000.00 KiB/s)  3.01 MS        
----------------------------------------------------------------------------------------------
```


```c++
volume_ctx->volume_stats.ScheduleCalc(

volume_ctx->io_stats, new CommonClosure(

[volume_ctx]() {

/* hold a volume_ctx reference count here to avoid to be

* deleted, and running in zbs client main thread.*/

volume_ctx->volume_perf = volume_ctx->volume_stats.GetPerf();

volume_ctx->PerfToMetrics();

},
```
从 GetPerf 得到的

UpdateUIOCounter
UpdateIOCounter

 ninja && src/zbs_test --gtest_filter="*LibZBSTest.ReadWriteTest*"
 sudo   zbs-perf-tools chunk uio  summary  &&  sudo  zbs-perf-tools chunk access summary
  sudo fio --name=read --ioengine=libaio --rw=read --bs=500k --numjobs=1 --size=0 --time_based=1 --runtime=-1 --rate=10000k --filename=/dev/sd

Note: Google Test filter = *LibZBSPerfTest.ReadWritePerfTest*
[==========] Running 1 test from 1 test suite.
[----------] Global test environment set-up.
[----------] 1 test from LibZBSPerfTest
[ RUN      ] LibZBSPerfTest.ReadWritePerfTest
gouxi 5 524288
gouxi 5 524288
gouxi1 1 524288
gouxi 4 524288
gouxi 4 524288

可能和replica有关系

gouxi2 0 524288 0x562601694000
gouxi 5 524288
gouxi2 0 524288 0x5625fe117400 

的确

[----------] 1 test from LibZBSPerfTest
[ RUN      ] LibZBSPerfTest.ReadWritePerfTest
gouxi3 1 524288
gouxi4 1 524288
gouxi5 1 524288
gouxi2 0 524288 0x55ade3264000
gouxi 5 524288
gouxi2 0 524288 0x55addfce4000
gouxi 5 524288
UpdateUIOCounter 1 0x55addfce4000
UpdateUIOCounter 1 0x55ade3264000
gouxi 4 524288
gouxi 4 524288
UpdateUIOCounter 1 0x55addfce4000
UpdateUIOCounter 1 0x55ade3264000


zbs::libzbs::ZbsClient::UIOCtxDone

[Switching to Thread 0x7fff8a9e2700 (LWP 179439)]

Thread 84 "zbs-client-prox" hit Breakpoint 2, zbs::libzbs::ZbsClient::Write(unsigned __int128, char const*, unsigned int, unsigned long, zbs::IOController*, std::function<void ()> const&, zbs::IOBufOwner*) (this=0x5555a6db8000, volume_id=0xcfc535c6647e8895ee4bcbd382eb157a, buf=0x7fffffefddc0 "", len=524288, 
    offset=0, ctrl=0x7fffffefd660, req_done=..., wbuf_owner=0x0) at ../src/libzbs/zbs_client.cc:496
496                           const Callable& req_done, IOBufOwner* wbuf_owner) {
(gdb) c
Continuing.
E1024 15:01:35.238667 179391 main.cc:83] [ZOO] handle_socket_error_msg:2393 2023-10-24 15:01:35,238:179356(0x7fff9ea0a700):ZOO_ERROR@handle_socket_error_msg@2393: Socket [127.0.0.1:2181] zk retcode=-7, errno=110(Connection timed out): connection to 127.0.0.1:2181 timed out (exceeded timeout by 1416ms)
E1024 15:01:35.238811 179410 main.cc:83] [ZOO] handle_socket_error_msg:2393 2023-10-24 15:01:35,238:179356(0x7fff969fa700):ZOO_ERROR@handle_socket_error_msg@2393: Socket [127.0.0.1:2181] zk retcode=-7, errno=110(Connection timed out): connection to 127.0.0.1:2181 timed out (exceeded timeout by 1221ms)
gouxi2 0 524288 0x5555a6db8000
gouxi 5 524288
E1024 15:01:35.239113 179429 main.cc:83] [ZOO] handle_socket_error_msg:2393 2023-10-24 15:01:35,239:179356(0x7fff8e1e9700):ZOO_ERROR@handle_socket_error_msg@2393: Socket [127.0.0.1:2181] zk retcode=-7, errno=110(Connection timed out): connection to 127.0.0.1:2181 timed out (exceeded timeout by 1151ms)
[Switching to Thread 0x7fff9b203700 (LWP 179401)]

Thread 46 "zbs-chunkd1" hit Breakpoint 2, zbs::libzbs::ZbsClient::Write(unsigned __int128, char const*, unsigned int, unsigned long, zbs::IOController*, std::function<void ()> const&, zbs::IOBufOwner*) (this=0x5555a37de000, volume_id=0xcfc535c6647e8895ee4bcbd382eb157a, buf=0x7fffdf380000 "", len=524288, 
    offset=0, ctrl=0x5555a7c06090, req_done=..., wbuf_owner=0x0) at ../src/libzbs/zbs_client.cc:496
496                           const Callable& req_done, IOBufOwner* wbuf_owner) {


2 倍数关系可能是 client-proxy 和 chunk 多次调用 Write

void ZbsClient::UpdateIOCounter(IOCtx* ioctx, bool succeed) {多次数调用

似乎无问题


本来就是统计的 BIO，估计是 By Design，尚未敲定修改与否
