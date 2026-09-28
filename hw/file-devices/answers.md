# 第46章作业 (file-devices)

## 第1题：./process-run.py -l 5:100,5:100 -c
预测：两个进程同时到达，各需 5 条 CPU 指令。系统先运行 Process 0，完成后切换到 Process 1。
核对：运行结果：
- Time 1-5: Process 0 运行 5 条 CPU 指令，Process 1 处于 READY。
- Time 6-10: Process 1 运行 5 条 CPU 指令，Process 0 已完成（DONE）。
- 总时间：10 个时间单位。