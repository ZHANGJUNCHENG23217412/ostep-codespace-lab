# 第38章作业 (file-ffs)

## 第1题：./ffs.py -f in.example1 -c
预测：模拟 FFS 文件系统，10 个块组，每组 10 个 inode、30 个数据块。
核对：运行结果：
- num_groups: 10
- inodes_per_group: 10
- blocks_per_group: 30
- free data blocks: 289 (of 300)
- free inodes: 93 (of 100)
- spread inodes: False, spread data: False, contig alloc: 1
- 块分配图（group 0-9），其中 group 0 和 1 有 inode 和数据分配。
## 第2题：./ffs.py -f in.example2 -c
预测：模拟 FFS 文件系统，10 个块组，每组 10 个 inode、30 个数据块。
核对：运行结果：
- num_groups: 10
- inodes_per_group: 10
- blocks_per_group: 30
- free data blocks: 269 (of 300)
- free inodes: 98 (of 100)
- spread inodes: False, spread data: False, contig alloc: 1
- group 0 和 group 1 中有 inode 和数据分配（大量 a 字符）。
## 第3题：./ffs.py -f in.fragmented -c
预测：模拟 FFS 文件系统，使用 in.fragmented 输入文件。
核对：运行结果：
- num_groups: 10
- inodes_per_group: 10
- blocks_per_group: 30
- free data blocks: 287 (of 300)
- free inodes: 94 (of 100)
- spread inodes: False, spread data: False, contig alloc: 1
- group 0 分配情况：inode 中分配了 /ib-d-f-h-，数据块中分配了 /ibidifihi 和 iii。