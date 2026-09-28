# 第25章作业 (threads-intro)

## 第1题：./x86.py -p simple-race.s -t 2 -i 2 -c
预测：两个线程执行 simple-race.s，每隔 2 条指令发生一次中断（interrupt）。
核对：运行结果：
- Thread 0 执行 1000 mov 2000(%bx), %ax 和 1001 add $1, %ax
- 发生 Interrupt，切换到 Thread 1
- Thread 1 执行同样的两条指令
- 再发生 Interrupt，切回 Thread 0 执行 1002 mov %ax, 2000(%bx) 和 1003 halt
- 最后 Thread 1 执行 1002 mov %ax, 2000(%bx) 和 1003 halt
- 这个交错执行会导致竞态条件（race condition），最终 2000(%bx) 的值可能不是预期的结果。
## 第2题：./x86.py -p simple-race.s -t 2 -i 1 -c
预测：两个线程执行 simple-race.s，每隔 1 条指令发生一次中断（interrupt）。这次交错更加频繁。
核对：运行结果：
- Thread 0: 1000 mov 2000(%bx), %ax -> Interrupt -> 1001 add $1, %ax -> Interrupt -> 1002 mov %ax, 2000(%bx) -> Interrupt -> 1003 halt
- Thread 1: 1000 mov 2000(%bx), %ax -> Interrupt -> 1001 add $1, %ax -> Interrupt -> 1002 mov %ax, 2000(%bx) -> Interrupt -> 1003 halt
- 注意 Thread 0 和 Thread 1 的指令交错执行，每次只执行一条指令就切换，导致竞态条件更加明显。
- 最终 2000(%bx) 的值取决于 Thread 1 的最后一次写入。