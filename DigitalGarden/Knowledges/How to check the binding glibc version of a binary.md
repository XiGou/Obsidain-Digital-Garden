---
dg-publish: true
---
有以下几种方法可以查看一个二进制文件依赖的glibc版本:

1. 使用ldd命令

```
ldd binary_file
```

会显示该二进制文件动态链接的共享库,其中libc.so.6对应glibc版本。

2. objdump反汇编

```
objdump -T binary_file | grep -i glibc
```

打印出使用的glibc符号表,从版本前缀判断版本。

3. 使用readelf

```
readelf -d binary_file | grep libc
```

查看 dynamic section 获取libc版本信息。

4. 查看编译信息

如果有源码,可以查看编译时指定的glibc版本号。

5. rpm/deb查询

如果是从包管理工具安装的,可以查询对应的软件包指定的依赖版本。

6. gdb调试 

```
gdb binary_file
print (const char*) ELF->gnu_required->name 
```

打印链接时使用glibc的SONAME版本。

7. strings strings提取

```
strings binary_file | grep GLIBC
```

从汇编指令中提取glibc符号版本前缀信息。

以上方法都可以用于查询二进制文件Compile/Link时依赖的glibc的具体版本,从而判断二进制兼容范围。