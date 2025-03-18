
要跟踪自己编写的函数的耗时，可以利用 **eBPF** 的 `uprobe` 和 `uretprobe` 功能，因为这些探测点可以跟踪用户空间的函数执行时间。以下是实现过程的详细步骤：

---

[[use bcc in container]]

## 环境准备

1. **确保支持 eBPF**：
   - 确保 Linux 内核版本 >= 4.4，推荐使用 5.x 或更新版本。
   - 确保安装了 eBPF 工具，如 `bpftrace` 或 `bcc`。

2. **编译你的程序**：
   - 确保你的程序带有调试符号（`-g`），以便能找到函数符号。
   - 示例程序 `my_program`：

     ```c
     #include <stdio.h>
     #include <unistd.h>

     void my_function() {
         sleep(1);  // 模拟耗时操作
         printf("Inside my_function\n");
     }

     int main() {
         my_function();
         return 0;
     }
     ```

   - 编译：

     ```bash
     gcc -g -o my_program my_program.c
     ```

---

## 使用 `bpftrace` 跟踪函数耗时

1. **查找函数符号**：  
   使用 `nm` 或 `objdump` 确定要跟踪的函数符号：

   ```bash
   nm my_program | grep my_function
   ```

   输出示例：

   ```
   0000000000001169 T my_function
   ```

   `my_function` 是全局符号，可以直接跟踪。

2. **编写 bpftrace 脚本**：  
   假设你的程序路径是 `/path/to/my_program`，可以编写如下 `bpftrace` 脚本：

```bash

cat <<EOF > my_bpftrace_script.bt
uprobe:/path/to/my_program:my_function {
    @start[tid] = nsecs;
}
uretprobe:/path/to/my_program:my_function {
    if (@start[tid]) {
        printf("Function my_function latency: %d ns\n", nsecs - @start[tid]);
        delete(@start[tid]);
    }
}
EOF

bpftrace my_bpftrace_script.bt

```

3. **运行程序**：  
   在另一个终端运行你的程序：

   ```bash
   ./my_program
   ```

4. **查看输出**：  
   在运行 `bpftrace` 的终端可以看到类似输出：

   ```
   Function my_function latency: 1000078023 ns
   ```

---

## 使用 `bcc` 实现自定义逻辑

如果需要更复杂的功能（如保存耗时统计、跨多次运行分析），可以使用 `bcc` 编写一个 Python 脚本。

### 示例代码

```python
from bcc import BPF

# eBPF 程序
bpf_program = """
#include <uapi/linux/ptrace.h>

BPF_HASH(start, u32, u64);

int trace_entry(struct pt_regs *ctx) {
    u32 tid = bpf_get_current_pid_tgid();
    u64 ts = bpf_ktime_get_ns();
    start.update(&tid, &ts);
    return 0;
}

int trace_return(struct pt_regs *ctx) {
    u32 tid = bpf_get_current_pid_tgid();
    u64 *tsp = start.lookup(&tid);
    if (tsp != 0) {
        u64 delta = bpf_ktime_get_ns() - *tsp;
        bpf_trace_printk("Function latency: %llu ns\\n", delta);
        start.delete(&tid);
    }
    return 0;
}
"""

# 加载 eBPF 程序
b = BPF(text=bpf_program)

# 绑定函数探测点
binary_path = "/path/to/my_program"
b.attach_uprobe(name=binary_path, sym="my_function", fn_name="trace_entry")
b.attach_uretprobe(name=binary_path, sym="my_function", fn_name="trace_return")

# 输出结果
print("Tracing... Press Ctrl+C to stop.")
try:
    b.trace_print()
except KeyboardInterrupt:
    pass
```

### 运行方法

1. 保存为 `trace_my_function.py`。
2. 运行：

   ```bash
   sudo python3 trace_my_function.py
   ```

3. 在另一终端运行程序：

   ```bash
   ./my_program
   ```

4. 查看输出：

   ```
   Function latency: 1000000123 ns
   ```

---

## 注意事项

1. **动态库的函数**：  
   如果你的函数位于动态库中，需要使用 `ldd` 找到动态库路径，并用 `uprobe` 指定库路径。

2. **多线程程序**：  
   对多线程程序，可以用线程 ID (`tid`) 来区分函数调用。

3. **性能开销**：  
   eBPF 的开销极低，但仍建议避免在高频函数上长期运行探测。

4. **安全性**：  
   运行需要 `sudo` 权限，但对目标程序无侵入性，不会修改程序行为。

---

这种方式能高效跟踪自定义函数的耗时，并提供极大的灵活性，适用于调试和性能分析场景。

