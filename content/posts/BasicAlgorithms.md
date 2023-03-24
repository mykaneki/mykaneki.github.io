---
title: "排序"
date: 2023-02-25
tags: ["算法", "数据结构"]
showDate: true

---

## 快速排序

> 分治

**算法步骤**

1. 确定分界点，`x = q[(l+r)/2]`
2. 调整范围，使得左边<=x，右边>=x
3. 递归处理两段

**算法模板（双指针）**

```cpp
#include<iostream>
using namespace std;

const int N = 1e5+10;

int n;
int q[N];

void quick_sort(int q[], int l, int r) {
	// 只有一个数，不需要排序
	if(l >= r) return;
	// x=[l] 不完全正确
	// i和j在数组两侧，因为每次都是先移动再判断
	int x = q[(l+r)/2], i = l - 1, j = r + 1;
	while(i < j) {

		while(q[++i]<x);
		while(q[--j]>x);
		if(i < j) swap(q[i],q[j]);
	}
	quick_sort(q,l,j);
	quick_sort(q,j+1,r);
}

int main() {
	scanf("%d",&n);
	for(int i = 0; i < n; i++) scanf("%d",&q[i]);
	quick_sort(q,0,n-1);
	for(int i = 0; i < n; i++) printf("%d ",q[i]);
	return 0;
}
```

## 归并排序

> 分治

1. 找分界点
2. 递归排序左边和右边
3. 归并——合二为一

## 二分 - 数的范围

1. 找中间值 mid=(l+r)/2
2. check(mid)
3. 更新区间



## 大数计算

> 两个大整数（len(A) < 10^6）相加或大整数与小整数（a < 1000）相乘相除

**大整数怎么存？**每一位存在数组里

**高位在前还是低位在前？** 低位在前，arr[0]=大整数的个位。因为相加会进位，在数组末尾补位数效率较高 

**思考每次计算需要什么？** 三个数的加法 两个整数和进位



```java
public static String solve (String s, String t) {
    // 将字符串放入全局字符串池中
    s = s.intern();
    t = t.intern();
    // 定义两个指针分别指向两个字符串的末尾
    int i = s.length() - 1;
    int j = t.length() - 1;
    StringBuilder ans = new StringBuilder();
    int carry = 0;
    while (i >= 0 || j >= 0) {
        // 取出两个字符相加
        if (i >= 0) {
            carry += s.charAt(i) - '0';
            i--;
        }
        if (j >= 0) {
            carry += t.charAt(j) - '0';
            j--;
        }
        // 将结果存入新的字符串中
        ans.append(carry % 10);
        // 更新进位
        carry = carry / 10;
    }
    if (carry > 0) {
        ans.append(carry);
    }
    // 反转新的字符串并返回
    return ans.reverse().toString();
}
```



## 前缀和

![image-20230315195928594](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202303241358690.png?token=ASIJOH2RTMOKID3LB3TTVGLEDU6FE)



## 差分

已知A，构造B使得A是B的前缀和

![image-20230315200208496](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202303241358940.png?token=ASIJOH2MRTGZZTR3CETDY3DEDU6FS)

