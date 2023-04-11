---
title: "Java复习"
date: 2023-01-01
tags: [‘Java’]
draft: true
---

## day03 Java语言的特性

~

（1）Java的简单性

Java语言底层是**C++**，所以JVM是用C++语言写好的一个虚拟的电脑。

没有指针概念，不允许直接通过指针操作内存；java只支持单继承；

（2）Java的跨平台

（3）Java的健壮性

GC机制，自动垃圾回收机制

（4）Java完全面向对象

（5）Java语言支持多线程

~~

**什么是跨平台，什么是可移植？**~

​	java语言只要编写一次，可以做到到处运行。(.java - .class 字节码 - 机器码 JVM)

跨平台指的是Java程序可以在不同的操作系统上运行，而不需要修改源代码或重新编译。这是因为Java程序运行在JVM（Java虚拟机）上，而JVM负责将Java字节码转换为特定平台的机器码[1](https://bing.com/search?q=java+跨平台+可移植)。

可移植指的是Java程序可以在不同的硬件和软件环境中保持一致的行为和结果。这是因为Java语言规范定义了一套严格的规则，例如数据类型的大小、数值运算的精度、字符编码等~~

**JVM在哪里？怎么安装JVM？**

**不同的操作系统需要安装对应版本的JVM？**

**JDK JRE JVM三者关系？**~

	JDK:Java开发工具箱
	JRE:java运行环境
	JVM:java虚拟机
	
	JDK包括JRE，JRE包括JVM。
	
	JVM是不能独立安装的。
	JRE和JDK都是可以独立安装的。
	有单独的JDK安装包。
	也有单独的JRE安装包。
	没有单独的JVM安装包。
	
	安装JDK的时候：JRE就自动安装了，同时JRE内部的JVM也就自动安装了。
	安装JRE的时候：JVM也就自动安装了。

~~

**java程序从开发到最终运行经历了什么？**~

编译使用编译器（javac【JDK安装后自带】）对xxx.java文件进行编译和运行（JRE在起作用）

只有编译通过了才会生成class字节码文件

编译阶段和运行阶段**可以在不同的操作系统中完成**；

如果需要在不同系统上运行java代码，只需要拷贝`.class`文件并且有JRE~~

## day04 path

**怎么编译？使用什么命令？这个命令怎么用？**~

为什么ipconfig、ping等命令可以使用呢？为什么javac用不了？
		我们发现windows操作系统中有这样一个环境变量，名字叫做：path，
		并且发现path环境变量的值是：
			C:\Windows\system32;%SystemRoot%;%SystemRoot%\System32\Wbem;%SYSTEMROOT%\System32\WindowsPowerShell\v1.0\
		我们还发现了在：C:\Windows\System32 这个目录下存在：ipconfig.exe

		注意：修改完环境变量之后，DOS命令窗口必须关闭重新打开才会起作用。
		注意：环境变量包括“系统变量”和“用户变量”
				系统变量：范围比较大，系统变量会让计算机所有用户都起作用。
				用户变量：范围比较小，这个变量只是作用于当前用户。

~~

**path环境变量的作用是什么？**~
			path环境变量的作用就是给windows操作系统指路的。
			告诉windows操作系统去哪里找这个命令文件。~~

**java HelloWorld.class 对不对？？？？？**~
			不对！！！！
		正确的写法是：
			java HelloWorld~~

**“java HelloWorld”的执行过程以及原理。**~

D:\course\JavaProjects\02-JavaSE\chapter01>java HelloWorld
	敲完回车，都发生了什么？？？？？

1. 启动JVM

2. JVM启动类加载器`classloader`，在硬盘上寻找字节码文件

   > 找不到类对应的字节码文件会报错

   **默认从哪里开始找？如何让类加载器去指定的路径下加载字节码文件？**

   默认情况下类加载器（classloader）会从当前路径下找。

   我们需要设置一个环境变量，叫做：classpath，是给“类加载器”指路的，配置了classpath=D:\course之后，类加载器只会去D:\course目录下“xxx.class”文件不再从当前路径下找了。

   > classpath环境变量不属于windows操作系统，classpath环境变量隶属于java。

~~

## day05 变量、数据类型

**什么是变量？变量的三要素？**~
				变量就是一个存数据盒子。（盒子大小谁来决定啊？数据类型）
				在内存中的**最基本**的存储单元。
				存数据用的，而且这个数据是**可变的**，所以叫做变量。~~

**变量的分类**~

变量的分类可以依据声明的位置；

方法体当中声明的叫做**局部变量**，局部变量只在该方法体中有效，方法结束后该变量的内存就释放了；

方法体外，类体内，声明的变量叫做**成员变量**；

成员变量又包括：**实例变量和静态变量**；

成员变量被static修饰的叫做静态变量，没有被static修饰的叫做实例变量；

```java
// 通过测试得出结论是：a,b没有赋值，c赋值100
		int a, b, c = 100;
```

~~

## day06 数据类型

**数据类型有什么用？**~

数据类型用来声明变量，程序在运行过程中根据不同的数据类型分配不同大小的空间。（确定盒子的大小）~~

**数据类型在java语言中包括两种**~

基本数据类型、引用数据类型

java中整数型字面量被**默认当做int类型来处理**，想以long的形式表示需要在字面量后添加L/l

java开发中浮点型字面量默认被当做double来处理，想默认被当做float处理，需要在字面量后面添加F/f

```java
// 编译器会报错吗？为什么？
// 在java中，整数型字面量一上来编译器就会将它看做int类型
// 而2147483648已经超出了int的范围，所以在没有赋值之前就出错了。
// 记住，不是e放不下2147483648，e是long类型，完全可以容纳2147483648
// 只不过2147483648本身已经超出了int范围。
// 错误: 整数太大
//long e = 2147483648;

// 怎么解决这个问题呢？
long e = 2147483648L;
System.out.println(e);



```



![image-20230227151055566](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202302271511678.png)

！！！

Java语言表达式所操作的`boolean`值，在编译之后都使用`Java`虚拟机中的`int`数据类型来代替，而`boolean`数组将会被编码成`Java`虚拟机的`byte`数组。（**boolean类型数组的访问与修改共用byte类型数组的baload和 bastore指令**，因为两者共用，只有两者字节一样才能通用呀）

有几个取值范围需要大家记住：
			(1个字节)byte: [-128 ~ 127]
			(2个字节)short:[-32768 ~ 32767] 可以表示65536个不同的数字
			(4个字节)int: [-2147483648 ~ 2147483647]
			(2个字节)char: [0~65535]  可以表示65536个不同的数字

			short和char实际上容量相同，不过char可以表示更大的数字。
			因为char表示的是文字，文件没有正负之分，所以char可以表示
			更大的数字。
			
			char可以存储1个汉字吗？
			// 可以的，汉字占用2个字节，java中的char类型占用2个字节，正好。

~~

**float和double存储数据的时候都是存储的近似值。为什么？**~

​      因为现实世界中有这种无线循环的数据，例如：3.3333333333333....

​      数据实际上是无限循环，但是计算机的内存有限，用一个有限的资源

​      表示无限的数据，只能存储近似值。

如果用在银行方面或者说使用在财务方面，double    也是远远不够的，在java中提供了一种精度更高的类型，这种类型专门    使用在财务软件方面：java.math**.BigDecimal** 不是基本数据类型，属于    引用数据类型。）

~~

## day07 类型转换~

八种基本数据类型中，除 boolean 类型不能转换，剩下七种类型之间都可以进行转换

多种数据类型做混合运算的时候，最终的结果类型是“最大容量”对应的类型。 char+short+byte 这个除外。  因为char + short + byte混合运算的时候，会各自先转换成int再做运算。

大容量转换成小容量，可能出现精度损失，谨慎使用；

逻辑运算符：
		& | ! && ||

逻辑运算符要求两边都是布尔类型，并且最终结果还是布尔类型。
				左边是布尔类型 & 右边还是布尔类型 -->最终结果还是布尔类型。
			& 两边都是true，结果才是true
			| 一边是true，结果就是true
			! 取反

			&&实际上和&运算结果完全相同，区别在于：&&有短路现象。
			左边的为false的时候：&& 短路了。
	
			左边为true的时候：|| 短路了。

~~

## day08 

**控制语句包括几类？**~
		3类：
			选择语句  if语句	switch语句
			循环语句
			转向语句

```java
完整语法结构：
				switch(值){ //值允许是String、int，（byte,short,char可以自动转换int）
				case 值1: case 值x:
					java语句;
					break;
				case 值2:
					java语句;
					break;
				case 值3:
					java语句;
					break;
				default:
					java语句;
				}
```

~~

## day 10

**public class 和class的区别**~
		一个java文件中可以定义多个class
		一个class编译之后会生成1个class字节码文件，2个class编译之后会生成2个class文件
		任何一个class中都可以编写main方法，每一个main方法都是一个入口，想选择其中任意一个入口进入，应该怎么做？

想从哪个入口进去执行，你就加载哪个类就行了！！！如：java T1、java T2

以下例子没有main可以编译但不可以运行

```java
public class Test2{

}

public class Test3{
	static void main(String[] args){
	
	}
}
```

public static void main(String[] args){ //这是一个入口方法。同一个类不能有两个入口。(args可以改名字，随意，对于主方法来说只有这个位置可以改，其它位置不能动)

类中写方法，方法中才写Java语句

```java

// 以下程序符合java语法规则吗？
// 不是不运行，是编译报错。编译过不去，运行肯定不行。

public class Test5{

	// 类体当中应该是方法，而不是直接的java语句
	// 这里可以写吗？
	System.out.println("hello1");
	
	// 主方法，入口
	public static void main(String[] args){
	
	}

	// 这里可以写吗？
	System.out.println("hello2");
}
```

​		public的类可以没有
​		**public的类如果有的话，只能有1个**，并**且public的类名要求和文件名一致。**（如果和文件名不一致，报错信息如下：Test8.java:20: 错误: 类 B 是公共的, 应在名为 B.java 的文件中声明）~~

**标识符的命名规范？**~
			见名知意
			驼峰命名方式，一高一低
			类名、接口名：首字母大写，后面每个单词首字母大写。
			变量名、方法名：首字母小写，后面每个单词首字母大写。
			常量名：全部大写，每个单词之间使用下划线衔接。~~

**分析程序运行过程中的内存变化**~
	方法只定义不调用是不会执行的。
	方法调用时：压栈 （在栈中给该方法分配空间）
	方法执行结束时：弹栈（将该方法占用的空间释放，局部变量的内存也释放。）~~

**JVM的内存结构中三块比较重要的内存空间。**~
	方法区：
		存储代码片段，存储xxx.class字节码文件，这个空间是最先有数据的，
		类加载器首先将代码加载到这里。
	堆内存：
		后面讲（面向对象）
	栈内存：
		stack栈当中存储什么？
			每个方法执行时所需要的内存空间（局部变量）。~~

## day 11

**调用方法时，什么情况下“类名.”可以省略?**~

a()方法调用b()方法的时候，a和b方法都在同一个类中，“类名.”可以省略。如果不在同一个类中“类名.”不能省略。~~

**break;语句和return;语句有什么区别？**~

​    不是一个级别。

​    break;用来止switch和离它最近的循环。

​    return;用来终止离它最近的一个方法。~~

**JVM主要内存空间**~

![image-20230227202055802](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202302272020902.png)~~

**计算n的阶乘**~

```java
// 使用递归的方式计算N的阶乘
// 5的阶乘：5 * 4 * 3 * 2 * 1

// 用递归的方式实现一个。
// 使用for循环的方式实现一个。
public class RecursionTest04{

	public static void main(String[] args){
		int n = 5;
		int jieGuo = jieCheng(n);
		System.out.println(jieGuo); // 120

		System.out.println(jieCheng2(5));
	}

	public static int jieCheng2(int n){
		int result = 1;
		for(int i = 2; i <= n; i++){
			result *= i;
		}
		return result;
	}

	public static int jieCheng(int n){
		// 5 * 4 * 3 * 2 * 1
		if(n == 1){
			return 1;
		}
		/*
		int result = n * jieCheng(n - 1);
		return result;
		*/

		return n * jieCheng(n - 1);
	}
}
```

~~

## day 13 类 对象 构造方法~

	这几个术语你需要自己能够阐述出来：
		类：不存在的，人类大脑思考总结一个模板（这个模板当中描述了共同特征。）
		对象：实际存在的个体。
		实例：对象还有另一个名字叫做实例。
		实例化：通过类这个模板创建对象的过程，叫做：实例化。
		抽象：多个对象具有共同特征，进行思考总结抽取共同特征的过程。
	
		类 --【实例化】--> 对象(实例)
		对象 --【抽象】--> 类

~~

**Java语言中，实例变量在访问的时候，是不是必须先创建对象？**~

Java语言中，实例变量是属于对象的变量，必须先创建对象后，才可以通过这个对象来访问。而静态变量是属于类的变量，可以直接使用类名来引用，不需要创建对象。~~

**构造方法**~

		1.构造方法有什么作用？
		2.构造方法怎么定义，语法是什么？
		构造方法不需要指定返回值类型!!
		3.构造方法怎么调用，使用哪个运算符？
		4.什么是缺省构造器？
		5.怎么防止缺省构造器丢失？
		6.实例变量在类加载时初始化吗？实例变量在什么时候初始化？

![image-20230227204010631](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202302272040737.png)

~~

**方法在调用的时候参数是如何传递的？**~

在调用swap方法时，传递的是a和b的值的复制，而不是a和b本身。所以，在swap方法内部，交换了x和y的值，并不会影响到main方法内部的a和b的值。

如果参数是引用类型，那么传递的是引用变量的地址的复制，而不是对象本身。这样，被调用方法可以通过引用变量来访问和修改**对象的属性**，但是不能改变引用变量指向的对象。~~

**如何让swap方法真正地交换两个Person对象呢？**~

使用一个数组来包装这两个对象，然后传递数组的引用。这样，被调用方法就可以改变数组中的元素，从而实现交换。

```java
public class Test {
    public static void main(String[] args) {
        Person p1 = new Person("Tom", 18); // 创建一个Person对象，赋值给p1
        Person p2 = new Person("Jerry", 20); // 创建另一个Person对象，赋值给p2
        Person[] arr = {p1, p2}; // 创建一个数组，包含p1和p2指向的对象
        swap(arr); // 调用swap方法，传递arr的地址的复制
        System.out.println("arr[0] = " + arr[0] + ", arr[1] = " + arr[1]); // 输出arr中的元素
    }

    public static void swap(Person[] x) { // 定义swap方法，接收x的地址的复制
        Person temp = x[0]; // 交换x中的元素
        x[0] = x[1];
        x[1] = temp;
        System.out.println("x[0] = " + x[0] + ", x[1] = " + x[1]); // 输出x中的元素
    }
}

class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    
}
```

运行这段代码，你会看到输出是：

```java
x[0] = Jerry@74a14482, x[1] = Tom@4554617c
arr[0] = Jerry@74a14482, arr[1] = Tom@4554617c
```

~~

**对象和引用的区别？**~

对象是通过new出来的，在堆内存中存储。
引用是在栈内存中存储的一个变量，它指向了对象在堆内存中的地址。~~

**实例变量在类加载时初始化吗？实例变量在什么时候初始化？类变量又是什么时候初始化的？**~

类变量是属于类的，它们在加载类时初始化1。程序可以在两个地方对类变量进行初始化1：

定义类变量时指定初始值，如static int count = 0;
在静态初始化块中对类变量赋值，如static {count = 0;}

[实例变量是属于对象的，它们在对象创建时初始化](https://blog.csdn.net/weixin_41814716/article/details/100020455)[1](https://blog.csdn.net/weixin_41814716/article/details/100020455)[2](https://www.cnblogs.com/walkingzq/p/8490383.html)[。程序可以在三个地方对实例变量进行初始化](https://blog.csdn.net/weixin_41814716/article/details/100020455)[1](https://blog.csdn.net/weixin_41814716/article/details/100020455)：

- 定义实例变量时指定初始值，如`int age = 18;`

- 在构造器中对实例变量赋值，如`this.age = age;`（注意，如果是默认的构造器，也是会赋默认值的）

- 在**初始化块**中对实例变量赋值，如`{age = 18;}`

  > 初始化块是类中的一段代码块，用花括号括起来，它可以对类属性或对象属性进行初始化。
  >
  > 初始化块有两种：普通初始化块和静态初始化块。
  >
  > 普通初始化块在创建对象时隐式调用
  >
  > 静态初始化块在加载类时执行一次

```java
下面是一个代码例子，演示了三种对实例变量进行初始化的方式：

class Person {
    //定义实例变量时指定初始值
    String name = "Tom";
    int age;

    //在构造器中对实例变量赋值
    public Person(int age) {
        this.age = age;
    }

    //在初始化块中对实例变量赋值
    {
        System.out.println("执行普通初始化块");
        age = 18;
    }
}

public class Test {
    public static void main(String[] args) {
        Person p = new Person(20);
        System.out.println(p.name); //输出Tom
        System.out.println(p.age); //输出20
    }
}
```

~~

**静态和普通的初始化块同时出现的代码例子吗 ？ 那么如果有多个初始化块 ， 它们的执行顺序是怎样的呢 ？ 另外，不是说类体内只能写方法体吗？**~

代码例子，同时使用了静态和普通的初始化块：

```java
class Person {
    //定义类变量时指定初始值
    static int count = 0;
    String name = "Tom";
    int age;

    //在静态初始化块中对类变量赋值
    static {
        System.out.println("执行静态初始化块");
        count = 1;
    }

    //在构造器中对实例变量赋值
    public Person(int age) {
        this.age = age;
    }

    //在普通初始化块中对实例变量赋值
    {
        System.out.println("执行普通初始化块");
        age = 18;
        name = "Jerry";
    }
}

public class Test {
    public static void main(String[] args) {
        Person p1 = new Person(20);
        System.out.println(p1.name); //输出Jerry
        System.out.println(p1.age); //输出20
        System.out.println(Person.count); //输出1

        Person p2 = new Person(30);
        System.out.println(p2.name); //输出Jerry
        System.out.println(p2.age); //输出30
        System.out.println(Person.count); //输出1
    }
}
```

如果有多个初始化块，它们的执行顺序是这样的：

- 静态初始化块按照它们在类体中出现的顺序执行一次，只有在第一次加载类时才会执行。
- 普通初始化块按照它们在类体中出现的顺序执行，每次创建对象时都会执行。
- 构造器最后执行，每次创建对象时都会执行。

另外，你说的不是很准确。类体内可以写属性、方法、构造器、初始化块、内部类等成员。但是不能写任意的语句，比如`System.out.println("Hello");`就不是一个合法的成员。~

**{ System.out.println("Hello"); } 加个花括号就可以写任意语句了吗**~

不是的，加个花括号就变成了一个初始化块，它也是一个类的成员，不能随意写语句。初始化块的作用是对属性进行初始化，而不是执行其他的逻辑。如果你想在类体内写任意的语句，你可以把它们放在一个方法里面，然后调用这个方法。~~

**Java中 为什么不能在类体内直接执行语句~**

Java是一种面向对象的编程语言，它要求所有的代码都必须封装在类和方法中。类是一种抽象的数据类型，它定义了对象的属性和行为。方法是一种具体的操作，它实现了类的行为或功能。在类体内直接执行语句会破坏类的封装性和结构性，导致代码难以理解和维护。

如果你想要在类体内输出一些信息，你可以使用静态代码块或构造方法~~

## day 16 this 继承~

（1）this在内存中是怎么存储的，this的本质是什么

this是一个关键字，全部小写。

this存储在**堆内存的对象内部**。

**this是一个变量，是一个引用，保存内存地址指向自身。**

（2）this可以使用在实例方法中，**不能使用在静态方法中**

this使用在实例方法中，谁调用这个实例方法this就代表谁，**this代表的是当前正在执行这个动作的对象**，而静态方法的执行没有当前对象的参与，是通过“类名.”的方式访问的，固然在静态方法中是无法使用this的。

（3）在实例方法中，this什么时候可以省略，什么时候不能省略

实例方法中的“this.”多数情况下是可以省略的，但如果是为了区分局部变量和实例变量的话，不能省略。

（4）静态方法中可以直接访问本类中其他静态方法，不能直接访问本类中其他实例方法

（5）实例方法中可以直接访问本类中其他静态方法，也可以直接访问本类中其他实例方法

（6）一条法则：静态方法采用“类名.”方式访问，实例方法采用“引用.”方式访问。你需要记住“类名.”什么时候可以省略，“引用.”什么时候可以省略

（7）在当前的构造方法中使用“this(实参)”调用本类中其他构造方法

目的是代码复用。

**this()只能出现在构造方法第一行。**

~~

**子类继承父类，在创建子类对象时，构造方法是如何执行的**~

（1）在创建子类的构造方法在执行的时候会先去调用父类的构造方法。

（2）虽然父类的构造方法执行了，但并**不会创建父类对象**，父类构造方法执行的作用是为了**初始化当前子类对象中的父类型特征**。

（3）创建子类对象的时候，Object类中的无参数构造方法是一定会执行的。

（4）当一个构造方法第一行没有显示的调用“this(参数)”，也没有显示的调用“super(参数)”时，**默认**会有一个“super();”

（5）this()和super()都只能出现在构造方法**第一行**，**两者不能共存。**~~

## day 17 继承 多态

**方法重载和方法覆盖有什么区别？**~

		方法重载发生在同一个类当中。
	
		方法覆盖是发生在具有继承关系的父子类之间。
	
		方法重载是一个类中，方法名相同，参数列表不同。
	
		方法覆盖是具有继承关系的父子类，并且重写之后的方法必须和之前的方法一致：
			方法名一致、参数列表一致、返回值类型一致。

~~

**向上转型和向下转型**~

向上转型：子--->父 (upcasting)
			又被称为自动类型转换：Animal a = new Cat();

向下转型：父--->子 (downcasting)
			又被称为强制类型转换：Cat c = (Cat)a; 需要添加强制类型转换符。

ClassCastException（类型转换异常）

向下转型之前一定要使用**instanceof**运算符进行判断。~~

**什么是多态。**~
		多种形态，多种状态，编译和运行有两个不同的状态。
		编译期叫做静态绑定。
		运行期叫做动态绑定。

Animal a = new Cat();//编译看左边，方法看右边

~~

**final关键字**~

1. final修饰的类无法被继承
2. final修饰的方法无法被覆盖
3. final修饰的变量，只能赋一次值
4. final修饰的实例变量必须**手动赋值**，不能采用系统默认值。
5. final修饰的引用一旦指向某个对象，则不能再重新指向其它对象，但该引用指向的对象**内部的数据是可以修改**的。
6. final修饰的实例变量一般和static联合使用，称为**常量**

```java
public static final double PI = 3.1415926;
```

~~

**抽象类和接口以及抽象类和接口的区别** ~

1. 一个非抽象的类，继承抽象类，必须将抽象类中的抽象方法进行覆盖/重写/实现。所以，final和abstract不能联合使用，这两个关键字是对立的。

2. **抽象类有构造方法**，这个构造方法是供子类使用的。

3. 抽象类可以没有抽象方法，这样做仅仅是不让该类创建对象

4. `public abstract void doSome();`

5. java语言中凡是没有方法体的方法都是抽象方法？错

   > Object类中就有很多方法都没有方法体，都是以“;”结尾的，但他们
   > 			都不是抽象方法，例如：
   > 				public native int hashCode();
   > 				这个方法底层调用了C++写的动态链接库程序。
   > 				前面修饰符列表中没有：abstract。有一个native。表示调用JVM本地程序。



**为什么抽象类不能被实例化？**~

- 抽象类是一种**不完整的类**，它包含了一些没有实现的抽象方法，所以不能直接创建对象。
- 抽象类是一种**设计层面的概念**，它用来表示一种共性的特征，而不是具体的实例。
- 抽象类是**为了被继承而存在的**，它要求子类必须重写它的抽象方法，否则子类也会变成抽象类

抽象类就像一个模板或者一个规范，它定义了一些必须要有的功能，但是具体怎么实现这些功能，它不关心，交给子类去实现。所以你不能直接使用一个模板或者一个规范来创建对象，你需要先根据这个模板或者规范来创建一个具体的子类，然后再用这个子类来创建对象。~~

**接口基础语法**~

`[修饰符列表] interface 接口名{}` 

1. 接口中**只**允许出现**常量和抽象方法**

2. 接口是完全抽象的，没有构造方法，无法实例化。

   > ！**抽象类有构造方法**，这个构造方法是供子类使用的。

3. 一个完整的类声明的语法格式

   `public class MyClass extends Superclass implements MyInterface1,MyInterface2{}` ~~

**抽象类和接口怎么选择**~

接口和抽象类的概念不一样。接口是对动作的抽象，抽象类是对根源的抽象。抽象类表示的是，这个对象是什么。接口表示的是，这个对象能做什么。当你关注一个事物的本质的时候，用抽象类；当你关注一个操作的时候，用接口。~~

**4个访问控制权限：控制的范围是什么？**~

| 访问控制修饰符 | 本类 | 同包 | 子类 | 任意位置 |
| -------------- | ---- | ---- | ---- | -------- |
| public         | √    | √    | √    | √        |
| protected      | √    | √    | √    | ×        |
| 默认           | √    | √    | ×    | ×        |
| private        | √    | ×    | ×    | ×        |

~~

**访问控制权限修饰符可以修饰什么？**~
		属性（4个都能用）
		方法（4个都能用）
		类（public和默认能用，其它不行。）
		接口（public和默认能用，其它不行。）

## day 25

~~

**StringBuffer/StringBuilder/String 有什么区别？怎么选择？只用StringBuffer可以完全替代String吗？**~

- String是**线程安全**的字符串**常量**

> 常量不可变，这意味着它可以被缓存和共享，从而节省内存和时间。字符串常量池就是一种缓存机制，可以避免创建重复的字符串对象。具体来说：当我们写 String s = “hello”; 时，JVM会先在字符串常量池中查找是否已经存在"hello"这个字符串对象，如果存在，则直接返回它的引用给s；如果不存在，则在字符串常量池中创建一个新的"hello"对象，并返回它的引用给s。

- StringBuilder和StringBuffer都是字符串**变量**，区别在于是否线程安全。StringBuffer线程安全，StringBuilder不安全，因此性能略高。
- 只用StringBuffer不能完全替代String。有时候使用String更合适和高效。在需要一个不可变或者只需少量操作的字符串时，使用String会更合适和高效。
- 如果使用局部变量，使用StringBuilder，因为局部变量在栈里，不共享，没有线程安全问题。

~~

**包装类的写法和继承的父类**~

每个类型的包装类的构造方法的参数都有对应的基本数据类型和**String类型**

| 基本数据类型 | 包装类型  | 父类   |
| ------------ | --------- | ------ |
| byte         | Byte      | Number |
| short        | Short     | Number |
| int          | Integer   | Number |
| long         | Long      | Number |
| char         | Character | Object |
| float        | Float     | Number |
| double       | Double    | Number |
| boolean      | Boolean   | Object |

~~

**自动装箱和自动拆箱** ~

| 装箱（基本数据类型--->引用数据类型） | 自动装箱             |
| ------------------------------------ | -------------------- |
| Integer I = new Integer(123) ;       | Integer x = 123 ;    |
| Character  c = new Character('a') ;  | Character  X = 'a' ; |

 

| 拆箱（引用数据类型--->基本数据类型） | 自动拆箱                        |
| ------------------------------------ | ------------------------------- |
| int retValue = i.intValue() ;        | int y = x ;                     |
| double  retValue = d.doubleValue() ; | double  Y = new Double(13.14) ; |

~~

## day 27 异常处理机制

**异常分类**~

Object下有Throwable（可抛出的）
		Throwable下有两个分支：Error（不可处理，直接退出JVM）和Exception（可处理的）
		Exception下有两个分支：
			Exception的直接子类：编译时异常。
			RuntimeException：运行时异常。

~~

**编译时异常发生在编译阶段，对吗？** ~

不对，编译时异常和运行时异常，都是发生在**运行阶段**。编译阶段异常是不会发生的。因为异常的发生就是**new异常对象**，而程序运行阶段才可以new对象

~~

**main方法throws异常，会发生什么？**  ~

Java中异常发生之后如果一直上抛，最终抛给了main方法，main方法继续
	向上抛，**抛给了调用者JVM**，JVM知道这个异常发生，只有一个结果。**终止java程序**的执行。

~~

## day 28 集合

**集合是什么？能存储什么？** ~

集合实际上就是一个**容器**。可以来容纳其它类型的数据。集合**不能直接存储基本数据类型**，另外集合也不能直接存储java对象，
	集合中存储的是**引用**。

~~

**java中集合根据存储元素的方式分为两大类** ~

一类是**单个方式**存储元素：
			java.util.Collection;

一类是以**键值对**儿的方式存储元素
			java.util.Map;

~~

**迭代器注意事项** ~

迭代器迭代元素的过程中**不能使用**集合对象的remove方法删除元素，**要使用**迭代器Iterator的remove方法来删除元素，防止出现异常：`ConcurrentModificationException` 

~~

**为什么使用集合的remove方法会报错？** ~

迭代器实际上是集合的快照，使用集合的remove方法，会使得原型和快照不一致。但是用Iterator的remove方法来删除元素，则快照和原型都会一起删除。

~~



**ArraryList、LInkedList、vector**  初始化容量、底层是什么数据结构、扩容大小、优缺点、线程安全吗？~

### ArraryList集合

1. 当第一个元素添加进去时，初始化容量为10.
2. 底层是数组。
3. 每次扩容都扩容到原容量的1.5倍。

4. 平时使用什么集合最多？

ArrayList集合。因为往数组末尾添加元素，效率不受影响。另外，我们平时检索和查找的操作较多。

### LInkedList集合

1. 无初始化容量
2. 底层是双向链表
3. add第一个元素时，first和last都指向这第一个元素（参考源码）



### vector集合

1. 初始化容量为10。
2. 底层是数组。
3. 每次扩容都扩容到原来的两倍。
4. 线程安全但效率较低。
5. 把非线程安全的arraylist转换成线程安全的vector的方法。

`Collections.synchronizedList(myList);` 

6. Collections类是一个工具类，Collection是一个接口。

~~

## 泛型

**优缺点** ~

1. 泛型只在**编译阶段**起作用，给编译器参考，运行阶段意义不大

2. 优点

3. 1. 集合中存储的类型统一了
   2. 从集合中取出的元素是泛型指定的类型，不需要进行大量“向下转型”。

※ 注意：若是要调用子类特有方法，还是需要转型

1. 缺点：导致集合中存储的元素**缺乏多样性**

~~



**遍历Map集合的两种方式** ~

第一种方式：获取所有的key，通过遍历key，来遍历value

```java
        // 获取所有的key，所有的key是一个Set集合
        Set<Integer> keys = map.keySet();
        // 遍历key，通过key获取value
        // 迭代器可以
        Iterator<Integer> it = keys.iterator();
        while(it.hasNext()){
            // 取出其中一个key
            Integer key = it.next();
            // 通过key获取value
            String value = map.get(key);
            System.out.println(key + "=" + value);
        }
        // foreach也可以
        for(Integer key : keys){
            System.out.println(key + "=" + map.get(key));
        }


```

第二种方式：Set<Map.Entry<K,V>> entrySet()，把Map集合直接全部转换成Set集合。Set集合中元素的类型是：Map.Entry

```java
        // 以上这个方法是把Map集合直接全部转换成Set集合。
        // Set集合中元素的类型是：Map.Entry
        Set<Map.Entry<Integer,String>> set = map.entrySet();
        // 遍历Set集合，每一次取出一个Node
        // 迭代器
       Iterator<Map.Entry<Integer,String>> it2 = set.iterator();
        while(it2.hasNext()){
            Map.Entry<Integer,String> node = it2.next();
            Integer key = node.getKey();
            String value = node.getValue();
            System.out.println(key + "=" + value);
        }

        // foreach
        // 这种方式效率比较高，因为获取key和value都是直接从node对象中获取的属性值。
        // 这种方式比较适合于大数据量。
        for(Map.Entry<Integer,String> node : set){
            System.out.println(node.getKey() + "--->" + node.getValue());
        }
```

**map.put(k,v) 原理** ~

1. 先将k、v封装到Node对象当中
2. 底层调用k的hashcode方法得出hash值
3. 根据hash值计算放在哪个数组，若数组为null直接放，若有链表，则将新k与原来的k进行equals，如果TRUE就覆盖，false的就添加到链表末尾

~~

**v=map.get(k)原理** ~

1. 先计算k的hashcode，找到数组下标
2. 如果该数组没有链表，返回null
3. 如果有，那就有，对k使用equals，TRUE则返回，false则返回v

~~

**hashmap集合底层数据结构、默认初始容量、默认加载因子、扩容大小** ~

**数组+链表+红黑树**，如果哈希表单向链表中的元素超过8个，单向链表就会变成红黑树，而当红黑树上的结点数量小于6时，会变回单向链表。

默认初始化容量为16，默认加载因子为0.75，扩容到原来容量的2倍

~~



**为什么HashMap集合初始化容量必须是2的倍数** ~

HashMap的底层数据结构是数组+链表+红黑树。将元素放在table数组上面，是用hash值%数组大小定位位置，而HashMap是用hash值& (数组大小-1)，却能和前面达到一样的效果，这就得益于HashMap的大小必须是2的倍数

~~

**为什么扩容因子是0.75**~

扩容因子也叫负载因子，表示一个散列表空间的使用程度。所以负载因子越大则散列表的装填程度越高，也就是能容纳更多的元素，元素多了，链表大了，所以此时索引效率就会降低。反之，负载因子越小则链表中的数据量就越稀疏，此时会对空间造成烂费，但是此时索引效率高。

而0.75的原因和泊松分布有关。用0.75作为加载因子，每个碰撞位置的链表长度超过８个是几乎不可能的。

~~

**对自定义的类型来说，TreeSet可以排序吗**~

放到TreeSet或者TreeMap集合key部分的元素要想做到排序,包括两种方式：

​    第一种：存放在key部分的元素实现java.lang.Comparable接口。

```java
@Override
    public int compareTo(Vip v) {
        // 写排序规则，按照什么进行比较。
        if(this.age == v.age){
            // 年龄相同时按照名字排序。
            // 姓名是String类型，可以直接比。调用compareTo来完成比较。
            return this.name.compareTo(v.name);
        } else {
            // 年龄不一样
            return this.age - v.age;
        }
    }
```

​    第二种：创建TreeMap集合时通过构造方法传递一个比较器，比较器实现java.util.Comparator接口。

```java
// 给构造方法传递一个比较器。
        //TreeSet<WuGui> wuGuis = new TreeSet<>(new WuGuiComparator());

        // 大家可以使用匿名内部类的方式（这个类没有名字。直接new接口。）
        TreeSet<WuGui> wuGuis = new TreeSet<>(new Comparator<WuGui>() {
            @Override
            public int compare(WuGui o1, WuGui o2) {
                return o1.age - o2.age;
            }
        });
```



~~

**Comparable和Comparator怎么选择呢？**~

  当比较规则不会发生改变的时候，或者说当比较规则只有1个的时候，建议实现Comparable接口。

  如果比较规则有多个，并且需要多个比较规则之间频繁切换，建议使用Comparator接口。

~~

**hashmap、hashtable、concurrenthashmap的区别** ~

HashMap、HashTable和ConcurrentHashMap都是基于数组+链表的数据结构，用于存储键值对。它们的主要区别如下：

HashMap是非线程安全的，也就是说多个线程同时操作HashMap可能会导致数据不一致。HashTable和ConcurrentHashMap都是线程安全的，可以在多线程环境下使用。
HashTable使用单一锁来保护整个哈希表，这意味着每次修改数据时都需要锁住整个哈希表，效率较低。ConcurrentHashMap使用分段锁来保护哈希表的不同部分，这样可以减少锁的竞争，提高并发性能。
HashMap允许键和值为null，而HashTable和ConcurrentHashMap不允许键和值为null。
HashMap属于传统的集合框架（Collection framework），而ConcurrentHashMap属于并发的集合框架（Executor framework）。

~~

**Collections集合工具类**~
	怎么获取一个线程安全的ArrayList
	集合排序：Collections.sort(collection)

~~

**Java中有三大变量？** ~

	实例变量：在堆中。
	
	静态变量：在方法区。
	
	局部变量：在栈中。
	
	以上三大变量中：
		局部变量永远都不会存在线程安全问题。
		因为局部变量不共享。（一个线程一个栈。）
		局部变量在栈中。所以局部变量永远都不会共享。
	
	实例变量在堆中，堆只有1个。
	静态变量在方法区中，方法区只有1个。
	堆和方法区都是多线程共享的，所以可能存在线程安全问题。
	
	局部变量+常量：不会有线程安全问题。
	成员变量：可能会有线程安全问题。

~~

**以后开发中应该怎么解决线程安全问题？**~

	是一上来就选择线程同步吗？synchronized
		不是，synchronized会让程序的执行效率降低，用户体验不好。
		系统的用户吞吐量降低。用户体验差。在不得已的情况下再选择
		线程同步机制。
	
	第一种方案：尽量使用局部变量代替“实例变量和静态变量”。适用于无状态的对象
	
	第二种方案：如果必须是实例变量，那么可以考虑创建多个对象，这样
	实例变量的内存就不共享了。适用于有状态但是不需要共享状态的对象（一个线程对应1个对象，100个线程对应100个对象，
	对象不共享，就没有数据安全问题了。）
	
	第三种方案：如果不能使用局部变量，对象也不能创建多个，这个时候
	就只能选择synchronized了。线程同步机制。适用于有状态并且需要共享状态的对象

~~

**死锁代码**~

开发中synchronize不要嵌套使用

```java
package com.bjpowernode.java.deadlock;
/*
死锁代码要会写。
一般面试官要求你会写。
只有会写的，才会在以后的开发中注意这个事儿。
因为死锁很难调试。
 */
public class DeadLock {
    public static void main(String[] args) {
        Object o1 = new Object();
        Object o2 = new Object();

        // t1和t2两个线程共享o1,o2
        Thread t1 = new MyThread1(o1,o2);
        Thread t2 = new MyThread2(o1,o2);

        t1.start();
        t2.start();
    }
}

class MyThread1 extends Thread{
    Object o1;
    Object o2;
    public MyThread1(Object o1,Object o2){
        this.o1 = o1;
        this.o2 = o2;
    }
    public void run(){
        synchronized (o1){
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            synchronized (o2){

            }
        }
    }
}

class MyThread2 extends Thread {
    Object o1;
    Object o2;
    public MyThread2(Object o1,Object o2){
        this.o1 = o1;
        this.o2 = o2;
    }
    public void run(){
        synchronized (o2){
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            synchronized (o1){

            }
        }
    }
}

```

~~

**实现定时器**~

在java的类库中已经写好了一个定时器：java.util.Timer，可以直接拿来用。
				不过，这种方式在目前的开发中也很少用，因为现在有很多高级框架都是支持
				定时任务的。

	在实际的开发中，目前使用较多的是Spring框架中提供的SpringTask框架，
	这个框架只要进行简单的配置，就可以完成定时器的任务。



使用反射机制和多态有关系吗



- 什么是反射机制？有什么作用？
- 反射机制如何获取 Class 对象？
- 反射机制如何创建对象？
- 反射机制如何访问私有成员？
- 反射机制有什么优缺点？
- 什么是多态？有什么作用？
- 多态的实现方式有哪些？
- 多态的运行时绑定原理是什么？
- 多态中如何调用父类特有方法？
- private修饰的方法可以通过反射访问，那么private意义何在？
  答：首先java的private修饰符并不是为了安全性设计的，private并不是解决“安全”问题的。private想表达的不是“安全性”的意思，而是面向对象编程的封装概念，是一种编译器可以帮助我们在设计上的一个点。private的设计理念是对一个类的封装，而封装带来的好处是，在项目开发过程当中，修改一个类的private属性是不影响使用的，因为不存在对private代码的显式引用。反射技术主要是为实现一些开发工具以及框架服务。在实际的开发过程当中，我们应该尽量避免使用反射。而在使用反射时也要非常小心。
- 反射和多态的区别
  - 同为运行时获取信息，多态获取的信息仅仅在于确定方法应用所指向的实际对象。而反射在于获取一个类的所有信息。
  - 多态是一种面向对象语言的机制。而反射技术是java提供的专门用于动态获取类的信息的技术。
