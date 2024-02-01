
https://mirrors.aliyun.com/openeuler/openEuler-22.03-LTS-SP2/EPOL/main/source/Packages/librsync-2.3.2-1.oe2203sp2.src.rpm?spm=a2c6h.25603864.0.0.79fa2afea73Caq
```bash
rpm2cpio librsync-2.3.2-1.oe2203sp2.src.rpm | cpio -di
rpmbuild -bb --define "dist .oe2203"   librsync.spec 
```


protobuf-2.5.0-8.el7.src.rpm
```Dockerfile
FROM openeuler/openeuler:22.03-lts


RUN sed -i 's/http:\/\/repo.openeuler.org\/openEuler-22.03-LTS/http:\/\/mirrors.tuna.tsinghua.edu.cn\/openeuler\/openEuler-22.03-LTS/g' \
    /etc/yum.repos.d/openEuler.repo && \
    curl -o /etc/yum.repos.d/1720.repo http://192.168.17.20/zbs_team/repo/zbs-build/oe2203/1720.repo && \
    yum makecache

RUN yum install -y gcc gcc-g++ rpm-build cpio dnf-utils

RUN mkdir -p /root/rpmbuild/SOURCES && pushd /root/rpmbuild/SOURCES && rpm2cpio /workdir/protobuf-2.5.0-8.el7.src.rpm | cpio -di

```
rpmbuild -bb --define "dist .oe2203" --without python --without java  protobuf.spec 



https://dl.fedoraproject.org/pub/fedora/linux/releases/37/Everything/source/tree/Packages/b/babeltrace2-2.0.4-6.fc37.src.rpm
rpm2cpio babeltrace2-2.0.4-6.fc37.src.rpm | cpio -di
rpmbuild -bb --define "dist .oe2203"   babeltrace2.spec 


跨平台编译v2ex帖子

https://www.v2ex.com/t/749654