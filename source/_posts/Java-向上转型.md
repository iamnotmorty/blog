---
title: Java--向上转型
date: 2017-11-27 19:51:20
tags: 
- Java
- OOP
categories: 
- Java
- OOP
---
要把OOP思想彻底了然于心，还有很长的路要走。今天被问了一个问题，用语言来描述就是，用向上转型和不用向上转型有什么区别。  
具体代码如下：
```java
//使用向上转型
EmployeeDao employeeDao = new EmployeeDaoImpl();
//不使用向上转型
EmployeeDaoImpl employeeDao1 = new EmployeeDaoImpl();
```
<!-- more -->
两者唯一的区别就是，前者用接口类型对象指向实现类实例对象的引用，后者类型同意，创建一个实现类的实例对象。  
那这两种写法到底有什么区别呢？  

设想一个场景，我们要调用很多employeeDao1对象的方法，于是就会写大量的
```java
employeeDao1.getByName();
employeeDao1.getByID();
employeeDao1.add();
employeeDao1.delete();
employeeDao1.update();
employeeDao1.show();
.....
```
此时我们的老板突然说我们数据库不用MySQL，改用Oracle，这个时候我们就要修改实现类里面实现方法的代码。当然有点基础的人都知道，如果不涉及方法的增加和改变，并且EmployeeDao接口里面一般是不做改变的。但是会发现在这种情况下，好像也并不用修改很多。那么来看，这时候针对不同的客户要求，有家要MySQL，有家要Oracle。就要新增加一个针对Oracle的实现类，比如叫EmployeeDaoImplForO
```java
EmployeeDaoImplForO edifo = new EmployeeDaoImplForO();
//这个时候就要修改之前的所有的employeeDao1为edifo，修改量非常的大。
//但是使用接口类型去调用方法则完全不同，这样就不需要去关注实现类了，直接拿过来new一个实现类给接口类型对象，下面的代码就不需要修改。
```
不过这种思想在这个例子下不是特别明显，因为可以把对象名称改成一样的，也不需要修改很多代码。但是这里的问题根本还是OOP思想，这样就不符合这个思想，若是遇到有些情况，这样便行不通了。不过想来想去还是有点硌着，心里还不是很明白，这就有待于代码量的增加和项目经验的增加慢慢的去了然于心，先记住这样用更好。遇到面试可以用反例去说给面试官听。

