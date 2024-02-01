```
[main]
#reposdir=/dev/null
protected_multilib=1
multilib_policy=best
#cachedir=/dev/null
keepcache=0
debuglevel=2
exactarch=1
obsoletes=1
gpgcheck=1
plugins=1
installonly_limit=3
metadata_expire=1

[smartx-iso-common]
name=smartx iso common
baseurl=http://192.168.17.20/repo/pub/openeuler/oe2003/smartx-iso-common/aarch64/
gpgcheck=0
enabled=1

[smartx-iso-common-noarch]
name=smartx iso common
baseurl=http://192.168.17.20/repo/pub/openeuler/oe2003/smartx-iso-common/noarch/
gpgcheck=0
enabled=1

[smartxos-base]
name=smartxos base
baseurl=http://192.168.17.20/repo/pub/openeuler/oe2003/qa/aarch64/
gpgcheck=0
enabled=1

[smartxos-base-noarch]
name=smartxos base
baseurl=http://192.168.17.20/repo/pub/openeuler/oe2003/qa/noarch/
gpgcheck=0
enabled=1

[smtx]
name=smartxos base zbs
baseurl=http://192.168.17.20/repo/pub/openeuler/oe2003/smtx/aarch64/
gpgcheck=0
enabled=1

[smtx-zbs]
name=smartxzbs base zbs
baseurl=http://192.168.17.20/repo/pub/openeuler/oe2003/smtx-zbs/aarch64/
gpgcheck=0
enabled=1

[oem-hygon]
name=smartxos oem hygon
baseurl=http://192.168.17.20/repo/pub/openeuler/oe2003/hygon/aarch64/
gpgcheck=0
enabled=1

[oem-hygon-zbs]
name=smartxzbs oem hygon
baseurl=http://192.168.17.20/repo/pub/openeuler/oe2003/hygon-zbs/aarch64/
gpgcheck=0
enabled=1

[oem-ali]
name=smartxos oem ali
baseurl=http://192.168.17.20/repo/pub/openeuler/oe2003/ali/aarch64/
gpgcheck=0
enabled=1

[oem-ali-hygon]
name=smartxos oem ali hygon
baseurl=http://192.168.17.20/repo/pub/openeuler/oe2003/ali-hygon/aarch64/
gpgcheck=0
enabled=1

[oem-ali-zbs]
name=smartxzbs oem ali
baseurl=http://192.168.17.20/repo/pub/openeuler/oe2003/ali-zbs/aarch64/
gpgcheck=0
enabled=1

[oem-ali-hygon-zbs]
name=smartxzbs oem ali hygon
baseurl=http://192.168.17.20/repo/pub/openeuler/oe2003/ali-hygon-zbs/aarch64/
gpgcheck=0
enabled=1

[base]
name=CentOS-$releasever - Base
baseurl=http://192.168.17.20/repo/pub/openeuler/oe2003/os/aarch64/
gpgcheck=0
enabled=1

[smtx-kernel-419]
name=smartxos kernel and driver
baseurl=http://192.168.17.20/kernel/openeuler-4.19.90/
gpgcheck=0
enabled=1


[smartx-iso-common-5.1.x]
name=smartx iso common
baseurl=http://192.168.17.20/repo/pub/openeuler/oe2003/smartx-iso-common-5.1.x/aarch64/
gpgcheck=0
enabled=1

[extra]
name=extra
gpgcheck=0
enabled=1
baseurl=https://repo.oepkgs.net/openEuler/rpm/openEuler-20.03-LTS-SP1/extras/aarch64
```