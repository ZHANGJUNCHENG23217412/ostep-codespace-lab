# 第10章作业 (cpu-sched-multi)

## 第1题：./multi.py -n 2 -L a:10:50,b:10:50 -c -t
预测：2个CPU，2个作业，各跑10秒，工作集50。2个CPU可以同时跑2个作业，所以10秒全部完成。
核对：运行结果：
- Finished time 10
- CPU 0 utilization 100.00 [ warm 0.00 ]
- CPU 1 utilization 100.00 [ warm 0.00 ]
## 第2题：./multi.py -n 2 -L a:10:50,b:10:50,c:10:50,d:10:50 -c -t
预测：2个CPU，4个作业，各跑10秒，工作集50。2个CPU同时只能跑2个作业，所以需要分两批，总时间20秒。
核对：运行结果：
- Finished time 20
- CPU 0 utilization 100.00 [ warm 0.00 ]
- CPU 1 utilization 100.00 [ warm 0.00 ]
## 第3题：./multi.py -n 2 -L a:10:50,b:10:50,c:10:50,d:10:50 -c -t -C
预测：同上，但加上 -C 追踪缓存状态。每个作业运行时，缓存会标记为 warm 或 cold。
核对：运行结果：
- Finished time 20
- CPU 0 utilization 100.00 [ warm 0.00 ]
- CPU 1 utilization 100.00 [ warm 0.00 ]
- 缓存状态显示每个作业在运行时的缓存情况
## 第4题：./multi.py -n 2 -L a:10:50,b:10:50,c:10:50,d:10:50 -c -t -C -M 50
预测：缓存大小从默认值改为 50，工作集大小也是 50，刚好能放下。追踪缓存状态。
核对：运行结果：
- Finished time 20
- CPU 0 utilization 100.00 [ warm 0.00 ]
- CPU 1 utilization 100.00 [ warm 0.00 ]
- 缓存状态显示每个作业在运行时的缓存情况（w 表示 warm）