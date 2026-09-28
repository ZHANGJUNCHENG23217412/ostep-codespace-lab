# 第26章作业 (threads-api)

## 第1题：valgrind --tool=helgrind ./main-race
预测：main-race.c 存在竞态条件，helgrind 应该能检测到。
核对：运行结果：
- Thread #2 was created at main-race.c:14
- Possible data race during read of size 4 at 0x10c014 by thread #1, main-race.c:15
- This conflicts with a previous write of size 4 by thread #2, main-race.c:8
- Possible data race during write of size 4 at 0x10c014 by thread #1, main-race.c:15
- This conflicts with a previous write of size 4 by thread #2, main-race.c:8
- Address 0x10c014 is 0 bytes inside data symbol "balance"
- ERROR SUMMARY: 2 errors from 2 contexts
## 第2题：valgrind --tool=helgrind ./main-deadlock
预测：main-deadlock.c 存在死锁问题，helgrind 应该能检测到锁顺序问题。
核对：运行结果：
- Address 0x10c080 is 0 bytes inside data symbol "m2"
- ERROR SUMMARY: 1 errors from 1 contexts (suppressed: 8 from 8)
- helgrind 检测到了潜在的死锁风险。
## 第3题：valgrind --tool=helgrind ./main-deadlock-global
预测：main-deadlock-global.c 使用全局锁来避免死锁，helgrind 可能仍然会提示一些潜在问题。
核对：运行结果：
- Address 0x10c0c0 is 0 bytes inside data symbol "m2"
- ERROR SUMMARY: 1 errors from 1 contexts (suppressed: 8 from 8)
- 虽然使用全局锁解决了死锁，但 helgrind 仍然检测到 1 个错误。
## 第4题：valgrind --tool=helgrind ./main-signal
预测：main-signal.c 使用忙等待（spin）来同步父子线程，helgrind 会检测到未加锁的共享变量访问。
核对：运行结果：
- Address 0x10c014 is 0 bytes inside data symbol "done"
- this should print last
- ERROR SUMMARY: 2 errors from 2 contexts (suppressed: 59 from 33)
- helgrind 检测到 2 个错误，主要是对 done 变量的竞态访问。
## 第5题：valgrind --tool=helgrind ./main-signal-cv
预测：main-signal-cv.c 使用条件变量（condition variable）进行同步，比忙等待更高效。helgrind 应该检测不到竞态条件。
核对：运行结果：
- this should print first
- this should print last
- ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 7 from 7)
- 使用条件变量后，没有任何错误，说明同步正确。