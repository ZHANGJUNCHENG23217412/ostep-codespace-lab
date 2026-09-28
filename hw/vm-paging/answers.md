# 第14章作业 (vm-paging)

## 第1题：./paging-linear-translate.py -s 1 -a 1k -p 16k -P 1k -n 5 -c
预测：地址空间 1k，物理内存 16k，页大小 1k。VPN 0 对应的页表项无效，所以所有虚拟地址都返回 Invalid。
核对：运行结果：
- Page Table: 0x00000000
- VA 867 --> Invalid (VPN 0 not valid)
- VA 782 --> Invalid (VPN 0 not valid)
- VA 261 --> Invalid (VPN 0 not valid)
- VA 507 --> Invalid (VPN 0 not valid)
- VA 460 --> Invalid (VPN 0 not valid)
## 第2题：./paging-linear-translate.py -s 1 -a 1k -p 16k -P 1k -n 5 -u 100 -c
预测：有效比例改为 100%，所有页表项都有效，所以所有虚拟地址都能成功翻译成物理地址。
核对：运行结果：
- Page Table: 0x80000000
- VA 782 --> 物理地址 14094
- VA 261 --> 物理地址 13573
- VA 507 --> 物理地址 13819
- VA 460 --> 物理地址 13772
- VA 667 --> 物理地址 13979