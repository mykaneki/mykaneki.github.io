---
title: "Fflush可移植性不高，用什么来代替？"
date: 2023-01-07
tags: [‘C\C++‘]
---

```c
//方法一
scanf("%*[^\n]");	//清除回车键以前的所有字符
scanf("%*c");	//清除任意一个字符，这里是回车
//方法二            
char c;
while ((c = getchar()) != '\n' && c != EOF);
```
