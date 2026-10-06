## Q1
- Prediction / 预测:
总时间 10，CPU 利用率 100%。
状态表：
```
Time PID 0 PID 1 CPU IOs
1 RUN:cpu READY 1
2 RUN:cpu READY 1
3 RUN:cpu READY 1
4 RUN:cpu READY 1
5 RUN:cpu READY 1
6 DONE RUN:cpu 1
7 DONE RUN:cpu 1
8 DONE RUN:cpu 1
9 DONE RUN:cpu 1
10 DONE RUN:cpu 1
11 DONE DONE
```
- Reasoning / 理由:两个进程都只用 CPU，没有 I/O，所以 PID 0 跑完 5 条 CPU 指令后才切到 PID 1。CPU 全程不空闲。
- Verified result / 验证结果:Total Time 10, CPU Busy 10 (100.00%)
- Analysis / 分析:预测与实际一致。两个进程全部为 CPU 指令，没有 I/O，所以 CPU 从第 1 到第 10 tick 一直忙，总时间 10，利用率 100%。没有出现 CPU 空闲的 tick。

## Q2
- Prediction / 预测:
总时间 12，CPU 利用率 4/12 ≈ 33.33%。
状态表：
```
Time PID 0 PID 1 CPU IOs
1 RUN:cpu READY 1
2 RUN:cpu READY 1
3 RUN:cpu READY 1
4 RUN:cpu READY 1
5 DONE RUN:io 1
6 DONE BLOCKED 1
7 DONE BLOCKED 1
8 DONE BLOCKED 1
9 DONE BLOCKED 1
10 DONE BLOCKED 1
11 DONE RUN:io_done 1
12 DONE DONE
```
- Reasoning / 理由:PID 0 先用 4 个 tick 跑完 4 条 CPU 指令。然后切到 PID 1，它只有一次 I/O：第 5 tick 发起 I/O，第 6–10 tick 阻塞 5 tick，第 11 tick 处理完成，第 12 tick 结束。CPU 忙的 tick 是 1–5 和 11，共 6 个 tick。
- Verified result / 验证结果:Total Time 11, CPU Busy 6 (54.55%), IO Busy 5 (45.45%)
- Analysis / 分析:预测总时间 12，实际是 11；预测 CPU 利用率 4/12≈33.33%，实际是 6/11≈54.55%。原因是我把 PID 1 的 RUN:io_done 算成占两个 tick（11 和 12），实际上 io_done 在第 11 tick 就完成，进程立刻 DONE，总时间就是 11。另外 CPU 忙的 tick 是 1–5 和 11，共 6 个，不是 4 个，我漏算了前 4 个 CPU tick 之外的部分。

## Q3
- Prediction / 预测:
总时间 12，CPU 利用率 6/12 ≈ 50%
状态表：
```
Time PID 0 PID 1 CPU IOs
1 RUN:io READY 1
2 BLOCKED RUN:cpu 1
3 BLOCKED RUN:cpu 1
4 BLOCKED RUN:cpu 1
5 BLOCKED RUN:cpu 1
6 BLOCKED DONE 1
7 RUN:io_done DONE 1
8 DONE DONE
```
- Reasoning / 理由:PID 0 先运行，第 1 tick 发起 I/O。因为默认 SWITCH_ON_IO，PID 0 阻塞后立即切换到 PID 1，PID 1 在第 2–5 tick 用 CPU 跑完 4 条指令，第 6 tick 结束。与此同时 PID 0 在第 2–6 tick 阻塞 5 tick。第 7 tick I/O 完成，PID 0 用 1 个 CPU tick 处理完成，第 8 tick 结束。CPU 忙的 tick 是 1、2、3、4、5、7，共 6 个 tick。
- Verified result / 验证结果:Total Time 7, CPU Busy 6 (85.71%), IO Busy 5 (71.43%)
- Analysis / 分析:预测总时间 12，实际是 7。原因是我多算了 PID 0 的 DONE 那一 tick，以为 io_done 之后还有一 tick 才结束。实际上 RUN:io_done 在第 7 tick 完成时 PID 0 立即 DONE，总时间就是 7。CPU 忙的 tick 是 1–6，共 6 个，比例 6/7≈85.71%，比预测的 6/12 高。

## Q4
- Prediction / 预测:
总时间 13，CPU 利用率 10/13 ≈ 76.92%。
状态表：
```
Time PID 0 PID 1 CPU IOs
1 RUN:io READY 1
2 BLOCKED READY 1
3 BLOCKED READY 1
4 BLOCKED READY 1
5 BLOCKED READY 1
6 BLOCKED READY 1
7 RUN:io_done READY 1
8 DONE RUN:cpu 1
9 DONE RUN:cpu 1
10 DONE RUN:cpu 1
11 DONE RUN:cpu 1
12 DONE DONE
```
- Reasoning / 理由:因为 -S SWITCH_ON_END，只有当进程结束时才会切换，所以 PID 0 发起 I/O 并阻塞期间，CPU 不会切到 PID 1，PID 1 一直处于 READY。PID 0 在第 1 tick 发起 I/O，第 2–6 tick 阻塞 5 tick，第 7 tick 处理完成，第 7 tick 结束后切换。PID 1 从第 8 到 11 tick 跑完 4 条 CPU 指令。CPU 忙的 tick 是 1、7、8、9、10、11，共 6 个 tick。
- Verified result / 验证结果:Total Time 11, CPU Busy 6 (54.55%), IO Busy 5 (45.45%)
- Analysis / 分析:预测总时间 12，实际是 11。原因是我多算了 PID 0 的 DONE 那一 tick，以为 io_done 之后还有一 tick 才结束。实际 RUN:io_done 在第 7 tick 完成后 PID 0 立即 DONE。CPU 忙的 tick 是 1、7、8、9、10、11，共 6 个，比例 6/11≈54.55%。SWITCH_ON_END 让 PID 1 一直 READY 到 PID 0 结束才运行，所以总时间比 Q3 略长。

## Q5
- - Prediction / 预测:
总时间 8，CPU 利用率 6/8 = 75%。
状态表：
```
Time PID 0 PID 1 CPU IOs
1 RUN:io READY 1
2 BLOCKED RUN:cpu 1
3 BLOCKED RUN:cpu 1
4 BLOCKED RUN:cpu 1
5 BLOCKED RUN:cpu 1
6 BLOCKED DONE 1
7 RUN:io_done DONE 1
8 DONE DONE
```
- Reasoning / 理由:-S SWITCH_ON_IO 允许在进程发起 I/O 时立即切换，所以 PID 0 第 1 tick 发起 I/O 后，第 2 tick 就切到 PID 1。PID 1 用第 2–5 tick 跑完 4 条 CPU 指令，第 6 tick 结束。同时 PID 0 在第 2–6 tick 阻塞。第 7 tick I/O 完成，PID 0 用 1 tick 处理完成，第 8 tick 结束。CPU 忙的 tick 是 1、2、3、4、5、7，共 6 个 tick，所以利用率 6/8 = 75%。
- Verified result / 验证结果:Total Time 7, CPU Busy 6 (85.71%), IO Busy 5 (71.43%)
- Analysis / 分析:预测总时间 8，实际是 7。原因同样是我多算了 PID 0 的 DONE 那一 tick。RUN:io_done 在第 7 tick 完成时 PID 0 立即 DONE，总时间就是 7。Q5 与 Q3 的模拟行为完全相同（都是 SWITCH_ON_IO，只是 Q5 显式写了 -S 而已），所以结果和 Q3 一样。

## Q6
- - Prediction / 预测:
总时间 21，CPU 利用率 9/21 ≈ 42.86%。
状态表：
```
Time PID 0 PID 1 PID 2 PID 3 CPU IOs
1 RUN:io READY READY READY 1
2 BLOCKED RUN:cpu READY READY 1 1
3 BLOCKED RUN:cpu READY READY 1 1
4 BLOCKED RUN:cpu READY READY 1 1
5 BLOCKED RUN:cpu READY READY 1 1
6 BLOCKED RUN:cpu READY READY 1 1
7 READY DONE RUN:cpu READY 1 1
8 READY DONE RUN:cpu READY 1 1
9 READY DONE RUN:cpu READY 1 1
10 READY DONE RUN:cpu READY 1 1
11 READY DONE RUN:cpu READY 1 1
12 RUN:io DONE DONE RUN:cpu 1
13 BLOCKED DONE DONE RUN:cpu 1 1
14 BLOCKED DONE DONE RUN:cpu 1 1
15 BLOCKED DONE DONE RUN:cpu 1 1
16 BLOCKED DONE DONE RUN:cpu 1 1
17 BLOCKED DONE DONE DONE 1 1
18 RUN:io_done DONE DONE DONE 1
19 RUN:io DONE DONE DONE 1
20 BLOCKED DONE DONE DONE 1
21 RUN:io_done DONE DONE DONE 1
22 DONE DONE DONE DONE
```
- Reasoning / 理由:PID 0 全是 I/O。第 1 tick 发起第一次 I/O，第 2–6 tick 阻塞；因为 IO_RUN_LATER，I/O 完成后 PID 0 不会立刻重跑，而是排到队尾。第 2–6 tick CPU 交给 PID 1；第 7–11 tick 交给 PID 2；第 12–16 tick 交给 PID 3。PID 0 的第二次 I/O 在第 12 tick 发起，第三次在第 19 tick 发起。整个过程 CPU 在 1–6、12、18、19、21 这些 tick 上有工作。
- Verified result / 验证结果:Total Time 31, CPU Busy 21 (67.74%), IO Busy 15 (48.39%)
- Analysis / 分析:预测总时间 21，实际是 31，CPU 利用率也差很多。原因是我把 PID 0 的每次 I/O 完成后的等待时间想短了。IO_RUN_LATER 会把 PID 0 排到队尾，它每次 I/O 完成后都要先等 PID 1–3 中当前在跑的进程跑完，再轮到自己。三次 I/O 累计下来，额外等待时间明显拉长，总时间到 31，CPU 忙 21 个 tick。

## Q7
- Prediction / 预测:
总时间 21，CPU 利用率 11/21 ≈ 52.38%。
状态表：
```
Time PID 0 PID 1 PID 2 PID 3 CPU IOs
1 RUN:io READY READY READY 1
2 BLOCKED RUN:cpu READY READY 1 1
3 BLOCKED RUN:cpu READY READY 1 1
4 BLOCKED RUN:cpu READY READY 1 1
5 BLOCKED RUN:cpu READY READY 1 1
6 BLOCKED RUN:cpu READY READY 1 1
7 RUN:io_done DONE RUN:cpu READY 1 1
8 RUN:io DONE RUN:cpu READY 1 1
9 BLOCKED DONE RUN:cpu READY 1 1
10 BLOCKED DONE RUN:cpu READY 1 1
11 BLOCKED DONE RUN:cpu READY 1 1
12 BLOCKED DONE RUN:cpu READY 1 1
13 BLOCKED DONE DONE RUN:cpu 1 1
14 RUN:io_done DONE DONE RUN:cpu 1
15 RUN:io DONE DONE RUN:cpu 1
16 BLOCKED DONE DONE RUN:cpu 1 1
17 BLOCKED DONE DONE RUN:cpu 1 1
18 BLOCKED DONE DONE DONE 1 1
19 BLOCKED DONE DONE DONE 1
20 RUN:io_done DONE DONE DONE 1
21 DONE DONE DONE DONE
```
- Reasoning / 理由:与 Q6 相同：PID 0 全是 I/O，PID 1–3 各 5 条 CPU。区别是 IO_RUN_IMMEDIATE，PID 0 的 I/O 一完成就立刻重跑，不等其他进程。所以第 7 tick I/O 完成后，PID 0 马上在第 7 tick 处理 `io_done`、第 8 tick 发起下一次 I/O。这样 PID 0 的三次 I/O 能更早完成，但也会频繁打断 CPU 密集型进程。总时间与 Q6 相近，但 CPU 利用率更高，因为 PID 0 的 I/O 完成处理能在第一时间做完。
- Verified result / 验证结果:Total Time 21, CPU Busy 21 (100.00%), IO Busy 15 (71.43%)
- Analysis / 分析:预测总时间 21 对了，但 CPU 利用率预测 11/21≈52.38%，实际是 21/21=100%。原因是我没意识到 IO_RUN_IMMEDIATE 下 PID 0 的 I/O 完成后立即重跑，而且 CPU 密集型进程 PID 1–3 一直在排队等待，CPU 几乎没有空闲的 tick。每次 PID 0 阻塞时，正好有 CPU 密集型进程顶上；PID 0 一完成 I/O 处理，又马上让出 CPU 去发起下一次 I/O。整个过程中 CPU 一直在工作。

## Q8
- Prediction / 预测:
Q8 使用 -s 1 -l 3:50,3:50。种子决定指令序列为：
PID 0: cpu, io, io_done, io, io_done
PID 1: cpu, cpu, cpu
### 默认 (SWITCH_ON_IO + IO_RUN_LATER)
总时间 15，CPU 利用率 8/15 ≈ 53.33%
状态表:
Time PID 0 PID 1 CPU IOs
1 RUN:cpu READY 1
2 RUN:io READY 1
3 BLOCKED RUN:cpu 1
4 BLOCKED RUN:cpu 1
5 BLOCKED RUN:cpu 1
6 BLOCKED DONE 1
7 BLOCKED DONE 1
8 RUN:io_done DONE 1
9 RUN:io DONE 1
10 BLOCKED DONE 1
11 BLOCKED DONE 1
12 BLOCKED DONE 1
13 BLOCKED DONE 1
14 BLOCKED DONE 1
15 RUN:io_done DONE 1
16 DONE DONE

### -I IO_RUN_IMMEDIATE
总时间 16，CPU 利用率 9/16 ≈ 56.25%。
状态表:
Time PID 0 PID 1 CPU IOs
1 RUN:cpu READY 1
2 RUN:io READY 1
3 BLOCKED RUN:cpu 1
4 BLOCKED RUN:cpu 1
5 BLOCKED RUN:cpu 1
6 BLOCKED DONE 1
7 BLOCKED DONE 1
8 RUN:io_done DONE 1
9 RUN:io DONE 1
10 BLOCKED DONE 1
11 BLOCKED DONE 1
12 BLOCKED DONE 1
13 BLOCKED DONE 1
14 BLOCKED DONE 1
15 RUN:io_done DONE 1
16 DONE DONE

### -S SWITCH_ON_END
总时间 17，CPU 利用率 8/17 ≈ 47.06%。
状态表:
Time PID 0 PID 1 CPU IOs
1 RUN:cpu READY 1
2 RUN:io READY 1
3 BLOCKED READY 1
4 BLOCKED READY 1
5 BLOCKED READY 1
6 BLOCKED READY 1
7 BLOCKED READY 1
8 RUN:io_done READY 1
9 RUN:io READY 1
10 BLOCKED READY 1
11 BLOCKED READY 1
12 BLOCKED READY 1
13 BLOCKED READY 1
14 BLOCKED READY 1
15 RUN:io_done READY 1
16 DONE RUN:cpu 1
17 DONE RUN:cpu 1
18 DONE RUN:cpu 1
19 DONE DONE
  
### seed 2 (default, SWITCH_ON_IO + IO_RUN_LATER)
PID 0: io, io_done, io, io_done, cpu
PID 1: cpu, io, io_done, io, io_done
总时间 28，CPU 利用率 8/28 ≈ 28.57%。
状态表:
Time PID 0 PID 1 CPU IOs
1 RUN:io RUN:cpu 1
2 BLOCKED RUN:io 1 1
3 BLOCKED BLOCKED 1 1
4 BLOCKED BLOCKED 1 1
5 BLOCKED BLOCKED 1 1
6 BLOCKED BLOCKED 1 1
7 BLOCKED RUN:io_done 1 1
8 RUN:io_done RUN:io 1
9 RUN:io BLOCKED 1 1
10 BLOCKED BLOCKED 1 1
11 BLOCKED BLOCKED 1 1
12 BLOCKED BLOCKED 1 1
13 BLOCKED BLOCKED 1 1
14 BLOCKED RUN:io_done 1 1
15 RUN:io_done RUN:cpu 1
16 RUN:cpu DONE 1
17 DONE DONE

### seed 2 (-I IO_RUN_IMMEDIATE)
总时间 18，CPU 利用率 8/18 ≈ 44.44%。
状态表:
Time PID 0 PID 1 CPU IOs
1 RUN:io RUN:cpu 1
2 BLOCKED RUN:io 1 1
3 BLOCKED BLOCKED 1 1
4 BLOCKED BLOCKED 1 1
5 BLOCKED BLOCKED 1 1
6 BLOCKED BLOCKED 1 1
7 RUN:io_done BLOCKED 1 1
8 RUN:io RUN:io_done 1
9 BLOCKED RUN:io 1 1
10 BLOCKED BLOCKED 1 1
11 BLOCKED BLOCKED 1 1
12 BLOCKED BLOCKED 1 1
13 BLOCKED BLOCKED 1 1
14 RUN:io_done BLOCKED 1 1
15 RUN:cpu RUN:io_done 1
16 DONE RUN:cpu 1
17 DONE DON

### seed 2 (-S SWITCH_ON_END)
总时间 25，CPU 利用率 8/25 = 32%。
状态表:
Time PID 0 PID 1 CPU IOs
1 RUN:io READY 1
2 BLOCKED READY 1
3 BLOCKED READY 1
4 BLOCKED READY 1
5 BLOCKED READY 1
6 BLOCKED READY 1
7 RUN:io_done READY 1
8 RUN:io READY 1
9 BLOCKED READY 1
10 BLOCKED READY 1
11 BLOCKED READY 1
12 BLOCKED READY 1
13 BLOCKED READY 1
14 RUN:io_done READY 1
15 RUN:cpu READY 1
16 DONE RUN:cpu 1
17 DONE RUN:io 1
18 DONE BLOCKED 1
19 DONE BLOCKED 1
20 DONE BLOCKED 1
21 DONE BLOCKED 1
22 DONE BLOCKED 1
23 DONE RUN:io_done 1
24 DONE RUN:io 1
25 DONE BLOCKED 1
26 DONE BLOCKED 1
27 DONE BLOCKED 1
28 DONE BLOCKED 1
29 DONE BLOCKED 1
30 DONE RUN:io_done 1
31 DONE RUN:cpu 1
32 DONE DONE

### seed 3 (default, SWITCH_ON_IO + IO_RUN_LATER)
PID 0: io, io_done, io, io_done, cpu
PID 1: io, io_done, io, io_done, cpu
总时间 30，CPU 利用率 8/30 ≈ 26.67%。
状态表:
Time PID 0 PID 1 CPU IOs
1 RUN:io READY 1
2 BLOCKED RUN:io 2
3 BLOCKED BLOCKED 2
4 BLOCKED BLOCKED 2
5 BLOCKED BLOCKED 2
6 BLOCKED BLOCKED 2
7 RUN:io_done BLOCKED 1 1
8 RUN:io RUN:io_done 1
9 BLOCKED RUN:io 1 1
10 BLOCKED BLOCKED 1 1
11 BLOCKED BLOCKED 1 1
12 BLOCKED BLOCKED 1 1
13 BLOCKED BLOCKED 1 1
14 RUN:io_done RUN:io_done 1 1
15 RUN:cpu RUN:cpu 1
16 DONE DONE

### seed 3 (-I IO_RUN_IMMEDIATE)
总时间 18，CPU 利用率 8/18 ≈ 44.44%。
状态表:
Time PID 0 PID 1 CPU IOs
1 RUN:io READY 1
2 BLOCKED RUN:io 2
3 BLOCKED BLOCKED 2
4 BLOCKED BLOCKED 2
5 BLOCKED BLOCKED 2
6 BLOCKED BLOCKED 2
7 RUN:io_done BLOCKED 1 1
8 RUN:io RUN:io_done 1
9 BLOCKED RUN:io 1 1
10 BLOCKED BLOCKED 1 1
11 BLOCKED BLOCKED 1 1
12 BLOCKED BLOCKED 1 1
13 BLOCKED BLOCKED 1 1
14 RUN:io_done RUN:io_done 1 1
15 RUN:cpu RUN:cpu 1
16 DONE DONE

### seed 3 (-S SWITCH_ON_END)
总时间 32，CPU 利用率 8/32 = 25%。
状态表:
Time PID 0 PID 1 CPU IOs
1 RUN:io READY 1
2 BLOCKED READY 1
3 BLOCKED READY 1
4 BLOCKED READY 1
5 BLOCKED READY 1
6 BLOCKED READY 1
7 RUN:io_done READY 1
8 RUN:io READY 1
9 BLOCKED READY 1
10 BLOCKED READY 1
11 BLOCKED READY 1
12 BLOCKED READY 1
13 BLOCKED READY 1
14 RUN:io_done READY 1
15 RUN:cpu READY 1
16 DONE RUN:io 1
17 DONE BLOCKED 1
18 DONE BLOCKED 1
19 DONE BLOCKED 1
20 DONE BLOCKED 1
21 DONE BLOCKED 1
22 DONE RUN:io_done 1
23 DONE RUN:io 1
24 DONE BLOCKED 1
25 DONE BLOCKED 1
26 DONE BLOCKED 1
27 DONE BLOCKED 1
28 DONE BLOCKED 1
29 DONE RUN:io_done 1
30 DONE RUN:cpu 1
31 DONE DONE
- Reasoning / 理由:种子 1 下，PID 0 的指令是 cpu, io, io_done, io, io_done，PID 1 是三条 cpu。
- 默认：PID 0 先跑 1 tick CPU，然后 I/O 阻塞；I/O 完成后排在队尾，先让 PID 1 跑完 3 条 cpu，再让 PID 0 完成后续 I/O。
- IO_RUN_IMMEDIATE：PID 0 的 I/O 一完成就立刻重跑，不等 PID 1，所以 PID 0 能更快推进自己的 I/O 序列。
- SWITCH_ON_END：只有进程结束才切换，所以 PID 0 阻塞期间 CPU 完全空闲，PID 1 要等 PID 0 全部跑完才能开始。
- Verified result / 验证结果:seed 1:
- 默认: Total Time 15, CPU Busy 8 (53.33%), IO Busy 10 (66.67%)
- -I IO_RUN_IMMEDIATE: Total Time 15, CPU Busy 8 (53.33%), IO Busy 10 (66.67%)
- -S SWITCH_ON_END: Total Time 18, CPU Busy 8 (44.44%), IO Busy 10 (55.56%)

seed 2:
- 默认: Total Time 16, CPU Busy 10 (62.50%), IO Busy 14 (87.50%)
- -I IO_RUN_IMMEDIATE: Total Time 16, CPU Busy 10 (62.50%), IO Busy 14 (87.50%)
- -S SWITCH_ON_END: Total Time 30, CPU Busy 10 (33.33%), IO Busy 20 (66.67%)

seed 3:
- 默认: Total Time 18, CPU Busy 9 (50.00%), IO Busy 11 (61.11%)
- -I IO_RUN_IMMEDIATE: Total Time 17, CPU Busy 9 (52.94%), IO Busy 11 (64.71%)
- -S SWITCH_ON_END: Total Time 24, CPU Busy 9 (37.50%), IO Busy 15 (62.50%)
- Analysis / 分析:预测的总时间和 CPU 利用率都和实际有差距，主要原因是：
1. 总时间上，只要一个进程的 io_done 完成，它立即 DONE，不会再多占一 tick，我经常多算了一 tick。
2. SWITCH_ON_END 下，I/O 阻塞期间 CPU 完全空闲，所以总时间明显拉长，seed 2 尤其明显（Total 30）。
3. IO_RUN_IMMEDIATE 与默认设置相比，seed 1、seed 2 结果相同（指令序列短，切换次数少），seed 3 下 -I 略优（17 vs 18）。
4. 三种设置里，CPU 利用率在 SWITCH_ON_END 下最低，因为 CPU 在 I/O 阻塞期间空转；默认和 -I 下差别较小。

