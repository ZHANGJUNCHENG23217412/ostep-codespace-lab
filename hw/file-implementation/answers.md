# 第45章作业 (file-implementation)

## 第1题：./vsfs.py -n 6 -s 16 -r
预测：在反向模式下，模拟器给出文件系统操作，要求预测每一步之后的状态。
核对：运行结果：
- Initial state: inode bitmap 10000000, inodes [d a:0 r:2], data bitmap 10000000
- creat("/y"): 创建文件 y
- fd=open("/y", O_WRONLY|O_APPEND); write(fd, buf, BLOCKSIZE); close(fd): 写入数据
- link("/y", "/m"): 创建硬链接 m
- unlink("/m"): 删除链接 m
- creat("/z"): 创建文件 z
- mkdir("/f"): 创建目录 f
- 每一步后系统都会提示用户预测 inode bitmap、inodes、data bitmap 和 data 的状态。
