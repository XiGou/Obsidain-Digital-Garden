---
dg-publish: true
Last Edited: 2023-11-17T23:58
date: 2022-11-05 00:22:33
---

## How to check RPM Dependencies

有以下几种方法可以查看一个RPM软件包的依赖关系:

1. 使用rpm命令的-qR参数

```
rpm -qR package.rpm
```

可以查看直接依赖的软件包。

2. 使用yum本地仓库

```
yum deplist package.rpm
```

可以查看详细的依赖树信息。

3. 解开RPM包查看spec文件

每一个RPM软件包里都包含一个spec文件,可以直接查看其中的依赖声明。

4. 查看RPM数据库

```
yumdownloader --resolve package.rpm
```

可以在本地yum数据库中解析所有依赖关系。

5. 使用rpmlint检查包

```
rpmlint package.rpm
```

可以获得详细包内分析报告,包括依赖声明检查等。

6. 手动安装测试依赖情况 

直接安装RPM,通过文件系统和命令进行依赖性检测。

7. 在线RPM数据库查询

比如在koji或repo查询具体软件包的事先声明和解析后的依赖情况。

8. 分析源代码和编译logs

如果有源代码,可以分析编译过程中自动生成的依赖。

以上方法结合使用可以全面了解一个RPM包的直接和间接所有依赖关系。

## List the files in a rpm package file

```bash
rpm -qpl xxx.rpm
```

## Show RPM Package Info

```bash
rpm -qi xxx.rpm
rpm -qpi xxx.rpm
```