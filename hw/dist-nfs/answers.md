# 第44章作业 (dist-nfs)

## 第1题：head -n 20 sampleTrace.csv
预测：NFS 的跟踪日志包含大量文件操作（Open、Attr、READ 等），记录了文件 ID、偏移量、大小和主机号。
核对：运行结果：
- 第一行：Open READ FID: 321e0000002988b OFF: 0 SIZE: 1011712 HOST: 2082
- 后续大量 Attr READ 操作，FID 各不相同，OFF 均为 0，SIZE 均为 0，HOST 为 2082 或 2208。
- 这说明 NFS 客户端在访问文件时，会频繁查询文件属性（Attr）和读取数据（READ）。
