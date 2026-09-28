# 第11章作业 (vm-mechanism)

## 第1题：./relocation.py -s 1 -a 1k -p 16k -n 5 -b 0 -l 100 -c
预测：Base=0, Limit=100。生成的虚拟地址都大于 100，所以全部会触发 SEGMENTATION VIOLATION（段错误）。
核对：运行结果：
- VA 0: 0x00000089 (137) --> SEGMENTATION VIOLATION
- VA 1: 0x00000363 (867) --> SEGMENTATION VIOLATION
- VA 2: 0x0000030e (782) --> SEGMENTATION VIOLATION
- VA 3: 0x00000105 (261) --> SEGMENTATION VIOLATION
- VA 4: 0x000001fb (507) --> SEGMENTATION VIOLATION
## 第2题：./relocation.py -s 1 -a 1k -p 16k -n 5 -b 0 -l 1000 -c
预测：Base=0, Limit=1000。生成的虚拟地址都小于 1000，所以全部是 VALID（合法）。
核对：运行结果：
- VA 0: 137 --> VALID
- VA 1: 867 --> VALID
- VA 2: 782 --> VALID
- VA 3: 261 --> VALID
- VA 4: 507 --> VALID
## 第3题：./relocation.py -s 1 -a 1k -p 16k -n 5 -b 100 -l 1000 -c
预测：Base=100, Limit=1000。所有虚拟地址加上 Base 后，物理地址都在合法范围内（且不超过物理内存16k），所以全部是 VALID。
核对：运行结果：
- VA 0: 137 --> VALID (物理地址 237)
- VA 1: 867 --> VALID (物理地址 967)
- VA 2: 782 --> VALID (物理地址 882)
- VA 3: 261 --> VALID (物理地址 361)
- VA 4: 507 --> VALID (物理地址 607)