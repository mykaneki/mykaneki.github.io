---
title: "把文件设为stdin后如何读数据"
date: 2021-01-07
tags: ["Python"]
---

可以借助sys.stdin.readline() sys.stdin.read() sys.stdin.readlines()

也可以直接用input()实现交互

坑点：在读的时候必须以只读的方式打开，图一是错误的，图二才能正确读出数据

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202204121645673.png" alt="图一" style="zoom:50%;" /><img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202204121648980.png" alt="图二" style="zoom:50%;" />

事实上，图二之所以能够读出数据，是因为指针偏移量重置为0了，在图一的flush后加上seek(0)即可，如图三

![图三](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202204121655269.png)

使用with语句改进：

```python
import sys
import random

with open("in.txt",'w+',encoding='utf8') as f:
    sys.stdin = f
    f.write("55")
    f.flush()
    f.seek(0)
    line = sys.stdin.readline()
    count = 0
    while True:
        print(random.randint(0, int(line)))
        count += 1
        if count == 50:
            break
```
