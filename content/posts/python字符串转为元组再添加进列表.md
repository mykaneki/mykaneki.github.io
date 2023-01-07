---
title: "Python字符串转为元组再添加进列表"
date: 2023-01-07
tags: ["Python"]
---

```python
    while True:  # 获取数据
        line = input("请输入学生的姓名，性别，年龄，语文成绩，数学成绩，英语成绩（输入-1时退出）：")
        if line == '-1':
            print("已退出")
            break
        # rows.append(tuple(line))
        line = tuple(line.split(','))
        rows.append(line)
```

`rows.append(tuple(line))` 如果直接如此，则一个字符为一个元素

[('a', 'm', 'y', ',', 'w', 'e', 'm', 'a', 'n', ',', '1', '8', ',', '8', '5', ',', '8', '5', ',', '8', '5')]

`line = tuple(line.split(','))` 应该这样写，会以逗号为分隔符确定元素

('amy', 'weman', '18', '85', '85', '85')
