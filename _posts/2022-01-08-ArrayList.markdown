---
layout: post
title:  " List、ArrayList "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-01-08 15:05:26 +0800
tags: [Java, ArrayList]
---

#### List接口和常用方法：

添加元素：

```java
list.add(1,"hi");// 在index = 1 的位置加上一个"hi"
```

获取指定index位置的元素：

```java
Object result = list.get(2);
```

返回指定元素在集合中首次，最后一次出现的位置：

```java
System.out.println(list.indexOf("hello"));
System.out.println(list.lastIndexOf("hello"));
```

移除指定index位置的元素，并返回此元素：

```java
list.remove(0);
System.out.println(list);
```

设置指定index位置的元素为某个元素，相当于替换：

```java
list.set(1,"hi");
System.out.println(list);
```

返回从`fromIndex` 到`toIndex`位置的子集和： 包含前一个元素，不包含后一个元素：

```java
List list = list.subList(1,3);
System.out.println(list);
```

List的三种遍历：迭代器，增强for，普通for

#### ArrayList底层结构和源码分析※※※：

- permits all elements，including null，ArrayList 可以加入null，并且多个

- ArrayList 是由数组来实现数据存储的，

- Arraylist基本等同于Vector，除了ArrayList是线程不安全（效率高）

  在多线程的情况下不建议使用ArrayList

##### ArrayList的底层操作机制原码分析：

1. ArrayList中维护了一个Object类型的数组elementData，`transient Object[] elementData;`

   `transient`  **表示瞬间短暂，该属性不会被序列化**

2. 当初次创建ArrayList对象时，如果使用的时无参构造器，则初始`elementData`容量为0，加入一个元素，扩容为10，后续按照1.5倍进行扩容。`（10，15，22，33.....)`；**如果时指定大小的构造器，扩容的话直接以1.5倍扩容。**

3. ```java
           ArrayList list = new ArrayList();
           for (int i = 0; i < 10; i++) {
               list.add(i);
           }
           for (int i = 11; i < 15; i++) {
               list.add(i);
           }
   ```

   ```java
   public ArrayList() {
           this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
   ```

   创建了一个无参构造器，`elementData数组 = {}`,然后进行下步骤执行`list.add`：

   ```java
       public boolean add(E e) {
           modCount++;//记录被修改的次数
           add(e, elementData, size);//该步骤源码在下方
           return true;
       }
   ```

   ```java
       private void add(E e, Object[] elementData, int s) {
           if (s == elementData.length)// 此处s = 10,进入扩容
               elementData = grow();//该步骤源码在下方
           elementData[s] = e;
           size = s + 1;
       }
   ```

   先确定是否需要扩容，再执行赋值，不够进行去扩容，以下为扩容机制：

   ```java
       private Object[] grow(int minCapacity) {
           int oldCapacity = elementData.length;
           if (oldCapacity > 0 || elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
               int newCapacity = ArraysSupport.newLength(oldCapacity,
                       minCapacity - oldCapacity, /* minimum growth */
                       oldCapacity >> 1           /* preferred growth */);
               return elementData = Arrays.copyOf(elementData, newCapacity);
           } else {
               return elementData = new Object[Math.max(DEFAULT_CAPACITY, minCapacity)];
           }
       }
   ```

   `ArraysSupport.newLength(oldCapacity, minCapacity - oldCapacity,  oldCapacity >> 1  );`

   此处的`newLength(int oldLength, int minGrowth, int prefGrowth)`方法返回一个

   `hugeLength(oldLength, minGrowth);` 该方法时判断要么返回一个`SOFT_MAX_ARRAY_LENGTH`  要么返回

   `minLength = oldLength + minGrowth`  谁大返回谁。

   综上：`newLength()`返回了一个旧的值（10）+（1和5中的最大值即5），所以变成了15.

   `oldCapacity >> 1`   变为原来的一半，原来为10，现在为5

   使用扩容机制来确定要扩大到多少，第一次扩容出来为`newCapacity` 为10.第二次及其以后采用1.5倍扩容。

   扩容时采用  `elementData = Arrays.copyOf(elementData, newCapacity);` copyof  可以保留之前的数据



















































