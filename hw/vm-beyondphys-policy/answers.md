# 第16章作业 (vm-beyondphys-policy)

## 第1题：./paging-policy.py -C 3 -a 1,2,3,4,1,2,5,1,2,3,4,5 -c
预测：缓存大小为 3，使用 FIFO 策略。访问序列为 1,2,3,4,1,2,5,1,2,3,4,5。
核对：运行结果：
- FINALSTATS hits 3 misses 9 hitrate 25.00
- 每次访问的详细记录显示了 HIT/MISS 以及缓存中的页面替换情况。
## 第2题：./paging-policy.py -C 4 -a 1,2,3,4,1,2,5,1,2,3,4,5 -c
预测：缓存大小为 4，使用 FIFO 策略。访问序列同上。
核对：运行结果：
- FINALSTATS hits 2 misses 10 hitrate 16.67
- 随着缓存变大，性能反而变差了（Belady 异常）。