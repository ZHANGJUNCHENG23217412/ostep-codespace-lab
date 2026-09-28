# 第27章作业 (threads-locks)

## 第1题：./x86.py -p simple-race.s -t 2 -i 2 -c
预测：两个线程执行 simple-race.s，每隔 2 条指令发生中断。这个程序没有加锁，会产生竞态条件。
核对：运行结果：
- Thread 0: 1000 mov 2000, %ax -> 1001 add $1, %ax -> Interrupt
- Thread 1: 1000 mov 2000, %ax -> 1001 add $1, %ax -> Interrupt
- Thread 0: 1002 mov %ax, 2000 -> 1003 halt
- Thread 1: 1002 mov %ax, 2000 -> 1003 halt
- 两个线程都读取了旧的 2000 值，各自加 1 后写回，最终 2000 的值可能只增加了 1（而不是 2），这就是竞态条件。
## 第2题：./x86.py -p looping-race-withlock-withcallret.s -t 2 -i 2 -c
预测：使用加锁版本后，两个线程通过 mutex 互斥，不会产生竞态条件。最终 count 的值应该是正确的（2）。
核对：运行结果：
- Thread 0 和 Thread 1 交替执行，但通过 xchg、test、jne 等指令实现了锁的获取和释放。
- 关键指令：1015 xchg %cx, (%ax)、1016 test $0, %cx、1017 jne .acquire、1018 ret（获取锁）
- 以及 1019 mov -4(%sp), %ax、1020 mov $0, (%ax)、1021 ret（释放锁）
- 加锁后，两个线程对 count 的修改不会互相干扰，最终结果正确。