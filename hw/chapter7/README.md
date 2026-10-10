# Chapter 7: CPU Scheduling

## 内容

本作业使用 OSTEP `scheduler.py` 模拟器，比较 FIFO、SJF、RR 三种 CPU 调度策略。

- `analysis.md`：所有问题的书面答案
- `data/`：各题的原始模拟器输出

## 题目与文件对应

| 题目 | 内容 | 数据文件 |
|------|------|----------|
| Q1 | FIFO/SJF/RR(q=1) 对比，工作负载 100,200,300 | q1_*.txt |
| Q2 | 随机工作负载 seed=1，三种策略对比 | q2_*.txt |
| Q3 | RR 不同 quantum（1/5/10） | q3_*.txt |
| Q4 | quantum 与切换次数，工作负载 100,200,300 | q4_*.txt |
| Q5 | quantum 与周转时间最优点 | 分析类，无数据文件 |
| Q6 | 护航效应，工作负载 300,100,200 | q6_rr_q1.txt |
| Q7 | SJF 最优性推导 | 分析类，无数据文件 |

## 运行环境

- Python 3.12
- 模拟器：`./ext/ostep-homework/cpu-sched/scheduler.py`

## 复现命令

```bash
python3 ./ext/ostep-homework/cpu-sched/scheduler.py -l 100,200,300 -p FIFO -c
python3 ./ext/ostep-homework/cpu-sched/scheduler.py -l 100,200,300 -p SJF -c
python3 ./ext/ostep-homework/cpu-sched/scheduler.py -l 100,200,300 -p RR -q 1 -c
python3 ./ext/ostep-homework/cpu-sched/scheduler.py -s 1 -p FIFO -c
python3 ./ext/ostep-homework/cpu-sched/scheduler.py -s 1 -p SJF -c
python3 ./ext/ostep-homework/cpu-sched/scheduler.py -s 1 -p RR -q 1 -c
```