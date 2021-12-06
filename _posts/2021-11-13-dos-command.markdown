---
layout: post
title:  "基本数据类型"
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2021-11-14 10:23:01 +0800
tags: [Java, Java AP]
---

#### Java相关问题的回答：

- 问JDK、JRE和JVM是什么关系？

答：JDK是JRE+Java开发工具的统称，全写是<u>Java Development Kit</u>

​       JRE是JVM+Java SE核心类库；JVM是Java虚拟机

- 问环境变量path配置及其作用？


答：环境变量的作用是为了在DOS的任意位置目录使用Java和javac程序、

#### 基本数据类型：

###### 数值型：
​               1.整数类型（`byte[1]`字节，`short[2]`短整型，`int[4]`整型，`long[8]`长整型）

​               2.浮点（小数）类型（`float[4]`，`double[8]`)

字符型（`char[2]`），存放单个字符'a'；

布尔类型（`boolean[1]`），存放`true`，`false`；

###### 引用数据类：

​                       1.类（`class`）

​                       2.接口类型（`interface`）

​                      3.数组[]

bit是计算机的最小储存单位，1byte = 8bit

###### 浮点类型：

|    类型     | 占用储存空间 | 范围                 |
| :---------: | ------------ | -------------------- |
| 单精度float | 4字节        | -3.403E38~3.403E38   |
| 双精度float | 8字节        | -1.798E308~1.798E308 |

浮点数 = 符号位 + 指数位 + 尾数位

尾数部分可能丢失，造成精度损失（小数都是近似值）

当我们对运算结果为小数的进行判断时，要小心

```
double num1 = 2.7   // 该结果为2.7
double num2 = 8.1 / 3  //该结果是一个接近2.7的小数（2.6666666666）
//二者之差非常小，小到我们规定的精度时，我们认为两者时相同的
```

#### 字符类型（char）:

char类型可以直接存放一个数字或者字符，字符常量用单引号括起来，`char c1 = 'a'`~~不允许双引号~~

char的本质是一个数字，默认输出在对应Unicode码对应的字符。

```
char c1 = 97
System.out.println(c1) //  输出的时97对应的a
```

char字符时可以参与运算的，相当于对应的一个整数，每个整数对应的都有Unicode码。

```
System.out.println('a' + 10)  //该步骤先把a对应的数字97，然后加上10，输出107
char = 'b' + 1
System.out.println((int)char)  //输出的结果为99
System.out.println(char)   //输出的结果为c
```

计算机储存a，首先找到编码97，在转化为二进制，在储存起来。

#### Java API文档

Java应用程序编程接口，查阅https://www.matools.com中文文档

包-> 类->方法 来进行查找，也可以直接进行搜索。





