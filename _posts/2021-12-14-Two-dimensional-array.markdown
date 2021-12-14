---
layout: post
title:  "冒泡排序、二维数组"
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2021-12-14 10:54:26 +0800
tags: [Java, 冒泡排序, 二维数组]
---

#### 冒泡排序（Bubblesort）：

冒泡排序（bubblesort）：我们将五个无序的：24，69，80，57，13使用冒泡排序将其变为一个从小到大的有序数列。

1.我们一共有5个元素，进行了4轮排序，可以看成是外层循环。

2.每一层可以确定一个数的位置，第一次确定最大数，第二次确定第二大的数。

3.当进行比较时，如果后边的数大于前边的数，那么就进行交换

4.每轮比较的次数在减少，有四次>三次>二次>一次。

代码实现如下：  //如果前边的数大于后边的数，前边的数时`array[j]` 后边的数时`array[j+1]`

```java
public class BubbleTest{
	public static void main(String[] args){
		int[] array = {24,69,80,57,13};
		int temp = 0;
		for(int i = 0;i < 4;i++){
			for(int j = 0;j < 4 -i;j++){
			    if(array[j] > array[j+1]){
					temp = array[j];
					array[j] = array[j+1];
					array[j+1] = temp;
			    }
		    }
	    }
	    for(int i = 0;i < array.length;i++){
			System.out.print(array[i]+" ");
        }
	}
}
```

如果数组的个数不固定，那么代码如下：

```java
public class BubbleSort{
	public static void main(String[] args){
		int[] array = {24,69,80,57,13};
        for(int i = 0;i < array.length-1;i++){
            for(int j = 0;j < array.length - 1 - i;j++){
              if(array[j] < array[j+1]){
	                int temp = array[j]; 
					array[j] = array[j+1];
					array[j+1] = temp;
				}
			}
		}
		for(int i = 0;i < array.length;i++){
			System.out.print(array[i]+" ");
		}
	}
}
```



#### 顺序查找：

```java
import java.util.Scanner;
public class SeqSearch{
	public static void main(String[] args){
		String[] array = {"bai","qing","jinx","zwji"};
		Scanner myScanner = new Scanner(System.in);
		String findname = myScanner.next();
		int index = -1;
        //我们在此定义一个标识符，如果查找到了那么我们就将i的值赋给index
        //如果没有找到，我们通过index == 1，来判断没有找到
		for(int i = 0;i < array.length-1;i++){
			if(findname.equals(array[i])){
        //findname.equals(array[i])用来判断字符串String是否相等。
				System.out.println("founded !"+i);
				index = i;
				break;
			}//并不能在此处加else说没有找到，因为没有循环完毕
		}//需要所有的都循环完毕后在来判断是否已经找到。
		if(index == -1){
			System.out.println("cant found");
		}		
	}
}
```

#### 二维数组：

一维数组的每个元素又是一个数组，所以称为二维数组。

`//如果我们想要得到第(i)个一维数组的第(j)个值，表示的是array[i-1][j-1]`

#### 二维数组的使用：

1.动态初始化：`int a [] [] = new int [2] [3]`  表示二维数组有两个一维数组，每个一维数组有三个元素。

内存的存在形式：存放的时一维数组的内存地址，采用引用方式。

2.先声明：`int [] [] ,array = new int[2][3]` 在使用。

3.动态初始化：列数不确定，解释如下：

```java
public class TwoDim{
	public static void main(String[] args){
		int array[][] = new int[3][];
      //创建二维数组，只确定一维数组的个数
		for(int i = 0;i < array.length;i++){
			array[i] = new int[i + 1];
          //给每个一维数组开数据空间，如果没有new，那么返回的一维数组就是null
          //给每个一维数组的每个元素赋值
			for(int j = 0;j < array[i].length;j++){				
				array[i][j] = i + 1;
			}
		}
	}
}
```

4.静态初始化：前述的即为静态初始化。

练习：

杨辉三角：中间每一个元素等于上方元素和上方左边元素之和。

```java
public class YangHui{
	public static void main(String[] args){
		int array[][] = new int[10][];
		for(int i = 0;i < array.length;i++){
			array[i] = new int[i+1];
			for(int j = 0;j < array[i].length;j++){				
				if(j == 0||j == array[i].length-1){
					array[i][j] = 1;
				}else{
					array[i][j] = array[i-1][j] +array[i-1][j-1];
				}//中间每一个元素等于上方元素和上方左边元素之和
			}			
		}
		for(int i = 0;i < array.length;i++){
			for(int j = 0;j < array[i].length;j++){
				System.out.print(array[i][j]+"\t");
			}		
		System.out.println();
	    }
	}
}
```

