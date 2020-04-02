---
title: Java Collection API源码分析之ArrayList
date: 2019-06-16 01:29:12
tags: - Java
---
# ArrayList源码分析-JDK1.8
## 通常要先来一个类图，但是我觉得这些继承和实现关系，能打开源码看看就清清楚楚了，何必多此一举，更何况idea还能直接生成类图。
## 看源码的方式呢，我们都从平常使用的角度入手
<!-- more -->
### 好，先来第一段代码
```java
List<E> list = new ArrayList<>();
```
调用了ArrayList的无参构造方法。来看看这个方法长什么样
```java
public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}
```
这个elementData和DEFAULTCAPACITY_EMPTY_ELEMENTDATA是个什么东西呢。
```java
//用来存放数据或者说元素的内部数组
transient Object[] elementData;
//空数组
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
//elementData默认数组大小为10
private static final int DEFAULT_CAPACITY = 10;
```
调用无参构造方法的时候有一个很不错的思想，就是再第一次添加元素的时候才会进行数组容量的分配，不然就是一个空数组。说到添加元素，那就一起来看看add方法。
```java
public boolean add(E e) {
    //确保内部数组容量符合插入一个元素的条件，数组是不可变的，所以当容量不够的时候就要进行扩容的操作。
    ensureCapacityInternal(size + 1);
    //数组下标index是从0-（size-1），所以新加入的元素的index就是size，然后元素加入后，就多了一个元素，size=size+1。这里注意size++和++size的区别
    elementData[size++] = e;
    return true;
}

private void ensureCapacityInternal(int minCapacity) {
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
    }
    ensureExplicitCapacity(minCapacity);
}

private void ensureExplicitCapacity(int minCapacity) {
    modCount++;
    if (minCapacity - elementData.length > 0) 
        grow(minCapacity);
}

private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;

private void grow(int minCapacity) {
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    if (newCapacity - minCapacity > 0) {
        newCapacity = minCapacity;
    }
    //MAX_ARRAY_SIZE是ArrayList里定义的最大容量
    if (newCapacity - MAX_ARRAY_SIZE  > 0) {
        newCapacity = hugaCapacity(minCapacity);
    }
    elementData = Arrays.copyOf(elementData, newCapacity);
}

private static int hugaCapacity(int minCapacity) {
    if(minCapacity < 0) {
        throw new OutOfMemoryError();
    }
    return (minCapacity > MAX_ARRAY_SIZE) ? Integer.MAX_VALUE : MAX_ARRAY_SIZE;
}
```
个人认为整个数据结构，最关键的就是这个扩容机制。因为数组是不可变的，所以封装成ArrayList以方便项目中的使用。
