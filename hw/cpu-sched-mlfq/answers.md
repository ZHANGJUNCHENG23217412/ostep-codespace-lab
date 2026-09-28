# 第8章作业 (cpu-sched-mlfq)

## 第1题：./mlfq.py -n 3 -q 10 -l 0,100,0:0,100,0:0,100,0 -c
预测：3个作业同时到达，各需100秒，MLFQ有3个优先级队列，每个时间片10秒。
核对：运行结果如下：
- Job 0: response 0 - turnaround 280
- Job 1: response 10 - turnaround 290
- Job 2: response 20 - turnaround 300
- Average: response 10.00 - turnaround 290.00
## 第2题：./mlfq.py -n 3 -q 10 -l 0,200,0:0,200,0:0,200,0 -c
预测：3个作业同时到达，各需200秒，MLFQ有3个优先级队列，每个时间片10秒。
核对：运行结果如下：
- Job 0: response 0 - turnaround 580
- Job 1: response 10 - turnaround 590
- Job 2: response 20 - turnaround 600
- Average: response 10.00 - turnaround 590.00
## 第3题：./mlfq.py -n 3 -q 10 -l 0,100,10:0,100,10:0,100,10 -c
预测：3个作业同时到达，各需100秒，每10秒发起一次IO。MLFQ有3个优先级队列。
核对：运行结果如下：
- Job 0: response 0 - turnaround 280
- Job 1: response 10 - turnaround 290
- Job 2: response 20 - turnaround 300
- Average: response 10.00 - turnaround 290.00
## 第4题：./mlfq.py -n 3 -q 10 -l 0,100,10:0,100,10:0,100,10 -S -c
预测：加上 -S 参数后，作业发起 IO 后保持在同一优先级队列（不降级）。
核对：运行结果如下：
- Job 0: response 0 - turnaround 280
- Job 1: response 10 - turnaround 290
- Job 2: response 20 - turnaround 300
- Average: response 10.00 - turnaround 290.00