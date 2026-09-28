# 第12章作业 (vm-freespace)

## 第1题：./malloc.py -n 5 -r 10 -s 1 -c
预测：分配器从空闲列表中找到合适的块进行分配。空闲列表按地址排序。
核对：运行结果：
- 初始 Free List: [ addr:1000 sz:9 ] [ addr:1009 sz:91 ]
- ptr[1] = Alloc(5) 返回 1000
- Free List: [ addr:1005 sz:4 ] [ addr:1009 sz:91 ]
- Free(ptr[1]) 返回 0
- Free List: [ addr:1000 sz:5 ] [ addr:1005 sz:4 ] [ addr:1009 sz:91 ]
- ptr[2] = Alloc(1) 返回 1005
## 第2题：./malloc.py -A +10,+5,-0,+20,-1 -c
预测：按指定的操作列表执行：分配10、分配5、释放ptr0、分配20、释放ptr1。
核对：运行结果：
- Free(ptr[0]) 返回 0
- Free List: [ addr:1000 sz:10 ] [ addr:1015 sz:85 ]
- ptr[2] = Alloc(20) 返回 1015
- Free List: [ addr:1000 sz:10 ] [ addr:1035 sz:65 ]
- Free(ptr[1]) 返回 0
- Free List: [ addr:1000 sz:10 ] [ addr:1010 sz:5 ] [ addr:1035 sz:65 ]
## 第3题：./malloc.py -A +10,+5,-0,+20,-1 -C -c
预测：加上 -C 参数后，释放内存时会自动合并相邻的空闲块。最终空闲列表应该变短。
核对：运行结果：
- Free(ptr[0]) 返回 0
- Free List: [ addr:1000 sz:10 ] [ addr:1015 sz:85 ]
- ptr[2] = Alloc(20) 返回 1015
- Free List: [ addr:1000 sz:10 ] [ addr:1035 sz:65 ]
- Free(ptr[1]) 返回 0
- Free List: [ addr:1000 sz:15 ] [ addr:1035 sz:65 ]（地址1000和1010合并成一个15大小的块）