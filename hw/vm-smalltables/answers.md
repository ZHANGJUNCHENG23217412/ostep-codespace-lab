# 第15章作业 (vm-smalltables)

## 第1题：./paging-multilevel-translate.py -c
题目：给定多级页表和虚拟地址，计算出物理地址和读取的值，或判断是否 Fault。
核对：运行结果（部分）：
- VA 0x2592:
  - pde index: 0x9, pde contents: 0x9e (valid 1, pfn 0x1e)
  - pte index: 0xc, pte contents: 0xbd (valid 1, pfn 0x3d)
  - Translates to Physical Address 0x7b2 --> Value: 0x1b
- VA 0x3e99:
  - pde index: 0xf, pde contents: 0xd6 (valid 1, pfn 0x56)
  - pte index: 0x14, pte contents: 0xca (valid 1, pfn 0x4a)
  - Translates to Physical Address 0x959 --> Value: 0x1e
- 其他地址（如 0x611c、0x3da8、0x17f5 等）有类似的多级页表映射结果，部分地址可能触发 Fault（页表项无效）。