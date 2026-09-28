# 第39章作业 (file-journaling)

## 第1题：./fsck.py -c
预测：文件系统初始状态存在一个损坏：INODE 8 指向了死块 7。fsck 会检测并修复它。
核对：运行结果：
- Initial state: inode bitmap 1000100010000101, data bitmap 1000001000001000
- CORRUPTION::INODE 8 points to dead block 7
- Final state: inode bitmap 1000100010000101, data bitmap 1000001000001000
- 修复后，inode 8 的指针被修正为指向有效块 7。
## 第2题：./fsck.py -c -w 1
预测：指定损坏点为 1。初始状态中 inode bitmap 的第 12 位被错误标记为 1，fsck 会检测并修复。
核对：运行结果：
- Initial state: inode bitmap 1000100010000101
- CORRUPTION::INODE BITMAP corrupt bit 12
- Final state: inode bitmap 1000100010001101（第12位被修正）
## 第3题：./fsck.py -c -w 2
预测：指定损坏点为 2。inode 15 的引用计数（refcnt）被错误增加。
核对：运行结果：
- Initial state: inode 15 的 refcnt 显示为 1。
- CORRUPTION::INODE 15 refcnt increased
- Final state: inode 15 的 refcnt 被修正为 2。