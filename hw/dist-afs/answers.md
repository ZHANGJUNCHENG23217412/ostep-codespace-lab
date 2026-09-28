# 第43章作业 (dist-afs)

## 第1题：./afs.py -A oa1:r1:c1 -s 1 -n 1 -C 1 -c
预测：AFS 客户端缓存文件。一个客户端打开文件 a、读取、然后关闭。
核对：运行结果：
- Server 端 file a 初始内容为 0。
- 客户端 c0: open:a [fd:1] -> read:1 -> 0 -> close:1
- 最终 Server 端 file a 内容仍为 0。
## 第2题：./afs.py -A oa1:r1:c1,oa1:w1:c1 -s 1 -n 2 -C 1 -c
预测：AFS 客户端缓存文件。两个客户端同时操作文件 a：c0 读、c1 写。
核对：运行结果：
- Server 端 file a 初始内容为 0。
- c0: open:a [fd:1] -> read:1 -> 0 -> close:1
- c1: open:a [fd:1] -> write:1 0 -> 1 -> close:1
- 服务器端 file a 最终内容变为 1。
- 当 c0 关闭后，服务器使 c1 的缓存失效，保证一致性。