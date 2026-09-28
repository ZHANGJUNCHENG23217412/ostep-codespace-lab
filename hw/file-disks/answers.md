# 第36章作业 (file-disks)

## 第1题：./disk.py -a 10,18 -c
预测：磁盘调度器处理两个请求：10 和 18。
核对：运行结果：
- Block: 10  Seek: 0   Rotate: 105  Transfer: 30  Total: 135
- Block: 18  Seek: 40  Rotate: 170  Transfer: 30  Total: 240
- TOTALS: Seek: 40  Rotate: 275  Transfer: 60  Total: 375
## 第2题：./disk.py -a 18,10 -c
预测：调换请求顺序为 18, 10。由于磁盘寻道策略不同，总时间会变化。
核对：运行结果：
- Block: 18  Seek: 40  Rotate: 305  Transfer: 30  Total: 375
- Block: 10  Seek: 40  Rotate: 50   Transfer: 30  Total: 120
- TOTALS: Seek: 80  Rotate: 355  Transfer: 60  Total: 495
## 第3题：./disk.py -a 10,18 -p SSTF -c
预测：使用 SSTF（最短寻道时间优先）策略。请求 10 和 18，SSTF 会优先处理离磁头最近的请求。
核对：运行结果与 FIFO 一致（因为只有两个请求且初始磁头位置固定）：
- Block: 10  Seek: 0   Rotate: 105  Transfer: 30  Total: 135
- Block: 18  Seek: 40  Rotate: 170  Transfer: 30  Total: 240
- TOTALS: Seek: 40  Rotate: 275  Transfer: 60  Total: 375