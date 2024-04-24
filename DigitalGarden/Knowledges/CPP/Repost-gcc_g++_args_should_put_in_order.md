---
title: "gcc/g++编译参数顺序所引起的链接错误"
---

> From: [gcc/g++编译参数顺序所引起的链接错误\_g++的参数是有顺序要求的-CSDN博客](https://blog.csdn.net/avideointerfaces/article/details/114156653#:~:text=%E5%9C%A8gcc%2Fg%2B%2B%E5%91%BD%E4%BB%A4%E4%B8%AD,%EF%BC%8C%E5%BB%BA%E8%AE%AE%E6%94%BE%E5%88%B0%E5%89%8D%E9%9D%A2%EF%BC%89%E3%80%82)  
TLDR: 我草泥马，这也能出错​, 简单说就是 -l 参数必须放在源代码后面

#### 问题描述

在应用代码里面调用[opencv](https://so.csdn.net/so/search?q=opencv&spm=1001.2101.3001.7020) API（比如imread imshow等），然后用gcc/g++编译就遇到了下面的链接错误。

![](https://img-blog.csdnimg.cn/20210226235233362.png)

#### 问题解析 

这个链接错误很显然是说opencv库没找到或版本不对而导致的。但是g++编译命令是有正确的把opencv库link进来，如下图所示。

![](https://img-blog.csdnimg.cn/20210226235620841.png)

再三确认，/usr/local/lib\_opencv343/lib下面是存在libopencv\_world.so的。 

经过大半天的折腾，后来才发现是gcc编译参数顺序引起的。

在gcc/g++命令中， -I（include参数）比较自由，可以放到源程序文件（\*.cpp/c）的前面或后面（当然为了易读性，建议放到前面）。

但是-lxxx（link参数）就不一样了。 它只能放到源程序文件的后面，来表示前面的源程序依赖于后面-l所指定的库。所以上面的代码要想编译链接通过，就只需要将上面参数中的-lopencv\_world移到opencv\_test.cpp的后面， 来表明前面cpp会link libopencv\_world.so库。 相反，如果写在前面（虽然写的正确），链接时不会去找该库。

#### 结论

所以， gcc/g++编译命令参数对于-l指定动态库是有位置顺序要求的。 当位置不对（在源代码文件的后面）就会引起链接错误。 而这很容易导致开发者花大量精力在调查是否-L所指定的库路径有问题，还可能觉得库版本或c++编译器有问题等等。

