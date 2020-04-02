---
title: 浅谈Java OOP和JavaScript OOP-之四--多态(下--JavaScript)
date: 2017-11-24 16:03:07
tags: 
- Java
- JavaScript 
- OOP
categories: 
- Java 
- OOP
---
在Java的多态里已经讲过多态的基本概念，这里不再赘述。
*******
由于语言特性的问题，JavaScript多态的实现机制不同于Java，这和JavaScript没有类的，所以略有不同。
<!-- more -->
```javascript
//先用一个不需要继承实现的多态
function sayAge(object) {
  if(object instanceof Child){
      console.log('10');
  }else if(object instanceof Parent){
      console.log('30');
  }
}

sayAge(child);   // '10'
sayAge(parent);  // '30'
```
这是一个在知乎看到的例子，满足了多态的基本定义。同一操作作用于不同的对象，可以有不同的解释，产生不同的执行结果。
**********************************
涉及到继承，直接拿ES6来做对比
```javascript
class People{
  revolt(){
    console.log("revolt");
  }
}

class Man extends People{
  revolt(){
    console.log("给你一拳")；
  }
}

class Woman extends People{
  revolt(){
    console.log("给你一巴掌")；
  }
}
```
