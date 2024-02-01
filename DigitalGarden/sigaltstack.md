```c++
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

#define STACK_SIZE 8192

void signal_handler(int signum) {
    printf("Signal received: %d\n", signum);
    
    // 递归调用自身，模拟栈溢出
    signal_handler(signum);
}

int main() {
    struct sigaltstack altstack;
    stack_t oldstack;
    
    // 设置替代信号栈
    altstack.ss_sp = malloc(STACK_SIZE);
    altstack.ss_size = STACK_SIZE;
    altstack.ss_flags = 0;
    if (sigaltstack(&altstack, &oldstack) == -1) {
        perror("sigaltstack");
        exit(EXIT_FAILURE);
    }
    
    // 注册信号处理函数
    struct sigaction sa;
    sa.sa_handler = signal_handler;
    sigemptyset(&sa.sa_mask);
    sa.sa_flags = SA_ONSTACK;
    if (sigaction(SIGUSR1, &sa, NULL) == -1) {
        perror("sigaction");
        exit(EXIT_FAILURE);
    }
    
    // 向自身发送信号
    printf("Sending signal...\n");
    kill(getpid(), SIGUSR1);
    
    // 恢复原始信号栈设置
    if (sigaltstack(&oldstack, NULL) == -1) {
        perror("sigaltstack");
        exit(EXIT_FAILURE);
    }
    
    // 释放替代信号栈内存
    free(altstack.ss_sp);
    
    return 0;
}
```