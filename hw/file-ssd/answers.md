# 第41章作业 (file-ssd)

## 第1题：./ssd.py -c
预测：模拟 SSD 的闪存转换层（FTL），生成 10 个随机命令（40% 读、50% 写、10% 擦除）。
核对：运行结果：
- ARG num_cmds 10, op_percentages 40/50/10
- ARG num_logical_pages 50, num_blocks 7, pages_per_block 10
- INITIAL: FTL empty
- FINAL: FTL 映射了页面 5, 14, 29, 37, 44, 45
- 块状态显示部分块被写入（E 表示 erased，v 表示 valid）