 第4章作业 (cpu-intro)

## 第1题：./process-run.py -l 5:100,5:100
预测：总计需要 10 个时间单位 (Time units)。
核对：运行加 -c 参数后，图表显示在第 10 个时间单位时两个进程都 DONE。
## 第2题：./process-run.py -l 3:0,5:100,5:100,5:0 -c
预测：总计 46 个时间单位。
核对：运行结果最后一行显示 Time: 46
## 第3题：./process-run.py -l 3:0,5:100,5:100,5:0 -c -S SWITCH_ON_END
预测：总计 66 个时间单位。
核对：运行结果最后一行显示 Time: 66
## 第4题：./process-run.py -l 3:0,5:100,5:100,5:0 -c -S SWITCH_ON_IO
预测：总计 46 个时间单位。
核对：运行结果最后一行显示 Time: 46
## 第5题：./process-run.py -l 3:0,5:100,5:100,5:0 -c -I IO_RUN_LATER
预测：总计 46 个时间单位。
核对：运行结果最后一行显示 Time: 46
## 第6题：./process-run.py -l 3:0,5:100,5:100,5:0 -c -I IO_RUN_IMMEDIATE
预测：总计 50 个时间单位。
核对：运行结果最后一行显示 Time: 50
## 第7题：./process-run.py -l 3:0,5:100,5:100,5:0 -c -p 2
预测：总计 46 个时间单位。
核对：运行结果 Stats: Total Time 46
## 第8题：./process-run.py -l 3:0,5:100,5:100,5:0 -c -p 2 -I IO_RUN_IMMEDIATE
预测：总计 50 个时间单位。
核对：运行结果 Stats: Total Time 50