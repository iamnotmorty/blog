---
title: 浅谈Java OOP和JavaScript OOP 之三--封装
date: 2017-11-21 13:11:09
tags: 
- Java
- JavaScript 
- OOP
categories: 
- Java 
- OOP
---
封装是面向对象程序语言对客观世界的模拟，在客观世界中对象的成员变量(用于描述对象的状态数据)都被隐藏在对象内部，外部无法直接操作和修改。
对一个类或对象实现良好的封装，可以实现以下目的。
> - 隐藏类的实现细节
> - 让使用者只能通过事先预定的方法来访问数据，从而可以在该方法里加入控制逻辑，限制对成员变量的不合理访问。
> - 可进行数据检查，从而有利于保证对象信息的完整性。
> - 便于修改，提高代码的可维护性。
<!-- more -->
为了实现良好的封装，需要考虑两方面。
> - 将对象的成员变量和实现细节隐藏起来，不允许外部直接访问。
> - 把方法暴露出来，让方法来控制对这些成员变量进行安全的访问操作。

Java的封装
```java
public class Person1{
  private String name;
  private int age;
  private String food = "什么都吃";
  public void setName(String name){
    this.name = name;
  }
  public String getName(){
    return name;
  }
  public void setAge(int age){
    this.age = age;
  }
  public int getAge(){
    return age;
  }
  public void eat(){
    System.out.println(food);
  }
}
```
一个基本的Java封装，用setter和getter方法操控私有属性。也乐意用构造方法，可以直接传递参数生成实例对象。
*************************
JavaScript的封装
```javascript
//对象的定义
var person1 = {
  food: "什么都吃"
};
person1.name = "Jack";
person1.age = 25;
console.log(person1.name);//"Jack"
```
person1.name相当于执行了setter和getter的操作,要创建不同的对象要一次次来，没有可复用性可言。
```javascript
//通过函数调用返回对象
function Person(name, age){
  return {
    name: name,
    age: age,
    food: "什么都吃"
    //eat: function(){
    //  console.log(this.food);
    //}
  }
}
var person1 = Person("a", 28);
var person2 = Person("b", 22);
```
两个对象之间没有什么内在联系

```javascript
//构造函数的封装
function Person(name, age){
  this.name = name;
  this.age = age;
  this.food = "什么都吃";
  this.eat = funciton(){
    console.log(this.food);
  }
}
var person1 =new Person("a", 28);
var person2 =new Person("b", 22);
```
person1和person2都是Person的实例对象，分别都创建了自己的实例属性，但是两个对象都是Person的实例，他们本来就是“什么都吃”，但是这种方式在实例对象里都有这个属性,甚至创建更多实例对象就会有更多的一样的food属性生成，造成内存的浪费。(eat方法也是一样的的重复创建)

```javascript
//prototype模式
//把共有属性和方法添加到原型上
function Person(name, age){
  this.name = name;
  this.age = age;
}
Person.prototype.food = "什么都吃";
Person.prototype.eat = function(){
  console.log(this.food);
}
var person1 = new Person("a", 28);
···
var personN 
```
这里不管创建多少个不相同的Person实例对象，他们都是公用food属性和eat方法，同时也体现了实例之间的关系。就像一个机器，可以不断生产产品，但是编号和出场时间不同。