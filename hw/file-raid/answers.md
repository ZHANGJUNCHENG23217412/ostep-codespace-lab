# 第37章作业 (file-raid)

## 第1题：./raid.py -n 6 -L 0 -c
预测：RAID-0（条带化）将数据分散到多个磁盘上。模拟器默认 numDisks=4。
核对：运行结果（实际 numDisks=4）：
- LOGICAL READ from addr:8444 -> read [disk 0, offset 2111]
- LOGICAL READ from addr:4205 -> read [disk 1, offset 1051]
- LOGICAL READ from addr:5112 -> read [disk 0, offset 1278]
- LOGICAL READ from addr:7837 -> read [disk 1, offset 1959]
- LOGICAL READ from addr:4765 -> read [disk 1, offset 1191]
- LOGICAL READ from addr:9081 -> read [disk 1, offset 2270]
## 第2题：./raid.py -n 6 -L 1 -c
预测：RAID-1（镜像）将数据同时写入两个磁盘。模拟器默认 numDisks=4。读取时从任一副本读取。
核对：运行结果（实际 numDisks=4）：
- LOGICAL READ from addr:8444 -> read [disk 0, offset 4222]
- LOGICAL READ from addr:4205 -> read [disk 2, offset 2102]
- LOGICAL READ from addr:5112 -> read [disk 0, offset 2556]
- LOGICAL READ from addr:7837 -> read [disk 2, offset 3918]
- LOGICAL READ from addr:4765 -> read [disk 2, offset 2382]
- LOGICAL READ from addr:9081 -> read [disk 2, offset 4540]
## 第3题：./raid.py -n 6 -L 4 -c
预测：RAID-4 使用一个专门的校验盘。逻辑地址会分散到数据盘，校验盘用于容错。模拟器默认 numDisks=4。
核对：运行结果（实际 numDisks=4）：
- LOGICAL READ from addr:8444 -> read [disk 2, offset 2814]
- LOGICAL READ from addr:4205 -> read [disk 2, offset 1401]
- LOGICAL READ from addr:5112 -> read [disk 0, offset 1704]
- LOGICAL READ from addr:7837 -> read [disk 1, offset 2612]
- LOGICAL READ from addr:4765 -> read [disk 1, offset 1588]
- LOGICAL READ from addr:9081 -> read [disk 0, offset 3027]