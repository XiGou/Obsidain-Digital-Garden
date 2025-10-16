Nmap（Network Mapper）是一个用于网络探测和安全评估的强大工具。要扫描常见的端口，你可以使用以下命令：

```bash
nmap -p 1-1024 <目标IP或主机名>
```


描一个网段，判断哪些 IP 是空闲的

```bash
# ICMP
nmap -sn 192.168.1.0/24

# No ICMP
nmap -Pn -p 80,443 192.168.1.0/24

```