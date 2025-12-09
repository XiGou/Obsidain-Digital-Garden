---
title: Linux 中的 fd 分配
---


在 Linux/Unix 系统中，文件描述符（fd）是操作系统为每个打开的文件、套接字或管道分配的 **整数编号**。

## 关键概念

1. **fd 是进程内全局的**

   * 每个进程有自己的 fd 表，fd 是 **进程局部编号**。
   * 不同进程可以有相同的 fd 编号，但在同一进程内是唯一的。

2. **fd 从 0 开始分配**

   * `0` → stdin
   * `1` → stdout
   * `2` → stderr
   * 新的 fd 分配为 **当前可用的最小整数**，关闭的 fd 可以被复用。

3. **动态库不会独立分配 fd**

   * 只要在同一进程内调用 `open()` 或 `socket()`，fd 都从同一张 fd 表中分配。
   * 与是主程序还是动态库无关。

1. **fd 在进程内是全局的**：不管是主程序还是动态库，都是同一张表。
2. **关闭的 fd 可以复用**：分配总是选择最小可用整数。
3. **跨进程 fd 不共享**：不同进程的 fd 编号可以相同，但它们互不干扰。

---

## 实验：主程序和动态库分配 fd

### 动态库 B (libb.so)

```cpp 
// libb.cpp
#include <fcntl.h>
#include <unistd.h>
#include <vector>
#include <iostream>

std::vector<int> b_fds;

extern "C" void b_open(int n) {
    for (int i = 0; i < n; ++i) {
        int fd = open("/dev/null", O_RDONLY);
        b_fds.push_back(fd);
        std::cout << "[B] opened fd: " << fd << std::endl;
    }
}

extern "C" void b_close_all() {
    for (int fd : b_fds) close(fd);
    b_fds.clear();
}
```

---

### 主程序 A

```cpp
// main.cpp
#include <fcntl.h>
#include <unistd.h>
#include <iostream>
#include <vector>
#include <dlfcn.h>

int main() {
    // A Open 3 fd
    for (int i = 0; i < 3; ++i) open("/dev/null", O_RDONLY);

    // load B
    void* handle = dlopen("./libb.so", RTLD_LAZY);
    auto b_open = (void(*)(int)) dlsym(handle, "b_open");
    b_open(10); // B 打开 10 个 fd

    // A open one more fd
    int fd = open("/dev/null", O_RDONLY);
    std::cout << "[A] opened fd after B: " << fd << std::endl;

    dlclose(handle);
    return 0;
}
```

---

### 运行结果

```bash
➜ g++ -fPIC -shared -o libb.so libb.cpp

➜ g++ main.cpp -ldl -o main            

➜ ./main
[A] opened fd: 3
[A] opened fd: 4
[A] opened fd: 5
[B] opened fd: 6
[B] opened fd: 7
[B] opened fd: 8
[B] opened fd: 9
[B] opened fd: 10
[B] opened fd: 11
[B] opened fd: 12
[B] opened fd: 13
[B] opened fd: 14
[B] opened fd: 15
[A] opened fd after B: 16
```
