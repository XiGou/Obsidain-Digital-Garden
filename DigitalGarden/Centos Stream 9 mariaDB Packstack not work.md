2024-01-18  2:49:37 0 [ERROR] Failed to setup SSL
2024-01-18  2:49:37 0 [ERROR] SSL error: SSL_CTX_set_default_verify_paths failed
2024-01-18  2:49:37 0 [ERROR] Aborting

[mysql - MariaDB always show SSL not in use when I start mysqld with --ssl - Server Fault](https://serverfault.com/questions/758678/mariadb-always-show-ssl-not-in-use-when-i-start-mysqld-with-ssl)

MariaDB is not allowed to access files in root's home directory `/root`. Thus, the CA, certificate and private key files are unreadable.

The standard locations for these files on RHEL are in directories under `/etc/pki`: `/etc/pki/CA`, `/etc/pki/tls/certs` and `/etc/pki/tls/private`. If you **copy** them here, and set the proper ownership and permissions so that MariaDB can _read_ them, you will find that it works. Alternately, you can place the certificates and private key file in a subdirectory of `/etc/mysql`.

[Share](https://serverfault.com/a/758757 "Short permalink to this answer")

[Site Unreachable](https://www.server-world.info/en/note?os=CentOS_Stream_9&p=mariadb&f=2)

 strace /usr/libexec/mariadbd --basedir=/usr $MYSQLD_OPTS $_WSREP_NEW_CLUSTER

openat(AT_FDCWD, "/etc/mysql/cacert.pem", O_RDONLY) = -1 ENOENT (No such file or directory)
write(2, "2024-01-18  3:26:38 0 [ERROR] Fa"..., 50) = 50
write(2, "2024-01-18  3:26:38 0 [ERROR] SS"..., 81) = 81
write(2, "2024-01-18  3:26:38 0 [ERROR] Ab"..., 39) = 39
futex(0x55e649115568, FUTEX_WAKE_PRIVATE, 1) = 1
madvise(0x7fb4d7096028, 1031664, MADV_DODUMP) = -1 EINVAL (Invalid argument)

readlink("/var/lib/mysql/maria_log_control", 0x7ffc239f9300, 511) = -1 ENOENT (No such file or directory)
access("/var/lib/mysql/maria_log_control", F_OK) = -1 ENOENT (No such file or directory)
readlink("/var/lib/mysql/aria_log_control", 0x7ffc239f9500, 511) = -1 EINVAL (Invalid argument)
access("/var/lib/mysql/aria_log_control", F_OK) = 0

[G031-OP-COA-OPEN-03 CentOS Stream 9 通过 Packstack 安装开源 OpenStack（Y版）\_CloudCS的技术博客\_51CTO博客](https://blog.51cto.com/cloudcs/6049827)