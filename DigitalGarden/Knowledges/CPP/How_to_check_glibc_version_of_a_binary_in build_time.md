---
dg-publish: true
---

There are several ways to check the `glibc` version that a binary file depends on::

1. ldd command

```
ldd binary_file
```

2. objdump, disassembly

```
objdump -T binary_file | grep -i glibc
```

3. readelf

```
readelf -d binary_file | grep libc
```

check dynamic section to get libc version。

4. check the source code repo

If you can access the source code, check the building scripts for the glibc version。

5. rpm/deb package manage tools  
[[RPM]]
6. gdb debugging 

```
gdb binary_file
print (const char*) ELF->gnu_required->name 
```

show the glibc SONAME version when linking。

7. strings extracting

```
strings binary_file | grep GLIBC
```

extracting glibc symbols version prefixes。
