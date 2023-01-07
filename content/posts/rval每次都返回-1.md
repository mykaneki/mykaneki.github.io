---
title: "Rval每次都返回 1"
date: 2023-01-07
tags: [‘C\C++’]
---

```c
rval = send(rval, buf, strlen(buf) + 1, 0);//error 10038 WSAENOTSOCK
//无效套接字上的套接字操作。任何一个把SOCKET句柄当作参数的Winsock函数都会返回这个错误。它表明提供的套接字句柄无效。
```

magsock才是获得连接的套接字

rval 改为 msgsock

而`msgsock = accept(sock, (struct sockaddr*)&tcpaddr, (int*)&len);` 
