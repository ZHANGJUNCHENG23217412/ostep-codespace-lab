# 第7章作业 (cpu-sched)

## 第1题：./scheduler.py -p FIFO -c
预测：FIFO 按顺序运行，Job 0 先跑 9 秒，然后 Job 1 跑 8 秒，最后 Job 2 跑 5 秒。
核对：运行结果如下：
- Job 0: Response 0.00, Turnaround 9.00, Wait 0.00
- Job 1: Response 9.00, Turnaround 17.00, Wait 9.00
- Job 2: Response 17.00, Turnaround 22.00, Wait 17.00
- Average: Response 8.67, Turnaround 16.00, Wait 8.67