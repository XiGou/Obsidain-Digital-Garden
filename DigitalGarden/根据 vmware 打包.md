```bash
[smartx@gouxi-505-el7-x86-64X20230824172350X2 13:01:02 ~]$ lddtree vmware-vix-disklib-distrib/lib64/libvixDiskLib.so.8
libvixDiskLib.so.8 => vmware-vix-disklib-distrib/lib64/libvixDiskLib.so.8 (interpreter => none)
    librt.so.1 => /lib64/librt.so.1
    libcurl.so.4 => /lib64/libcurl.so.4
        libidn.so.11 => /lib64/libidn.so.11
        libssh2.so.1 => /lib64/libssh2.so.1
            libssl.so.10 => /lib64/libssl.so.10
            libcrypto.so.10 => /lib64/libcrypto.so.10
        libssl3.so => /lib64/libssl3.so
        libsmime3.so => /lib64/libsmime3.so
        libnss3.so => /lib64/libnss3.so
        libnssutil3.so => /lib64/libnssutil3.so
        libplds4.so => /lib64/libplds4.so
        libplc4.so => /lib64/libplc4.so
        libnspr4.so => /lib64/libnspr4.so
        libgssapi_krb5.so.2 => /lib64/libgssapi_krb5.so.2
            libkrb5support.so.0 => /lib64/libkrb5support.so.0
                libselinux.so.1 => /lib64/libselinux.so.1
                    libpcre.so.1 => /lib64/libpcre.so.1
            libkeyutils.so.1 => /lib64/libkeyutils.so.1
            libresolv.so.2 => /lib64/libresolv.so.2
        libkrb5.so.3 => /lib64/libkrb5.so.3
        libk5crypto.so.3 => /lib64/libk5crypto.so.3
        libcom_err.so.2 => /lib64/libcom_err.so.2
        liblber-2.4.so.2 => /lib64/liblber-2.4.so.2
        libldap-2.4.so.2 => /lib64/libldap-2.4.so.2
            libsasl2.so.3 => /lib64/libsasl2.so.3
                libcrypt.so.1 => /lib64/libcrypt.so.1
                    libfreebl3.so => /lib64/libfreebl3.so
    libdl.so.2 => /lib64/libdl.so.2
    libpthread.so.0 => /lib64/libpthread.so.0
    libz.so.1 => /lib64/libz.so.1
    libsqlite3.so.0 => /lib64/libsqlite3.so.0
    libgcc_s.so.1 => /lib64/libgcc_s.so.1
    libc.so.6 => /lib64/libc.so.6
    ld-linux-x86-64.so.2 => /lib64/ld-linux-x86-64.so.2
```

```bash
$ ll vmware-vix-disklib-distrib/lib64/
total 67280
-rwxr-xr-x 1 smartx smartx  1671936 Apr  6 15:50 fips.so
lrwxrwxrwx 1 smartx smartx       13 Apr  6 15:54 libcares.so -> libcares.so.2
-rwxr-xr-x 1 smartx smartx    82416 Apr  6 15:50 libcares.so.2
lrwxrwxrwx 1 smartx smartx       14 Apr  6 15:54 libcrypto.so -> libcrypto.so.3
-rwxr-xr-x 1 smartx smartx  4763048 Apr  6 15:50 libcrypto.so.3
lrwxrwxrwx 1 smartx smartx       12 Apr  6 15:54 libcurl.so -> libcurl.so.4
-rwxr-xr-x 1 smartx smartx   557496 Apr  6 15:50 libcurl.so.4
-rwxr-xr-x 1 smartx smartx  3602816 Apr  6 15:54 libdiskLibPlugin.so
-rwxr-xr-x 1 smartx smartx   186160 Apr  6 15:50 libexpat.so
lrwxrwxrwx 1 smartx smartx       13 Apr  6 15:54 libgcc_s.so -> libgcc_s.so.1
-rw-r--r-- 1 smartx smartx   100944 Apr  6 15:50 libgcc_s.so.1
-rwxr-xr-x 1 smartx smartx   325904 Apr  6 15:50 liblookup-types.so
lrwxrwxrwx 1 smartx smartx       15 Apr  6 15:54 libsqlite3.so -> libsqlite3.so.0
lrwxrwxrwx 1 smartx smartx       19 Apr  6 15:54 libsqlite3.so.0 -> libsqlite3.so.0.8.6
-rwxr-xr-x 1 smartx smartx   999328 Apr  6 15:50 libsqlite3.so.0.8.6
lrwxrwxrwx 1 smartx smartx       11 Apr  6 15:54 libssl.so -> libssl.so.3
-rwxr-xr-x 1 smartx smartx   692344 Apr  6 15:50 libssl.so.3
-rwxr-xr-x 1 smartx smartx   452928 Apr  6 15:50 libssoclient.so
lrwxrwxrwx 1 smartx smartx       14 Apr  6 15:54 libstdc++.so -> libstdc++.so.6
-rwxr-xr-x 1 smartx smartx  1956512 Apr  6 15:50 libstdc++.so.6
lrwxrwxrwx 1 smartx smartx       21 Apr  6 15:54 libvddkVimAccess.so -> libvddkVimAccess.so.8
lrwxrwxrwx 1 smartx smartx       25 Apr  6 15:54 libvddkVimAccess.so.8 -> libvddkVimAccess.so.8.0.1
-rwxrwxr-x 1 smartx smartx  2746760 Apr  6 15:54 libvddkVimAccess.so.8.0.1
-rwxr-xr-x 1 smartx smartx 33622296 Apr  6 15:54 libvim-types.so
lrwxrwxrwx 1 smartx smartx       18 Apr  6 15:54 libvixDiskLib.so -> libvixDiskLib.so.8
lrwxrwxrwx 1 smartx smartx       22 Apr  6 15:54 libvixDiskLib.so.8 -> libvixDiskLib.so.8.0.1
-rwxrwxr-x 1 smartx smartx  2912312 Apr  6 15:50 libvixDiskLib.so.8.0.1
lrwxrwxrwx 1 smartx smartx       17 Apr  6 15:54 libvixMntapi.so -> libvixMntapi.so.1
lrwxrwxrwx 1 smartx smartx       21 Apr  6 15:54 libvixMntapi.so.1 -> libvixMntapi.so.1.1.0
-rwxrwxr-x 1 smartx smartx  3257088 Apr  6 15:51 libvixMntapi.so.1.1.0
-rwxr-xr-x 1 smartx smartx  7046648 Apr  6 15:50 libvmacore.so
-rwxr-xr-x 1 smartx smartx  3755088 Apr  6 15:50 libvmomi.so
lrwxrwxrwx 1 smartx smartx        9 Apr  6 15:54 libz.so -> libz.so.1
-rwxr-xr-x 1 smartx smartx   116872 Apr  6 15:50 libz.so.1


```

```bash
$ lddtree /lib64/libzbs.so.1
libzbs.so.1 => /lib64/libzbs.so.1 (interpreter => none)
    liblttng-ust.so.0 => /lib64/liblttng-ust.so.0
        liblttng-ust-tracepoint.so.0 => /lib64/liblttng-ust-tracepoint.so.0
        liburcu-bp.so.2 => /lib64/liburcu-bp.so.2
        liburcu-cds.so.2 => /lib64/liburcu-cds.so.2
    libpthread.so.0 => /lib64/libpthread.so.0
    libdl.so.2 => /lib64/libdl.so.2
    libsystemd.so.0 => /lib64/libsystemd.so.0
        libcap.so.2 => /lib64/libcap.so.2
            libattr.so.1 => /lib64/libattr.so.1
        libselinux.so.1 => /lib64/libselinux.so.1
        liblzma.so.5 => /lib64/liblzma.so.5
        libgcrypt.so.11 => /lib64/libgcrypt.so.11
        libgpg-error.so.0 => /lib64/libgpg-error.so.0
        libresolv.so.2 => /lib64/libresolv.so.2
        libdw.so.1 => /lib64/libdw.so.1
            libelf.so.1 => /lib64/libelf.so.1
            libz.so.1 => /lib64/libz.so.1
            libbz2.so.1 => /lib64/libbz2.so.1
    libprotobuf.so.8 => /lib64/libprotobuf.so.8
    librdmacm.so.1 => /lib64/librdmacm.so.1
        libnl-3.so.200 => /lib64/libnl-3.so.200
    libibverbs.so.1 => /lib64/libibverbs.so.1
        libnl-route-3.so.200 => /lib64/libnl-route-3.so.200
    libcgroup.so.1 => /lib64/libcgroup.so.1
    libsysfs.so.2 => /lib64/libsysfs.so.2
    liburcu.so.2 => /lib64/liburcu.so.2
    liburcu-common.so.2 => /lib64/liburcu-common.so.2
    libzstd.so.1 => /lib64/libzstd.so.1
    libcrypto.so.10 => /lib64/libcrypto.so.10
    libblkid.so.1 => /lib64/libblkid.so.1
    libuuid.so.1 => /lib64/libuuid.so.1
    librt.so.1 => /lib64/librt.so.1
    libfdisk.so.1 => /lib64/libfdisk.so.1
    librsync.so.2 => /lib64/librsync.so.2
    libunwind.so.8 => /lib64/libunwind.so.8
    libpcre.so.1 => /lib64/libpcre.so.1
    libmicrohttpd.so.10 => /lib64/libmicrohttpd.so.10
        libgnutls.so.28 => /lib64/libgnutls.so.28
            libp11-kit.so.0 => /lib64/libp11-kit.so.0
                libffi.so.6 => /lib64/libffi.so.6
            libtasn1.so.6 => /lib64/libtasn1.so.6
            libnettle.so.4 => /lib64/libnettle.so.4
            libhogweed.so.2 => /lib64/libhogweed.so.2
            libgmp.so.10 => /lib64/libgmp.so.10
    libsnappy.so.1 => /lib64/libsnappy.so.1
    libstdc++.so.6 => /lib64/libstdc++.so.6
    libm.so.6 => /lib64/libm.so.6
    libgcc_s.so.1 => /lib64/libgcc_s.so.1
    libc.so.6 => /lib64/libc.so.6
    ld-linux-x86-64.so.2 => /lib64/ld-linux-x86-64.so.2

```

```bash
#!/bin/bash

# 动态链接库路径
dynamic_library_path="/path/to/dynamic_library.so"

# 临时文件夹路径
temporary_folder_path="./temp_folder"

# 打包文件名称
archive_file_name="dependencies"

# 创建临时文件夹
mkdir -p "$temporary_folder_path"

# 使用 lddtree 获取依赖关系并复制依赖项到临时文件夹
lddtree "$dynamic_library_path" | awk '{print $3}' | xargs -I '{}' cp '{}' "$temporary_folder_path"

# 创建 tar.gz 文件
tar -czvf "$archive_file_name.tar.gz" -C "$temporary_folder_path" .

# 清理临时文件夹
rm -rf "$temporary_folder_path"

echo "打包完成！"


```




```bash
[root@0c0067d204a7 build]# ldd /lib64/liblttng-ust.so.0
[root@2d7457677920 zbs]# ldd build/src/libzbs.so | grep not\ found
        liblttng-ust.so.0 => not found
        libprotobuf.so.8 => not found
        librdmacm.so.1 => not found
        libibverbs.so.1 => not found
        libcgroup.so.1 => not found
        liburcu.so.2 => not found
        liburcu-common.so.2 => not found
        libzstd.so.1 => not found
        libfdisk.so.1 => not found
        librsync.so.2 => not found
        libunwind.so.8 => not found
        libmicrohttpd.so.10 => not found
        libsnappy.so.1 => not found
```

```bash
Installed:
  glibc-static.x86_64 0:2.17-326.el7_9                                                                                                                                                                     

Complete!
[root@0c0067d204a7 build]# rpm -ql glibc-static | grep libc.a
/usr/lib64/libc.a
[root@0c0067d204a7 build]# ll -h /usr/lib64/libc.a
-rw-r--r-- 1 root root 4.9M May 18  2022 /usr/lib64/libc.a
```

```cmake
set(libcommon_LIBRARIES cgroup sysfs urcu urcu-cds urcu-common zstd

ssl crypto blkid uuid rt pthread fdisk rsync unwind pcre microhttpd aio

${glog_LIBRARY} ${gflags_LIBRARY} leveldb absl::flat_hash_map

absl::base absl::memory absl::str_format)
```



```bash
[root@gouxi-505-el7-x86-64X20230824172350X2 09:44:25 smartx]$ cat vmware-vix-disklib-distrib/doc/samples/diskLib/Makefile 
INCLUDEDIR=../../../include
LIBDIR=../../../lib64
LIBS=-ldl -lz -lsqlite3 -lcurl -lssl -lcrypto -lcares

ifdef VIX_AIO_BUFPOOL_SIZE
CXXFLAGS+= -DVIX_AIO_BUFPOOL_SIZE=$(VIX_AIO_BUFPOOL_SIZE)
endif

CXXFLAGS+= -std=c++1y -lpthread

all: vix-disklib-sample vix-mntapi-sample

vix-disklib-sample: vixDiskLibSample.cpp
        $(CXX) $(CXXFLAGS) -o $@ -I$(INCLUDEDIR) -L$(LIBDIR) $? $(LIBS) -lvixDiskLib

vix-mntapi-sample:  vixDiskLibSample.cpp
        $(CXX) $(CXXFLAGS) -o $@ -DFOR_MNTAPI -I$(INCLUDEDIR) -L$(LIBDIR) $? $(LIBS) \
           -lfuse -lvixDiskLib -lvixMntapi

clean:
        $(RM) -f vix-disklib-sample vix-mntapi-sample

```


```bash
[root@2d7457677920 examples]# ldd ../lib64/libzbs.so | grep "not found"
        libprotobuf.so.8 => not found
        librdmacm.so.1 => not found
        libibverbs.so.1 => not found
        liburcu-common.so.2 => not found
        libzstd.so.1 => not found
        libfdisk.so.1 => not found
        librsync.so.2 => not found
        libunwind.so.8 => not found
        libmicrohttpd.so.10 => not found
        libsnappy.so.1 => not found
```


```bash
ldd "$dynamic_library_path" | awk '{print $3}' | xargs -I '{}' cp -vd '{}' "$temporary_folder_path/lib64/"

ldd "$dynamic_library_path" | awk '{print $3}' | xargs -I '{}' cp -v '{}' "$temporary_folder_path/lib64/"
```
## 以tar.gz 格式发布c库
 LD_LIBRARY_PATH=../lib64  g++ -std=c++11 ./src/snapshot_incremental.cc -o bin/zbs_example_snapshot_incremental  -I../include -L../lib64    -lzbs -lprotobuf -lzstd -lsnappy -lurcu-common -lfdisk -lmicrohttpd -lunwind -g

如果在运行程序时 -L 选项无效，但设置 LD_LIBRARY_PATH 环境变量可以使程序链接上动态库，可能有以下几个原因：

优先级问题：LD_LIBRARY_PATH 环境变量的优先级高于 -L 选项。当存在 LD_LIBRARY_PATH 环境变量时，系统会首先搜索该路径下的动态库，而不会再去关注 -L 选项指定的路径。

动态库路径不正确：可能 -L 选项指定的路径不正确，导致系统无法找到动态库。请确保通过 -L 选项指定的路径正确无误，包括路径是否存在以及是否包含目标动态库文件。

动态库命名不正确：动态库的命名也可能影响链接的结果。请确保动态库的命名符合系统的命名规范，并符合所链接程序的期望。

系统配置问题：有些系统可能会限制对指定路径的动态库访问权限，这可能导致 -L 选项无效。在这种情况下，设置 LD_LIBRARY_PATH 环境变量可以绕过权限限制，但请注意这可能会引入其他安全风险。

为了解决链接动态库的问题，建议按照以下步骤进行：

确保动态库文件存在于正确的路径中，并且命名正确。

使用 -L 选项指定动态库的路径，并确保路径正确无误。

检查系统的权限限制，确保对指定路径的动态库拥有足够的访问权限。

如果 -L 选项无效，可以尝试设置 LD_LIBRARY_PATH 环境变量，指定包含动态库的路径。

请注意，使用 LD_LIBRARY_PATH 环境变量可能会引入其他潜在问题，如版本冲突或安全风险。因此，最好在确保路径和命名正确的情况下，通过 -L 选项来链接动态库，以便更好地控制和管理依赖关系。


 LD_LIBRARY_PATH=../lib64 g++ -std=c++11 ./src/volume_async_io.cc -o bin/zbs_example_volume_async_io -L../lib64 -I../include  -lzbs-lprotobuf -lzstd -lsnappy -lurcu-common -lfdisk -lmicrohttpd -lunwind -lzbs -g
# Common
LD_LIBRARY_PATH=../lib64 ./script/run-examples.py zbs_example_volume 192.168.61.56:10301:10308

## Ubuntu 2204
### x86
```bash
 sed -i 's/archive.ubuntu.com/mirrors.tuna.tsinghua.edu.cn/g' /etc/apt/sources.list
 apt update
 apt install python3 g++ -y
 ln -s /bin/python3 /bin/python
 ./script/build-examples.py all
 ./script/run-examples.py all 192.168.61.56:10301:10308
```
##  OE 
###  2003

```dockerfile
import subprocess

# 停止并删除已存在的容器
subprocess.run(["docker", "stop", "zbs-all-in-one-cluster"], stdout=subprocess.DEVNULL)
subprocess.run(["docker", "rm", "zbs-all-in-one-cluster"], stdout=subprocess.DEVNULL)

# 运行第一个容器
subprocess.run(["docker", "run", "-d", "--name", "zbs-all-in-one-cluster", "image1"])

# 运行第二个容器，并通过网络连接到第一个容器
subprocess.run(["docker", "run", "--network", "container:zbs-all-in-one-cluster", "image2"])
```

./zadp_packaging/zbs_zadp_test.py test $PWD/build/zadp.tar.gz "127.0.0.1:10301:10308"


```python
def build_deb_amd64_ubuntu22_04():

    subprocess.run(

        [

            "docker",

            "run",

            "--rm",

            "--privileged",

            "-u",

            "root",

            "-v",

            "/home/user/debs/:/home/builder",

            "-v",

            "/home/user/project:/home/builder/zbs",

            "-v",

            "/tmp:/tmp",

            "-w",

            "/home/builder/zbs",

            "registry.smtx.io/zbs/zbs-buildtime:jammy-amd64",

            'bash -c "script/gen_deb_changelog.py | tee debian/changelog; dpkg-buildpackage -us -uc -b"',

        ]

    )
```

```bash
docker run  --privileged -it  -w /root/zbs -v $PWD:/root/zbs registry.smtx.io/zbs/zbs-buildtime:el7-x86_64  bash -c "source /opt/rh/devtoolset-7/enable && ./zadp_packaging/zbs_zadp_packaging.py packaging"
```

```
$1 = (zbs::libzbs::CompareVolumeManager::CompareVolumeTask * const) 0x7f25565c5c70
(gdb) p *this
$2 = {chunk_info_table_ = 0x7f2550040610, meta_ = 0x7f2550008d10, meta_connection_ = {
    _vptr.MetaConnection = 0x7f255b12e638 <vtable for zbs::libmeta::MetaConnection+16>, loop_ = 0x55b579d8ac28, mutex_ = {
      scheduler_ = 0x55b579d8ac28, locked_ = false, owner_ = 0x0, 
      locked_coros_ = {<zbs::intrusive::internal::SizeHolder<false>> = {<No data fields>}, head_ = {next = 0x0}, tail_ = 0x7f25565c5ca8}, 
      scheduled_ = 0x0}, connect_timeout_ms_ = 80, connected_ = false, meta_socket_ = std::shared_ptr<zbs::TcpSocket> (empty) = {get() = 0x0}, 
    status_socket_ = std::shared_ptr<zbs::TcpSocket> (empty) = {get() = 0x0}, common_rpc_service_ = 0x0, meta_service_ = 0x0, chunk_service_ = 0x0, 
    status_service_ = 0x0, nfs_service_ = 0x0, iscsi_service_ = 0x0, nvmf_service_ = 0x0, cdp_service_ = 0x0}, volume_id_1_ = 0x7f255007d788, 
  volume_id_2_ = 0x0, stripe_num_ = 1, stripe_size_ = 262144, sync_ = {<google::protobuf::Closure> = {<No data fields>}, co_ = 0x0, done_ = false}, 
  close_cb_ = {<std::_Maybe_unary_or_binary_function<void, zbs::libzbs::CompareVolumeManager::CompareVolumeTask*>> = {<std::unary_function<zbs::libzbs::CompareVolumeManager::CompareVolumeTask*, void>> = {<No data fields>}, <No data fields>}, <std::_Function_base> = {static _M_max_size = 16, 
      static _M_max_align = 8, _M_functor = {_M_unused = {_M_object = 0x7f2550040550, _M_const_object = 0x7f2550040550, 
          _M_function_pointer = 0x7f2550040550, 
          _M_member_pointer = (void (std::_Undefined_class::*)(std::_Undefined_class * const)) 0x7f2550040550, this adjustment 139798233249008}, 
        _M_pod_data = "P\005\004P%\177\000\000\360t\bP%\177\000"}, 
      _M_manager = 0x7f255ac7dc40 <std::_Function_base::_Base_manager<zbs::libzbs::CompareVolumeManager::CompareVolumeWithBlank(const string&, uint32_t, uint32_t, uint32_t, uint32_t, std::vector<zbs::RangeU64>*)::<lambda(zbs::libzbs::CompareVolumeManager::CompareVolumeTask*)> >::_M_manager(std::_Any_data &, const std::_Any_data &, std::_Manager_operation)>}, 
    _M_invoker = 0x7f255ac80190 <std::_Function_handler<void(zbs::libzbs::CompareVolumeManager::CompareVolumeTask*), zbs::libzbs::CompareVolumeManager::CompareVolumeWithBlank(const string&, uint32_t, uint32_t, uint32_t, uint32_t, std::vector<zbs::RangeU64>*)::<lambda(zbs::libzbs::CompareVolumeManager::CompareVolumeTask*)> >::_M_invoke(const std::_Any_data &, zbs::libzbs::CompareVolumeManager::CompareVolumeTask *&&)>}, 
  running_state_ = zbs::libzbs::CompareVolumeManager::CompareVolumeTask::RunningState::READY, sw_ = {start_ = 14966694369051165, total_time_ = 0, 
    running_ = true}, subtasks_ = std::set with 0 elements, wait_submit_subtasks_ = std::deque with 0 elements, 
  raw_compare_results_ = std::deque with 0 elements, vextent_context_ = std::map with 1 element = {[0] = {pid1 = 7051, pid2 = 0, 
      vextent_prepared = false}}, undone_vextent_tasks_ = std::map with 0 elements, done_vextent_tasks_ = std::map with 1 element = {[{first = 7051, 
      second = 0}] = 0x7f255005abf0}}
```

```bash
hread 2 "zbs-client-prox" hit Breakpoint 1, zbs::libzbs::CompareVolumeManager::CompareVolumeTask::~CompareVolumeTask (this=0x7ffff33dbc70, 
    __in_chrg=<optimized out>) at ../src/libzbs/compare_volume_manager.cc:424
424     ../src/libzbs/compare_volume_manager.cc: No such file or directory.
(gdb) l
419     in ../src/libzbs/compare_volume_manager.cc
(gdb) n
425     in ../src/libzbs/compare_volume_manager.cc
(gdb) p subtasks_
$20 = std::set with 1 element = {[0] = 0x7fffec05a5b0}
(gdb) n
424     in ../src/libzbs/compare_volume_manager.cc
(gdb) p subtasks_
$21 = std::set with 1 element = {[0] = 0x7fffec05a5b0}
(gdb) n
425     in ../src/libzbs/compare_volume_manager.cc
(gdb) n
426     in ../src/libzbs/compare_volume_manager.cc
(gdb) p subtasks_
$22 = std::set with 1 element = {[0] = 0x7fffec05a5b0}
(gdb) s
zbs::libzbs::SubTask::Release (this=0x7fffec05a5b0) at ../src/libzbs/compare_volume_manager.cc:312
312     in ../src/libzbs/compare_volume_manager.cc
(gdb) p this
$23 = (zbs::libzbs::SubTask * const) 0x7fffec05a5b0
(gdb) p *this
$24 = {status_ = {errors_ = 0x0, user_code_ = zbs::UOK}, arena_ = 0x7fffec05a568, pid1_ = 298, pid2_ = 0, cid_ = 1 '\001', vextent_no_ = 0, 
  total_size_ = 262144, remain_retry_times_ = 2, owner_ = 0x7ffff33dbc70, response_ = 0x7fffec05a610}
(gdb) n
313     in ../src/libzbs/compare_volume_manager.cc
(gdb) n
zbs::libzbs::CompareVolumeManager::CompareVolumeTask::~CompareVolumeTask (this=0x7ffff33dbc70, __in_chrg=<optimized out>)
    at ../src/libzbs/compare_volume_manager.cc:425
425     in ../src/libzbs/compare_volume_manager.cc
(gdb) p *this
$25 = {chunk_info_table_ = 0x7fffec0404e0, meta_ = 0x7fffec008cf0, meta_connection_ = {
    _vptr.MetaConnection = 0x7ffff7f44638 <vtable for zbs::libmeta::MetaConnection+16>, loop_ = 0x5555555aac28, mutex_ = {scheduler_ = 0x5555555aac28, 
      locked_ = false, owner_ = 0x0, locked_coros_ = {<zbs::intrusive::internal::SizeHolder<false>> = {<No data fields>}, head_ = {next = 0x0}, 
        tail_ = 0x7ffff33dbca8}, scheduled_ = 0x0}, connect_timeout_ms_ = 80, connected_ = true, 
    meta_socket_ = std::shared_ptr<zbs::TcpSocket> (use count 6, weak count 0) = {get() = 0x7fffec083520}, 
    status_socket_ = std::shared_ptr<zbs::TcpSocket> (use count 3, weak count 0) = {get() = 0x7fffec0835b0}, common_rpc_service_ = 0x7fffec0797a0, 
    meta_service_ = 0x7fffec07f580, chunk_service_ = 0x7fffec07f560, status_service_ = 0x7fffec06fbc0, nfs_service_ = 0x7fffec07f5a0, 
    iscsi_service_ = 0x7fffec0a20d0, nvmf_service_ = 0x7fffec0b0160, cdp_service_ = 0x7fffec0b0810}, volume_id_1_ = 0x7fffec07d658, volume_id_2_ = 0x0, 
  stripe_num_ = 1, stripe_size_ = 262144, sync_ = {<google::protobuf::Closure> = {<No data fields>}, co_ = 0x0, done_ = false}, 
  close_cb_ = {<std::_Maybe_unary_or_binary_function<void, zbs::libzbs::CompareVolumeManager::CompareVolumeTask*>> = {<std::unary_function<zbs::libzbs::CompareVolumeManager::CompareVolumeTask*, void>> = {<No data fields>}, <No data fields>}, <std::_Function_base> = {static _M_max_size = 16, static _M_max_align = 8, 
      _M_functor = {_M_unused = {_M_object = 0x7fffec040420, _M_const_object = 0x7fffec040420, _M_function_pointer = 0x7fffec040420, 
          _M_member_pointer = (void (std::_Undefined_class::*)(std::_Undefined_class * const)) 0x7fffec040420, this adjustment 153}, 
        _M_pod_data = " \004\004\354\377\177\000\000\231\000\000\000\000\000\000"}, 
      _M_manager = 0x7ffff7a93c40 <std::_Function_base::_Base_manager<zbs::libzbs::CompareVolumeManager::CompareVolumeWithBlank(const string&, uint32_t, uint32_t, uint32_t, uint32_t, std::vector<zbs::RangeU64>*)::<lambda(zbs::libzbs::CompareVolumeManager::CompareVolumeTask*)> >::_M_manager(std::_Any_data &, const std::_Any_data &, std::_Manager_operation)>}, 
    _M_invoker = 0x7ffff7a96190 <std::_Function_handler<void(zbs::libzbs::CompareVolumeManager::CompareVolumeTask*), zbs::libzbs::CompareVolumeManager::CompareVolumeWithBlank(const string&, uint32_t, uint32_t, uint32_t, uint32_t, std::vector<zbs::RangeU64>*)::<lambda(zbs::libzbs::CompareVolumeManager::CompareVolumeTask*)> >::_M_invoke(const std::_Any_data &, zbs::libzbs::CompareVolumeManager::CompareVolumeTask *&&)>}, 
  running_state_ = zbs::libzbs::CompareVolumeManager::CompareVolumeTask::RunningState::READY, sw_ = {start_ = 15194673368226090, total_time_ = 0, 
    running_ = true}, subtasks_ = std::set with 0 elements, wait_submit_subtasks_ = std::deque with 0 elements, 
  raw_compare_results_ = std::deque with 0 elements, vextent_context_ = std::map with 1 element = {[0] = {pid1 = 298, pid2 = 0, vextent_prepared = true}}, 
  undone_vextent_tasks_ = std::map with 0 elements, done_vextent_tasks_ = std::map with 1 element = {[{first = 298, second = 0}] = 0x7fffec05a5b0}}
(gdb) p subtasks_
$26 = std::set with 0 elements
(gdb) p subtask
$27 = (zbs::libzbs::SubTask *) 0x7fffec05a5b0
(gdb) n
426     in ../src/libzbs/compare_volume_manager.cc
(gdb) p subtask
$28 = (zbs::libzbs::SubTask *) 0x100000000
```

for (auto* subtask : subtasks_) {  
        subtask->Release();  
    }  
**subtasks_ 类型是set**  
   subtask->Release();这个操作会导致set中的元素被erase  
进而导致再次进入for循环分析这个原因和解决方案

[![](https://ca.slack-edge.com/T04B2HF09-U052MCFH4H4-95f8e7924524-48)](https://app.slack.com/team/U052MCFH4H4)

ClaudeAPP  [4:37 PM](https://smartx1.slack.com/archives/D053D1UN7GD/p1695717427815209)  

根据你提供的额外信息,问题的原因已经较清晰了:1. subtasks_ 本身就是一个 set* 类型,是一个set指针。2. 在for循环中,subtask 是获取到的 set 中的元素。3. subtask->Release() 调用会导致这个元素被从set中删除erase。4. 由于set中元素被删除,set的大小变化,会触发for循环重新开始遍历set。5. 因此存在重复遍历和重复释放资源的问题。这个问题的解决方案可以有:1. 使用普通变量替代 set 指针,避免 set 大小改变影响遍历。2. 事先复制一份 set 的拷贝进行遍历,而不对原set作修改。3. 在循环内部再加一层索引遍历,而不直接操作 set 元素。4. Release函数改为打标记,延后在loop结束后再统一释放。5. 使用迭代器替代for范围遍历,可以避免遍历过程中对容器的修改。6. 将set改为vector,erase不会影响遍历。具体来说,此处使用迭代器遍历或者在循环内使用索引遍历应该是比较好的解决方案,可以避免调用erase导致的不确定问题。 (edited) 

![](https://ca.slack-edge.com/T04B2HF09-U032UHH8LER-36ad05b26d27-48)

Xi Gou  [4:38 PM](https://smartx1.slack.com/archives/D053D1UN7GD/p1695717497439259)  

使用迭代器替代for范围遍历 改一下

[![](https://ca.slack-edge.com/T04B2HF09-U052MCFH4H4-95f8e7924524-48)](https://app.slack.com/team/U052MCFH4H4)

ClaudeAPP  [4:38 PM](https://smartx1.slack.com/archives/D053D1UN7GD/p1695717497908199)  

好的,使用迭代器替代for范围遍历可以这样修改:

cpp
std::set<T>* subtasks_ = ...; 

{
  auto it = subtasks_->begin();
  while (it != subtasks_->end()) {
    auto* subtask = *it; 
    subtask->Release();
    ++it;
  }
}

// 或者:

for(auto it = subtasks_->begin(); it != subtasks_->end(); ++it) {
  auto* subtask = *it;
  subtask->Release(); 
}

使用迭代器遍历的优点:1. 不会因容器大小改变而影响遍历。2. 支持遍历过程中安全地删除当前元素。3. 可以手动控制遍历顺序和emiesionm。4. 同时支持向前和向后遍历。5. 可以记录迭代器状态,后续继续遍历。所以这里使用迭代器替代for范围遍历,可以避免调用erase影响遍历,从而防止循环中重复释放资源的问题。记得包含头文件:`#include <iterator>`迭代器遍历相比for范围遍历更灵活,可以在遍历过程中安全删除元素,适用于类似的场景。 (edited)

```bash
cmake -G Ninja -DBUILD_TESTING=OFF -DBUILD_ENABLE_PMEM=OFF -DBUILD_ENABLE_WERROR=OFF -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_VERBOSE_MAKEFILE=ON -DBUILD_ENABLE_HARDENING=OFF -DBUILD_ENABLE_LTTNG_TRACING=OFF ..
```

 LD_LIBRARY_PATH=../lib64 ZbsHosts=127.0.0.1:10301:10308 gdb ./bin/zbs_example_backup_by_volume_diff_mt

-rw-r--r-- 1 root root 10574 Sep 27 09:59 backup_by_volume_diff_mt.cc
-rw-r--r-- 1 root root  7027 Sep 27 09:59 backup_by_volume_diff_simple.cc
-rw-r--r-- 1 root root   564 Sep 27 09:59 error_code.cc
-rw-r--r-- 1 root root 11020 Sep 27 09:59 full_copy.cc👌
-rw-r--r-- 1 root root  4115 Sep 27 09:59 snapshot.cc 👌
-rw-r--r-- 1 root root 12420 Sep 27 09:59 snapshot_incremental.cc
-rw-r--r-- 1 root root  4033 Sep 27 09:59 volume.cc.  👌
-rw-r--r-- 1 root root  7749 Sep 27 09:59 volume_async_io.cc👌



``` bash
ZADP 依赖如下开源组件：
- glibc - GNU LGPLv2.1
- pthreads - GNU LGPLv2.1
- dl - GNU LGPLv2.1
- systemd - LGPLv2.1+

- rdmacm - BSD-2-Clause

- ibverbs - BSD-2-Clause

- zstd - BSD-3-Clause

- blkld - LGPLv2.1+

- uuid - BSD-3-Clause

- rt - GPLv2/LGPLv2.1

- fdisk - LGPLv2.1+

- rsync - GPLv3+

- unwind - MIT

- stdc++ - GPLv3 with GCC Runtime Library Exception

- m - GPLv3 with GCC Runtime Library Exception

- gcc - GPLv3 with GCC Runtime Library Exception

- lzma - Public Domain 和 LGPLv2.1+

- lz4 - BSD-2-Clause

- cap - BSD-3-Clause

- gcrypt - LGPLv2.1+

- nl - LGPLv2.1+

- nl-route - LGPLv2.1+

- gpg-error - LGPLv2.1+
```


```bash
    LIBZBS_EOK = 0,
    LIBZBS_EUNKNOWN = -100001,  // ==== common
    LIBZBS_EBadArgument = -100002,
    LIBZBS_EMaxLimit = -100007,
    LIBZBS_EOther = -100009,
    LIBZBS_EProto = -100010,
    LIBZBS_EAllocSpace = -100011,
    LIBZBS_ETimedOut = -100012,
    LIBZBS_EShutDown = -100013,
    LIBZBS_ENoSpace = -100014,
    LIBZBS_ECGroup = -100017,
    LIBZBS_ENIOError = -100019,
    LIBZBS_EInvalidPath = -100021,
    LIBZBS_ENotSupport = -100022,
    LIBZBS_EInvalidArgument = -100025,
    LIBZBS_EAsyncEventQueue = -100027,
    LIBZBS_EBadLicense = -100029,
    LIBZBS_EBadKeyFile = -100030,
    LIBZBS_EBadSign = -100031,
    LIBZBS_EBlkDev = -100032,
    LIBZBS_ELicenseNotPermitted = -100034,
    LIBZBS_ELicenseExpired = -100035,
    LIBZBS_ENotEmpty = -100037,
    LIBZBS_ENotImplemented = -100044,
    LIBZBS_EChunkNotFound = -100045,
    LIBZBS_EDBCorrupt = -100046,
    LIBZBS_EDBIOError = -100047,
    LIBZBS_ENoMemory = -100048,
    LIBZBS_ESPDK = -100049,
    LIBZBS_EQueueFull = -100050,
    LIBZBS_ESocket = -100300,  // ==== socket
    LIBZBS_ESocketConnect = -100301,
    LIBZBS_ESocketBind = -100302,
    LIBZBS_ESocketListen = -100303,
    LIBZBS_ESocketAccept = -100304,
    LIBZBS_ESocketSelect = -100305,
    LIBZBS_ESocketClosed = -100306,
    LIBZBS_ESocketEOF = -100307,
    LIBZBS_ESocketPoll = -100308,
    LIBZBS_ESocketShutdown = -100309,
    LIBZBS_ESocketDisconnect = -100310,
    LIBZBS_EBadRequest = -100400,
    LIBZBS_EForbidden = -100403,
    LIBZBS_ENotFound = -100404,
    LIBZBS_EMethodNotAllowed = -100405,
    LIBZBS_EDuplicate = -100409,
    LIBZBS_EConnectError = -100420,
    LIBZBS_EChunksLessThanReplicas = -100421,
    LIBZBS_EChunkConnectUnavailable = -100422,
    LIBZBS_EChunksNotEnoughFreeSpace = -100423,
    LIBZBS_EInternalServerError = -100500,
    LIBZBS_EServiceUnavailable = -100503,
    LIBZBS_EZKConnectError = -100701,  // ==== consensus
    LIBZBS_EZKNoNode = -100702,
    LIBZBS_EZKError = -100703,
    LIBZBS_EZKStopped = -100704,
    LIBZBS_EZKSessionExpired = -100705,
    LIBZBS_EZKNodeExists = -100706,
    LIBZBS_EZKAPIError = -100707,
    LIBZBS_EZKInvalidCallback = -100708,
    LIBZBS_EBadNodeAddress = -100710,
    LIBZBS_EBadClusterStatus = -100711,
    LIBZBS_EZKAlreadyRegistered = -100712,
    LIBZBS_EInvalidDb = -100713,
    LIBZBS_EInvalidDbOp = -100714,
    LIBZBS_EZKCommit = -100715,
    LIBZBS_ENotInDbCluster = -100716,
    LIBZBS_ETooManyPendingJournals = -100717,
    LIBZBS_EBadElectionPathFound = -100718,
    LIBZBS_ENotLeader = -100719,
    LIBZBS_EDbReplay = -100720,
    LIBZBS_EDbClusterCommit = -100721,
    LIBZBS_EZKNotEmpty = -100722,
    LIBZBS_EBadZNodeVersion = -100723,
    LIBZBS_EIncompatibleZkJournal = -100724,
    LIBZBS_ESessionExpired = -100750,
    LIBZBS_EBadSessionEpoch = -100751,
    LIBZBS_ESessionReconnecting = -100752,
    LIBZBS_EZkMarshallingError = -100753,
    LIBZBS_EZkSystemError = -100754,
    LIBZBS_EOpen = -100901,  // ==== io
    LIBZBS_EIOVCountTooBig = -100903,
    LIBZBS_EDiskEOF = -100904,
    LIBZBS_EPathExist = -100905,
    LIBZBS_EPathNotFound = -100906,
    LIBZBS_EPathsRangeError = -100907,
    LIBZBS_EFillZero = -100908,
    LIBZBS_ECAWMiscompare = -100909,
    LIBZBS_ETooFewReplica = -100910,
    LIBZBS_EInvaildAccessPoint = -100911,
    LIBZBS_EDuringSpecialRecover = -100912,
    LIBZBS_EUnsupportedType = -100999,
    LIBZBS_EMetaCorrupt = -1001,  // ==== meta
    LIBZBS_EVolumeBroken = -1003,
    LIBZBS_ERecover = -1004,
    LIBZBS_ESnapshotNotHealthy = -1007,
    LIBZBS_EDumpMeta = -1008,
    LIBZBS_ENoCmdOwner = -1009,
    LIBZBS_ETooManyReplica = -1010,
    LIBZBS_EVolumeShrinked = -1011,
    LIBZBS_ELastReplica = -1012,
    LIBZBS_EModVerfMismatch = -1013,
    LIBZBS_EGuardCheck = -1014,
    LIBZBS_EUpgrade = -1015,
    LIBZBS_ERevokeLeaseFail = -1016,
    LIBZBS_EHasStoragePool = -1017,
    LIBZBS_EVolumeEOF = -1018,
    LIBZBS_ENoHeathyChunk = -1019,
    LIBZBS_EUpgradeTimeout = -1020,
    LIBZBS_EMetaRemoveSlowReplica = -1021,
    LIBZBS_ECNoVolume = -2001,  // ==== chunk
    LIBZBS_ECUnknowOpCode = -2004,
    LIBZBS_ECAllReplicaFail = -2005,
    LIBZBS_ECRejectRecover = -2006,
    LIBZBS_ECAllocExtent = -2009,
    LIBZBS_ECReadOnly = -2010,
    LIBZBS_ECBadLocationInfo = -2011,
    LIBZBS_ECSyncGeneration = -2013,
    LIBZBS_ECGenerationNotMatch = -2014,
    LIBZBS_ECRebalance = -2015,
    LIBZBS_ECBadExtentStatus = -2016,
    LIBZBS_EChunkDataChannelServer = -2020,
    LIBZBS_EPartitionWorker = -2021,
    LIBZBS_EOriginExtentBroken = -2022,
    LIBZBS_ENotOwner = -2023,
    LIBZBS_ENotAlloc = -2024,
    LIBZBS_ENoNeedRecover = -2025,
    LIBZBS_EMetaDisconnect = -2030,
    LIBZBS_EMetaAddReplica = -2031,
    LIBZBS_EMetaRemoveReplica = -2032,
    LIBZBS_EMetaReplaceReplica = -2033,
    LIBZBS_ELeaseExpired = -2034,
    LIBZBS_ENodeMonitorInit = -2035,
    LIBZBS_ENoStatInfo = -2036,
    LIBZBS_ELocalIOFull = -2037,
    LIBZBS_ELSMCanceled = -2038,
    LIBZBS_ELSMIOSlow = -2039,
    LIBZBS_ERetryImmediately = -2040,
    LIBZBS_ELSMInit = -3001,  // ==== lsm
    LIBZBS_EInvalidedChecksumType = -3006,
    LIBZBS_EChecksum = -3007,
    LIBZBS_EPartitionType = -3008,
    LIBZBS_EInvalidPartitionType = -3009,
    LIBZBS_EInvalidExtentStatus = -3010,
    LIBZBS_EIOQueueGet = -3011,
    LIBZBS_EIOQueuePut = -3012,
    LIBZBS_ENotFoundExtent = -3013,
    LIBZBS_EInvalidBIOCB = -3014,
    LIBZBS_EInvalidUIOCB = -3015,
    LIBZBS_EReadInvalid = -3016,
    LIBZBS_ENotEnoughPartitionSpace = -3017,
    LIBZBS_EThreadError = -3018,
    LIBZBS_EIOCTL = -3019,
    LIBZBS_EBadDevice = -3020,
    LIBZBS_EMount = -3021,
    LIBZBS_EFormat = -3022,
    LIBZBS_EExist = -3023,
    LIBZBS_EJournalBoundary = -3024,
    LIBZBS_EAllocateMem = -3025,
    LIBZBS_EReadSuperBlock = -3026,
    LIBZBS_EAllJournalsFull = -3027,
    LIBZBS_EReplayJournals = -3028,
    LIBZBS_EUmount = -3029,
    LIBZBS_ELoadJournalEntry = -3030,
    LIBZBS_EWriteSuperBlock = -3031,
    LIBZBS_EInvalidDeviceSize = -3032,
    LIBZBS_EJournal = -3033,
    LIBZBS_EPartition = -3034,
    LIBZBS_EMMap = -3035,
    LIBZBS_EMUnmap = -3036,
    LIBZBS_ENoJournal = -3037,
    LIBZBS_EUnknowCacheVersion = -3038,
    LIBZBS_EAlreadyInCheck = -3039,
    LIBZBS_EJournalBusy = -3040,
    LIBZBS_EPartitionWorkerBusy = -3041,
    LIBZBS_EBadExtentEpoch = -3042,
    LIBZBS_ENotFoundOrigin = -3043,
    LIBZBS_ENoAvailableDevID = -3044,
    LIBZBS_ELSMBusy = -3045,
    LIBZBS_EPromotion = -3046,
    LIBZBS_EWriteback = -3047,
    LIBZBS_EDeviceStatus = -3048,
    LIBZBS_EExtentEOF = -3049,
    LIBZBS_ELSMNotAllocData = -3050,
    LIBZBS_ENotReady = -5001,  // ==== libzbs
    LIBZBS_EStopped = -5002,
    LIBZBS_EBadHandle = -5003,
    LIBZBS_EIOError = -5004,
    LIBZBS_EDataChannelManager = -5005,
    LIBZBS_EIOThrottle = -5010,
    LIBZBS_ECancelled = -5011,
    LIBZBS_EResetVolume = -5012,
    LIBZBS_EAbortTask = -5013,
    LIBZBS_EDirtyBlockTrackError = -5014,
    LIBZBS_EInitiatorReservationConflict = -9001,  // ==== iscsi
    LIBZBS_EInitiatorInvalidURL = -9002,
    LIBZBS_EInitiatorConnectFail = -9003,
    LIBZBS_EInitiatorTimeOut = -9004,
    LIBZBS_EInitiatorLogoutFail = -9005,
    LIBZBS_EInitiatorDisconnectFail = -9006,
    LIBZBS_EInitiatorDiscoveryFail = -9007,
    LIBZBS_EInitiatorLoginFail = -9008,
    LIBZBS_EInitiatorPollError = -9009,
    LIBZBS_EInitiatorServiceError = -9010,
    LIBZBS_EInitiatorAlreadyLoggedIn = -9012,
    LIBZBS_EInitiatorReportLun = -9013,
    LIBZBS_EInitiatorUnmarshall = -9014,
    LIBZBS_EInitiatorResetLun = -9015,
    LIBZBS_EInitiatorWarmReset = -9016,
    LIBZBS_EInitiatorColdReset = -9017,
    LIBZBS_EInitiatorInquiry = -9018,
    LIBZBS_ERedirectorTargetProbe = -9050,  // ==== iscsi redirector
    LIBZBS_ERedirectorService = -9051,
    LIBZBS_ERedirectorConnection = -9052,
    LIBZBS_ESunRpc = -10001,  // ==== sunrpc
    LIBZBS_ERpcBind = -10002,
    LIBZBS_ECreateXprt = -10003,
    LIBZBS_EWrongRunner = -10101,  // ==== task
    LIBZBS_ERunnerInterrupt = -10102,
    LIBZBS_ETaskCanceled = -10103,
    LIBZBS_EInvalidOperation = -10104,
    LIBZBS_ETaskPaused = -10105,
    LIBZBS_ENetLink = -10201,  // ==== Network
    LIBZBS_EInterFace = -10202,
    LIBZBS_EBadAddress = -10203,
    LIBZBS_EARPBroadCastFailed = -10204,
    LIBZBS_ERDMACreateEventChannel = -10301,  // ====  RDMA
    LIBZBS_ERDMACreateID = -10302,
    LIBZBS_ERDMABind = -10303,
    LIBZBS_ERDMAListen = -10304,
    LIBZBS_ERDMAAccept = -10305,
    LIBZBS_ERDMAReject = -10306,
    LIBZBS_ERDMAConnect = -10307,
    LIBZBS_ERDMADisconnect = -10308,
    LIBZBS_ERDMACreateQP = -10309,
    LIBZBS_ERDMAEventMissingID = -10310,
    LIBZBS_ERDMAEventMissingVerbs = -10311,
    LIBZBS_ERDMARegisterMessages = -10320,
    LIBZBS_ERDMANotMemory = -10321,
    LIBZBS_ERDMAQueryDevice = -10330,
    LIBZBS_ERDMAPostRecv = -10331,
    LIBZBS_ERDMAPostSend = -10332,
    LIBZBS_ERDMAPostRead = -10333,
    LIBZBS_ERDMAPostWrite = -10334,
    LIBZBS_ERDMADeviceRemoved = -10335,
    LIBZBS_EInvalidReString = -10401,  // ==== regular expression
    LIBZBS_EReNotMatch = -10402,
    LIBZBS_EReInternalError = -10403,
    LIBZBS_ECompress = -10501,  // ==== compression
    LIBZBS_EDecompress = -10502

```


对应的 SMTX ZBS 5.4.0 或 SMTX OS 5.1.0 上未包含了所有副本的物理空间占用总和（代码里要确认一下）。之前为不考虑副本数的逻辑空间占用。

确认了 git tag --contains 2e186c4 是 5.4.0