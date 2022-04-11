---
layout: post
title:  " 数据结构与算法"
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-04-11 20:22:26 +0800
tags: [Java, 数据结构与算法]
---

# 数据结构

线性结构包括：数组、队列、链表、栈

非线性结构包括：二维数组、多维数组、广义表、**树结构**、**图结构**

## 线性结构：数组

稀疏数组：当一个数组中的大部分元素为0或同一个值的数组时，可以使用稀疏数组来保存该数组。 

​		具体做法：首先记录该数组一共有几行几列，有多少个不同的值，再把不同的值的元素的行列和值记录到一个更小规模的数组中，从而缩小程序的规模。

代码实现：

```java
public class SparseArray {
    public static void main(String[] args) {
        int chessArr1[][] = new int[11][11];
        chessArr1[1][2] = 1;
        chessArr1[2][3] = 2;
        for (int[] ints : chessArr1) {
            for (int anInt : ints) {
                System.out.print(anInt);
            }
            System.out.println();
        }
        //    转换为稀疏数组
        int sum = 0;
        for (int i = 0; i < 11; i++) {
            for (int j = 0; j < 11; j++) {
                if(chessArr1[i][j]!= 0){
                    sum++;
                }
            }
        }
        int sparseArr[][] = new int[sum+1][3];
        sparseArr[0][0] = 11;
        sparseArr[0][1] = 11;
        sparseArr[0][2] = sum;

        int count = 0;
        for (int i = 0; i < 11; i++) {
            for (int j = 0; j < 11; j++) {
                if(chessArr1[i][j]!= 0){
                    count++;
                    sparseArr[count][0] = i;
                    sparseArr[count][1] = j;
                    sparseArr[count][2] = chessArr1[i][j];

                }
            }
        }
        System.out.println("稀疏数组如下：");

        for (int i = 0; i < sparseArr.length; i++) {
            System.out.printf("%d\t%d\t%d\t\n",sparseArr[i][0],
                    sparseArr[i][1],sparseArr[i][2]);
        }
        System.out.println();
        System.out.println("恢复为二维数组");

        int chessArr2[][] = new int[sparseArr[0][0]][sparseArr[0][1]];
        for (int i = 1; i < sparseArr.length; i++) {
            chessArr2[sparseArr[i][0]][sparseArr[i][1]] = sparseArr[i][2];
        }
        for (int[] ints : chessArr2) {
            for (int anInt : ints) {
                System.out.printf("%d\t",anInt);
            }
            System.out.println();
        }
    }
}

```

## 线性结构：队列

队列是一个有序列表，可以由数组或者链表来实现  **列表遵循先入先出的原则**





