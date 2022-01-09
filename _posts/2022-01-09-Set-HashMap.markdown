---
layout: post
title:  " HashSet、HashMap "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-01-09 22:29:26 +0800
tags: [Java, HashSet, HashMap]
---

#### Set：

- 无序（添加和取出的顺序不一样），没有索引。
- 没有重复元素，所以最多包含一个null
- 和Collection接口一样，

```java
        Set set = new HashSet();
        set.add("hello");
        set.add("world");
        set.add("my");
        set.add("girl");
        // Set接口存放对象是无序的，添加和取出的顺序不一致
        // 取出一次后顺序都是一致的，不会一次一变
        System.out.println(set);

        // 遍历 1.使用迭代器   因为是Collection的接口
        Iterator iterator = set.iterator();
        while (iterator.hasNext()) {
            Object next = iterator.next();
            System.out.println(next);
        }
        // 增强for循环：
        for (Object o : set){
            System.out.println(o);
        }
        // Set 对象不能通过索引来获取
```



HashSet ：实现了Set接口：底层实际上是HashMap: 源码如下：

```java
    public HashSet() {
        map = new HashMap<>();
    }
```

```java
        Set hashSet = new HashSet();
        hashSet.add(null);
        hashSet.add(null);
        //只能存放一个null  即元素不能重复
        System.out.println(hashSet);// [null]
        
        // 不保证存放元素的顺序和取出的顺序一致。不能有重复的元素
```

再执行add方法后，会返回一个`boolean`，成功返回`true`，失败返回`false`

```java
        HashSet set = new HashSet();
        // HashSet 不能添加相同的元素/对象
        set.add("lucy");
        set.add("lucy");//添加不了
        set.add(new Dog("tom"));
        set.add(new Dog("tom"));//可以添加
        System.out.println(set);
        set.add(new String("belle"));
        set.add(new String("belle"));//添加不了
        System.out.println(set);
    }
}
class Dog{
    private String name;

    public Dog(String name) {
        this.name = name;
    }
    @Override
    public String toString() {
        return name;
    }
}
```



#### HashSet底层机制说明：

**HashMap底层为（数组+链表+红黑树）**：

```java
HashSet hashSet = new HashSet();
hashSet.add("java");
hashSet.add("java");
```
以这个为例：按照步骤进行：

```java
public HashSet() {//构造器
    map = new HashMap<>();
}
```

```java
public boolean add(E e) { //
    return map.put(e, PRESENT)==null;// PRESENT 静态的Object()
}
```

```java
public V put(K key, V value) {//key :java   value: PRESENT
    return putVal(hash(key), key, value, false, true);
}
```

注意：以上的hash(key) 方法会得到key对应的hash值（并不完全等价于`hashcode`）

hash(key) 方法:`(key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);`

![](\assets\img\blogpic\HashMap.jpg)

至此，java添加完毕。
