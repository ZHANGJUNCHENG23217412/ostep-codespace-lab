# 第7章作业 (cpu-sched)

## 第1题：./scheduler.py -p FIFO -c
预测：FIFO 按顺序运行，Job 0 先跑 9 秒，然后 Job 1 跑 8 秒，最后 Job 2 跑 5 秒。
核对：运行结果如下：
- Job 0: Response 0.00, Turnaround 9.00, Wait 0.00
- Job 1: Response 9.00, Turnaround 17.00, Wait 9.00
- Job 2: Response 17.00, Turnaround 22.00, Wait 17.00
- Average: Response 8.67, Turnaround 16.00, Wait 8.67
## 第2题：./scheduler.py -p SJF -c
预测：SJF 会先选最短的作业。Job 2 (5秒) 先跑，然后 Job 1 (8秒)，最后 Job 0 (9秒)。
核对：运行结果如下：
- Job 2: Response 0.00, Turnaround 5.00, Wait 0.00
- Job 1: Response 5.00, Turnaround 13.00, Wait 5.00
- Job 0: Response 13.00, Turnaround 22.00, Wait 13.00
- Average: Response 6.00, Turnaround 13.33, Wait 6.00
## 第3题：./scheduler.py -p RR -c
预测：RR 使用时间片轮转（默认 quantum=1）。所有作业同时开始，轮流执行。
核对：运行结果如下：
- Job 0: Response 0.00, Turnaround 22.00, Wait 13.00
- Job 1: Response 1.00, Turnaround 21.00, Wait 13.00
- Job 2: Response 2.00, Turnaround 15.00, Wait 10.00
- Average: Response 1.00, Turnaround 19.33, Wait 12.00
## 第4题：./scheduler.py -p RR -q 3 -c
预测：RR 使用时间片轮转，quantum=3。每个作业一次最多运行3秒。
核对：运行结果如下：
- Job 0: Response 0.00, Turnaround 20.00, Wait 11.00
- Job 1: Response 3.00, Turnaround 22.00, Wait 14.00
- Job 2: Response 6.00, Turnaround 17.00, Wait 12.00
- Average: Response 3.00, Turnaround 19.67, Wait 12.33