# 第29章作业 (threads-sema)

## 第1题：gcc -o rendezvous rendezvous.c -Wall -pthread && ./rendezvous
预测：rendezvous 问题要求两个子线程都到达某个点后，父线程才能继续。运行结果应显示两个子线程都执行完 before 后，父线程再执行。
核对：运行结果：
- parent: begin
- child 1: before
- child 1: after
- child 2: before
- child 2: after
- parent: end
- 这表示 rendezvous 同步成功，父线程等待两个子线程都完成后才结束。
