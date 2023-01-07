---
title: "Python重定向输入notreadable"
date: 2023-01-07
tags: ["Python"]
---

两处错误

1. 用open打开一个文件，此时调用的是w写入模式，下面使用read是没有权限的，得使用w+读写模式
2. 使用write写入一个字符s，但是此时并没有真正的写入，而是还存在与内存中。此时执行read读取的为空字符。需要执行a.close()以后，再使用a=open(“D://2.txt”)
   a.read()才能够读取到数据。
