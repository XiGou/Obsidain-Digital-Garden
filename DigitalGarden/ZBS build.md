安装编译所需要的所有依赖
yum-builddep /home/code/zbs/rpm/zbs.spec


启用 dev-toolset 7 
cat <<EOF >> ~/.bashrc
source /opt/rh/devtoolset-7/enable
alias cmake=cmake3
export PATH=/usr/local/bin:\$PATH
EOF

source ~/.bashrc


安装并配置 ccache，ccache 用于缓存编译结果，加速编译
cp $(which ccache) /usr/local/bin/
ln -s ccache /usr/local/bin/gcc
ln -s ccache /usr/local/bin/g++
ln -s ccache /usr/local/bin/c++
ln -s ccache /usr/local/bin/cc

cat <<EOF > /root/.ccache/ccache.conf
max_size=30G
cache_dir=/home/.ccache/ccache
EOF


更新 git submodule （注意：每次切换分支后都需要执行一次）
git submodule update --init --recursive


准备编译
mkdir build
cd build
cmake -G Ninja ..

# 编译单元测试
ninja zbs_test

# 以发行模式编译 zbs rpm 包（只有提交 commit 的 代码可以被编译）
ninja rpm

# 以 debug 模式编译所有代码（包含目录中未被 commit 的临时代码）
ninja 

# 以 debug 模式编译指定模块的代码（包含目录中未被 commit 的临时代码）
ninja zbs-metad
ninja zbs-chunkd

# ZBS 单测
yum install zookeeper rpcbind
安装好 Zookeeper 和 rpcbind 后，首先 service zookeeper start，然后可以使用脚本初始化 ZBS 单测环境。
#!/usr/bin/bash
```
#
# Copyright (C) 2013-2023 SmartX
# All rights reserved.
#

# preparation
mkdir -p /etc/zbs
mkdir -p /var/log/zbs
mkdir -p /var/lib/zbs/chunkd
mkdir -p /var/lib/zbs/iscsid
mkdir -p /var/lib/zbs/metad
mkdir -p /var/lib/zbs/registry
mkdir -p /var/lib/zbs/zbs-nfsd
chmod -R 1777 /var/log/zbs /var/lib/zbs

# create config file
cat > /etc/zbs/zbs.conf <<  EOF
[network]
data_ip=127.0.0.1
heartbeat_ip=127.0.0.1
vm_ip=127.0.0.1
web_ip=127.0.0.1

[cluster]
role=standalone
members=127.0.0.1
zookeeper=127.0.0.1:2181
mongo=127.0.0.1:27017
EOF
cp -v /etc/zookeeper/zoo_zbs.cfg /etc/zookeeper/zoo.cfg
sed -i 's/cnxTimeout=2/cnxTimeout=10/g' /etc/zookeeper/zoo.cfg

# start rpcbind
/sbin/rpcbind -w

# start zookeeper
zkServer.sh stop
rm -rf /var/log/zookeeper
mkdir -p /var/log/zookeeper
chown -R zookeeper:zookeeper /var/log/zookeeper
zkServer.sh start || die "Failed to start zookeeper"
sleep 3
```


单元测试相关的代码均在 /zbs/src/tests 下，尝试在对应的 test 文件里修改指定的测试用例，例如为某个测试用例添加一个 Log。在编译时使用 ninja ｜ ninja zbs_test 即编译 test case。在编译完成后 test 程序位于  /zbs/build/src  目录下 : zbs_test.
执行 zbs_test 即可运行完整的单元测试，大部分情况下，我们在本地运行时仅需要运行与我们修改相关的测试用例。此时可以用过滤器缩小执行范围，例如
./zbs_test --gtest_filter="*FunctionalTest.WriteResize*"

如果一个单元测试间歇性失败，不是稳定浮现，可以使用 CI 脚本来执行重复运行：
newci-x86_64 -builddir build0 -p 10 -action "/run 2000 FunctionalTest.GCDeleteVolume"

Newci-x86_64 为 CI 程序，可以使用  wget http://192.168.17.20/newci/newci-x86_64 获取。

