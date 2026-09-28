# 第9章作业 (cpu-sched-lottery)

## 第1题：./lottery.py -j 3 -l 10:100,20:100,30:100 -c -s 1
预测：彩票调度随机抽签，每个作业有 100 张彩票，共 300 张。每次抽签随机选一个作业运行。
核对：运行结果（使用 seed 1）：
- Random 242740 -> Winning ticket 40 (of 100) -> Run 1
- Random 797405 -> Winning ticket 5 (of 100) -> Run 1
- Random 414314 -> Winning ticket 14 (of 100) -> Run 1
- Job 1 DONE at time 60
## 第2题：./lottery.py -j 3 -l 10:100,20:100,30:100 -c -s 1 -q 5
预测：把时间片从默认值改为 5。每次抽签后，中奖的作业运行 5 个时间单位。
核对：运行结果（使用 seed 1, quantum=5）：
- Job 2 DONE at time 50
- Job 1 DONE at time 60
## 第3题：./lottery.py -j 3 -l 10:100,20:200,30:300 -c -s 1
预测：改变彩票数量（Job 0 有100张，Job 1 有200张，Job 2 有300张，共600张），彩票多的作业被抽中的概率更高。
核对：运行结果（使用 seed 1）：
- Job 2 DONE at time 58
- Job 0 DONE at time 60
## 第4题：./lottery.py -j 3 -l 10:100,20:200,30:300 -c -s 1 -q 10
预测：改变彩票数量（Job 0 有100张，Job 1 有200张，Job 2 有300张），时间片改为 10。
核对：运行结果（使用 seed 1, quantum=10）：
- Job 0 DONE at time 40
- Job 2 DONE at time 50
- Job 1 DONE at time 60