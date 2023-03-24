---
title: "杂记"
date: 2020-11-03
tags: ["idea","SQL","内存","操作系统","Java"]
showDate: true
---

## 彻底删除idea项目

1. remove module
2. 手动删除文件夹
3. 删除项目引用
4. 使用Everything搜索找到 项目.contexts.zip和项目.tasks.zip两个文件，将其删除

## idea快捷键

批量编辑 ：alt + shift+insert

方法说明注释：输入/** ,点击“Enter”

## 数组越界问题

```c
int main(int argc, char* argv[]){
    int i = 0;
    int arr[3] = {0};
    for(; i<=3; i++){
        arr[i] = 0;
        printf("hello world\n");
    }
    return 0;
}
```

疑问：这段代码的运行结果理论上是无限打印，实际上VS编译器中只打印了四次。

在 C 语言中，只要不是访问受限的内存，所有的内存空间都是可以自由访问的。根据我们前面讲的数组寻址公式，a[3]也会被定位到某块不属于数组的内存地址上，而这个地址正好是存储变量 i 的内存地址，那么 a[3]=0 就相当于 i=0，所以就会导致代码无限循环。

那么，为什么&arr[3]=&i呢？

因为，函数体内的局部变量存在栈上，而且是连续压栈。在Linux进程的内存布局中，栈区在高地址，且从高向低增长，那么实际上arr和i在内存中的状态如下表所示。

| arr[0] | arr[1] | arr[2] | i    |
| ------ | ------ | ------ | ---- |



## 什么时候使用数组而不使用ArrayList等容器

1. 需要存储基本类型
2. 对性能要求较高
3. 需要表达多维时，数组比较直观
4. 数据大小事先已知且操作简单

## 在多条件模糊查询的时候，如何确定SQL语句（实战技巧）

![image-20220410201941262](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202204102019434.png)

如图，在这`"businessAddress like '%"+businessAddress+"%'"`句之前是否需要加and？

解决：利用永真式1=1

```java
String sql = "select * from business where 1=1 ";
        if (businessName!=null&&!businessName.equals("")){//判断字符串是否为空
            sql += "and businessName like '%"+businessName+"%'";
        }
        if (businessAddress!=null&&!businessAddress.equals("")){//判断字符串是否为空
            sql += "and businessAddress like '%"+businessAddress+"%'";
        }
```

## String和StringBuffer的选用

String的字符串对象是字符串常量池里的，如果需要频繁的连接和改动的话，会导致很多不用的字符串常量被废弃

此时，选用StringBuffer效率更高

![image-20220410202733398](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202204102030320.png)

## SQL查询

### where写布尔表达式的注意事项

1. 找出与“Smith”居住在同一城市、同一街道的所有客户的名字。

   ```sql
   --正确
   SELECT
   	customer_name 
   FROM
   	customer 
   WHERE
   	Customer_street IN ( 
           SELECT Customer_street 
           FROM customer 
           WHERE Customer_name = 'Smith' 
       ) 
   	AND 
   	Customer_city IN ( 
           SELECT Customer_city 
           FROM customer 
           WHERE Customer_name = 'Smith' 
       );
       
   --错误 where后并列查询不可以直接用逗号表示，用and
   Select 
   	customer_name 
   from 
   	customer
   where 
   	Customer_street, Customer_city in (
       select Customer_street, Customer_city 
       from customer 
       where Customer_name=’ Smith’ 
   );
   ```

