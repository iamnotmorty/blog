---
title: 浅谈Java OOP和JavaScript OOP 之二--继承
date: 2017-11-20 15:37:11
tags: 
- Java
- JavaScript 
- OOP
categories: 
- Java 
- OOP
---
* Java继承：用extends关键字，实现SubType继承SuperType
* JavaScript继承：在ES6之前，没有extends关键字，主要靠几种模式实现继承（比如：构造函数继承和原型继承还有组合继承等）。ES6之后可以像Java一样通过extends关键字，不过底层实现还是原型继承。
<!-- more -->
*************************************
Java继承
```java
//Java
//父类
class SuperType {
  String name;
  int age;
  String job;
  String[] friends = {"Jack", "Mack", "Salle"}
  SuperType(String name, int age, String job){
    this.name = name;
    this.age = age;
    this.job = job;
  }
  void getPeople(){
    System.out.println(this.name + " is " + this.age + " years old" + " and he is a " + this.job);
  }
  void getFriends(){
    System.out.println(this.friends);
  }
}

//子类
class SubType extends SuperType {
  SubType(String name, int age, String job) {
    super(name, age, job);
  }
  @override
  //void getPeople(){}
}

//Test
class Test {
  public static void main(String[] args){
    SubType people = new SubType("Nicholas", 38, "Software Engineer");
    people.getPeople();
    //Nicholas is 38 years old and he is a Software Engineer
    people.getFriends();
    //["Jack", "Mack", "Salle"]
  }
}
```

JavaScrpt继承
* 原型链继承
```javascript 
function SuperType (name, age) {
    this.name = name;
    this.age = age;
    this.friends = ["a", "b"];
}
SuperType.prototype.say = function(){
    console.log('hello, my name is ' + this.name);
};
SuperType.prototype.sayFriends = function(){
    console.log(this.friends);
};
function SubType() {
}
SubType.prototype = new SuperType('Nicholas');
var man1 = new SubType();
man1.friends.push("c");
man1.say(); //hello, my name is Nicholas
man1.sayFriends();//["a", "b", "c"]
var man2 = new SubType();
man2.sayFriends();//["a", "b", "c"]
console.log(man1.say === man2.say);//true
console.log(man1.name === man2.name);//true
```
事实上这里的man1和man2都是空对象，属性都是顺着原型链往上找到SubType上的name和say方法。由此也导致了实例属性共享，任何一个实例改变属性，都会影响其他实例。所以这种继承并没有很大的作用。
* 利用构造函数继承
```javascript
function SuperType (name, age) {
    this.name = name;
    this.age = age;
}
SuperType.prototype.say = function(){
    console.log('hello, my name is ' + this.name);
};
function SubType(name, age) {
    SuperType.apply(this, arguments);
}
//SubType.prototype = new SuperType('Nicholas');
var man1 = new SubType('joe');
var man2 = new SubType('david');
console.log(man1.name === man2.name);//false
man1.say(); //say is not a function
```
这里继承的属性，非常的优雅，但是却无法继承父类型的原型方法。由此就引出组合继承
* 组合继承
```javascript
function SuperType (name, age) {
    this.name = name;
   this.age = age;
}
SuperType.prototype.say = function(){
    console.log('hello, my name is ' + this.name);
};
function SubType(name, age) {
    SuperType.apply(this, arguments);
}
SubType.prototype = new SuperType();
var man1 = new SubType('joe');
var man2 = new SubType('david');
console.log(man1.name === man2.name);//false
console.log(man1.say === man2.say);//true
man1.say(); //hello, my name is joe
```
这里虽然具备了两种继承特点，但是重复的“继承”了实例属性,性能上有缺陷。并且在应对引用类型实例属性时，还是会共享实例属性

* 寄生组合继承
```javascript
function SuperType (name, age) {
            this.name = name;
            this.age = age;
        }
SuperType.prototype.say = function(){
    console.log('hello, my name is ' + this.name);
};
function SubType(name, age) {
    SuperType.apply(this, arguments);
}
SubType.prototype = Object.create(SuperType.prototype);//a.
SubType.prototype.constructor = SubType;//b.
var man1 = new SubType('pursue');
var man2 = new SubType('joe');
console.log(man1.say == man2.say);
console.log(man1.name == man2.name);
```
a.创建一个SuperType的原型的副本，并让SubType的原型指向它。由于是一个副本，所以不会向SuperType的应用类型传递，解决了实力属性共享的问题。
b.a这一步操作重写了原型，出于严谨或者避免一些特殊的情况，重新指定构造函数
```javascript
//a相当于
function create(obj){
    function T(){};
    T.prototype = obj;
    return new T();
}
```
* ES6 class继承
```javascript 
class SuperType{
    constructor(name,age){
        this.name = name;
        this.age = age;
    }
    say(){
        console.log('hello, my name is ' + this.name);
    }
}

class SubType extends SuperType{
    constructor(name, age, job){
        super(name, age);
        this.job = job;
    }
}
```
************************************
所以说，Java的继承更加存粹一些“类-继承”，而JavaScript就算有了class，那也是基于原型的继承模式，从《你不知道的JavaScript》里，作者给出了一个更合理的说法，这是一种委托。