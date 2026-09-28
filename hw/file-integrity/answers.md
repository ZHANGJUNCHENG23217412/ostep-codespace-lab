# 第42章作业 (file-integrity)

## 第1题：./checksum.py -D 1,2,3,4 -c
预测：计算数据 1,2,3,4 的 Add、Xor 和 Fletcher 校验和。
核对：运行结果：
- Decimal: 1 2 3 4
- Hex: 0x01 0x02 0x03 0x04
- Bin: 0b00000001 0b00000010 0b00000011 0b00000100
- Add: 10 (0b00001010)
- Xor: 4 (0b00000100)
- Fletcher(a,b): 10, 20 (0b00001010, 0b00010100)
