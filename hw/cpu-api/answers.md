# 第5章作业 (cpu-api)

## 第1题：./fork.py -a 1
预测：a 生成 b 后，进程树有两个进程：a 和 b。
核对：Process Tree 显示 a 和 b。

## 第2题：./fork.py -a 2
预测：a 生成 b 后，b 退出。最终进程树只剩下 a。
核对：Process Tree 最终显示只有 a。

## 第3题：./fork.py -a 3
预测：a 是根，b 是 a 的子，c 和 d 是 b 的子。
核对：Process Tree 显示：
a
└── b
    ├── c
    └── d

## 第4题：./fork.py -a 4
预测：a 是根，b、c、e 是 a 的子，d 是 b 的子。
核对：Process Tree 显示：
a
├── b
│   └── d
├── c
└── e

## 第5题：./fork.py -a 5
预测：a 是根，c、d、e 是 a 的子进程。
核对：Process Tree 显示：
a
├── c
├── d
└── e

## 第6题：./fork.py -a 6
预测：a 生成 b、c，然后 b、c 退出；接着 a 生成 f、e。最终树里只有 a、f、e。
核对：Process Tree 显示：
a
├── b
│   └── c
├── f
└── e

## 第7题：./fork.py -a 7
预测：a 是根，c、d 是 a 的子。b 生成 e、f、g。c 退出后，树里剩下 a、b、d、e、f、g。
核对：Process Tree 显示：
a
├── e
├── f
└── d

## 第8题：./fork.py -a 8
预测：这是一个复杂的树。b 和 c 退出后，剩下 a、d。d 生成 e，a 生成 f，e 生成 g。
核对：Process Tree 最后显示：
a
└── e