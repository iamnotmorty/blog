---
title: 浅谈Java OOP和JavaScript OOP 之四--多态(上-Java)
date: 2017-11-22 15:51:02
tags: 
- Java
- JavaScript 
- OOP
categories: 
- Java 
- OOP
---
# 什么是多态？
> 多态：同一操作作用于不同的对象，可以有不同的解释，产生不同的执行结果

这是百度百科的一个解释

> 多态意味着“多种形态”。在面向对象的编程当中，你有相同的’脸’（基类里一种通用的接口），以及使用该接口的不同的形式：即各种不同的动态绑定方法的版本。

这是Java编程思想里的解释，由此可见，在面向对象编程中多态必须是动态绑定实现的，注意关键词“动态绑定”。这样就直接否定了Java里方法重载是多态的一种实现形式。因为，方法重载是在编译时发生的，也就是“静态绑定”的，所以将不再纠结重载是否是多态的问题。

<!-- more -->
****************************************************
Java中的多态
```java
//父类
public class People {
	public People() {}
	public void revolt() {
		System.out.println("revolt");
	}
}
//子类
public class Women extends People {
	public Women() {}
	//@Override
	public void revolt() {
		System.out.println("给你一巴掌");
	}
}
public class Man extends People {
	public Man() {}
	//@Override
	public void revolt() {
		System.out.println("给你一拳头");
	}
}
//测试类
public class You {
	//你有一个打人的方法
	public static void play(People people) {
		people.revolt();
	}
	public static void main(String[] args) {
		play(new Women());
		play(new Man());
	}
}
```
理解：有一天我叫你去打指定的人，然后想看看不同的人有什么不同的反应。这里的重写就是人被你打了都会反抗，但是不同性别的人有不同的反抗。而你事先并不知道那个人会怎么反抗，我指这个你就打这个，我指那个你就打那个。由此引出多态的三要素
* 首先被你打的都是人--继承（或接口实现）
* 不同的人有不同的反抗方式，但是都会反抗--重写（动态绑定子类方法）
* 父类引用指向子类对象（向上转型）

父类引用指向子类对象（向上转型），这个的作用可以实现如You类里的。不用再为不同的子类型去写相同的执行方式。比如：
```java
public class You {
	//你有一个打人的方法
  //这个方法只能打Women
	public static void play(Women people) {
		people.revolt();
	}
  //要打Man只能重新定义方法
  public static void play(Man people) {
		people.revolt();
	}
  //这样每打一个人就要写一个打一个人的方法，有很多人的时候就很麻烦了
	public static void main(String[] args) {
		play(new Women());
		play(new Man());
	}
}
```
多态的作用
> 消除数据类型之间的耦合，增加代码的可读性和可扩展性，改善代码的组织结构

理解：有新的子类（人）要添加时，只需重写方法就可以，不会影响其他子类，（你去打的时候）也不用创建新的打人方法，直接去打就可以了。用接口实现道理相同，实现类去实现接口的抽象方法。然后将接口类型引用指向实现类的对象。
* 这样就体现出向上转型为编程带来的便捷性。

但是这样的方式也有一个致命的缺点，比如，人都是会反抗的合乎常理。这里有个酒喝得不省人事的人，他不会反抗，也就是说他有个不反抗的方法，那用向上转型是没法调用的。这里会用到向下转型。先看段代码：
```java
public class Drunkard extends People {
	//这是一个喝醉的人
	public Drunkard() {}
		
	//喝多了不还手的方法
	public void revoltNot() {
		System.out.println("我喝多了，不还手")；
	}
}
//你去打他
public class You {
	//你有一个打人的方法
	public static void play(People people) {
		people.revolt();
	}
	public static void main(String[] args) {
		play(new Women());
		play(new Man());
		play(new Drunkard());
	}
}
//这样子的话他还是会调用继承过来的反抗方法，那要调用他喝醉的方法就要把他灌醉，变成喝醉的人。
public class You {
	//你有一个打喝醉的人的方法
	public static void playDrinking(People people) {
		((Drunkard)people).revolt();
	}
	public static void main(String[] args) {
		playDrinking(new Drunkard());
	}
}
```
写到这里发现向下转型用这个例子描述不是特别恰当，下次将重新整理一篇有关类型转换的文章。