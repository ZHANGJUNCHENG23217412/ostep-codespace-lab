# 第13章作业 (vm-segmentation)

## 第1题：./segmentation.py -s 1 -a 1k -p 16k -n 5 -b 0 -l 100 -B 200 -L 100 -c
预测：两个段：段0 (base=0, limit=100)，段1 (base=200, limit=100)。
虚拟地址会被映射到段0或段1。如果超出 limit 则触发 SEGMENTATION VIOLATION。
核对：运行结果：
- Segment 0 base: 0, limit: 100
- Segment 1 base: 200, limit: 100
- VA 0: 137 --> SEGMENTATION VIOLATION (SEG0)
- VA 1: 867 --> SEGMENTATION VIOLATION (SEG1)
- VA 2: 782 --> SEGMENTATION VIOLATION (SEG1)
- VA 3: 261 --> SEGMENTATION VIOLATION (SEG0)
- VA 4: 507 --> SEGMENTATION VIOLATION (SEG0)
## 第2题：./segmentation.py -s 1 -a 1k -p 16k -n 5 -b 0 -l 500 -B 200 -L 500 -c
预测：段0 (base=0, limit=500)，段1 (base=200, limit=500)。地址如果在 limit 内则 VALID，否则触发 SEGMENTATION VIOLATION。
核对：运行结果：
- Segment 0 base: 0, limit: 500
- Segment 1 base: 200, limit: 500
- VA 0: 137 --> VALID in SEG0 (137)
- VA 1: 867 --> VALID in SEG1 (物理地址 43)
- VA 2: 782 --> VALID in SEG1 (物理地址 -42)
- VA 3: 261 --> VALID in SEG0 (261)
- VA 4: 507 --> SEGMENTATION VIOLATION (SEG0)