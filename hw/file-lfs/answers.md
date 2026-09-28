# 第40章作业 (file-lfs)

## 第1题：./lfs.py -n 5 -c
预测：日志结构文件系统（LFS）使用写时复制和日志记录。生成 5 个随机命令。
核对：运行结果：
- INITIAL file system contents: checkpoint 3, inode 1-3
- FINAL file system contents: checkpoint 23, 包含多个 inode、目录、文件和数据块。
- 关键变化：checkpoint 从 3 增长到 23，说明 LFS 通过追加写入更新了多次。
## 第2题：./lfs.py -L c,file1:r,file1:w,file1,0,5 -c
预测：使用自定义命令列表：创建 file1、读取 file1、写入 file1（偏移0，5个块）。
核对：运行结果：
- INITIAL file system contents: checkpoint 3
- FINAL file system contents: checkpoint 3（命令执行后，checkpoint 未变）
- 命令执行过程中，file1 被创建、读取和写入，但日志未更新 checkpoint。