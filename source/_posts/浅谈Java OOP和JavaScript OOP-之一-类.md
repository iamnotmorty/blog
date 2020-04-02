---
title: 浅谈Java OOP和JavaScript OOP 之一--类
date: 2017-11-01 19:11:59
tags: 
- Java
- JavaScript 
- OOP
categories: 
- Java 
- OOP
---
转眼间已参加学习Java有一个星期了，也把自己讨厌Java的心扭转到了热爱的路上，当然最让我着迷的还是JavaScript。慢慢的好像能写一些Java OOP的代码了，对这种纯面向对象语言的学习，一下子打通了我对JavaScript OOP的疑惑，因为互相对比学习，好像也明白了一些Java OOP的所以然。
* Java就是朝着OOP去设计的
* JavaScript可以有很多种编程方式
* 在我眼中，Java就是有很悠久历史和规矩的古典乐，而JavaScript就是多变充满激情和套路的摇滚乐。
<!-- more -->
****************************
先来看两段代码

```java
//Java
public class SuperType{
    String name;
    int age;
    public SuperType(String name, int age){
        this.name = name;
        this.age = age;
    }
    public sayName(){
        System.out.println("Hello, " + this.name);
    }
}
```
```javascript
//JavaScript（ES6）
class SuperType{
    constructor(name, age){
        this.name = name;
        this.age = age;
    }
    sayName(){
        console.log("Hello, " + this.name);
    }
}
```
分析一下，由于JavaScript的ES6内置了classes，所以写出来跟Java视觉上的差距就是只剩下几个关键字的不同了，Java要声明类型，JavaScript不需要，这几乎就是这两门语言的很关键的不同，也决定了Java是“强”，JavaScript是“弱”。
Java里有一个SuperType（）构造方法和sayName()普通方法，JavaScript里有构造函数和sayName（）方法，这里JavaScript里与Java不同的地方：
* JavaScript里方法也都是函数（Function）
* Java的方法定义在类里面
* JavaScript的方法看似是定义在“类”里面，实则还是
```javascript 
SuperType.prototype.sayName = function(){...}
```
所以JavaScript的类只是ES6的语法糖，内部实现还是基于原型的，这里的类也不是真的类，到底还是一个构造函数。