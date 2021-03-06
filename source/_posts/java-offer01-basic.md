---
title: Java面试题-基础
date: 2019-09-30 15:09:18
tags:
- Java
categories:
- Java面试题
index_img: https://s2.ax1x.com/2019/09/30/uYwpDS.jpg
---
第一部分，Java基础题目，共17粒。附带常量池和堆内存简介。
<!--more-->
## Java基础
### 1.JDK和JRE有什么区别？
* JDK：Java Development Kit 的简称，java 开发工具包，提供了 java 的开发环境和运行环境。
* JRE：Java Runtime Environment 的简称，java 运行环境，为 java 的运行提供了所需环境。

具体来说 JDK 其实包含了 JRE，同时还包含了编译 java 源码的编译器 javac，还包含了很多 java 程序调试和分析的工具。简单来说：如果你需要运行 java 程序，只需安装 JRE 就可以了，如果你需要编写 java 程序，需要安装 JDK。

### 2.== 和 equals 的区别是什么？
==：
对于基本类型和引用类型 == 的作用效果是不同的。
* 基本类型：比较的是值是否相同；
* 引用类型：比较的是引用是否相同；

equals：
equals 本质上就是 ==，只不过 String 和 Integer 等重写了 equals 方法，把它变成了值比较。

### 3.两个对象的 hashCode()相同，则 equals()也一定为 true，对吗？
不对，两个对象的 hashCode()相同，equals()不一定 true。例如在散列表中，hashCode()相等即两个键值对的哈希值相等，然而哈希值相等，并不一定能得出键值对相等。

### 4.final 在 java 中有什么作用？
* final 修饰的类叫最终类，该类不能被继承。
* final 修饰的方法不能被重写。
* final 修饰的变量叫常量，常量必须初始化，初始化之后值就不能被修改。

### 5.java 中的 Math.round(-1.5) 等于多少？
等于 -1，因为在数轴上取值时，中间值（0.5）向右取整，所以正 0.5 是往上取整，负 0.5 是直接舍弃。

### 6.String 属于基础的数据类型吗？
String 不属于基础类型，基础类型有 8 种：byte、boolean、char、short、int、float、long、double，而 String 属于对象。

### 7.java 中操作字符串都有哪些类？它们之间有什么区别？
操作字符串的类有：String、StringBuffer、StringBuilder。

String 和 StringBuffer、StringBuilder 的区别在于 String 声明的是不可变的对象，每次操作都会生成新的 String 对象，然后将指针指向新的 String 对象，而 StringBuffer、StringBuilder 可以在原有对象的基础上进行操作，所以在经常改变字符串内容的情况下最好不要使用 String。

StringBuffer 和 StringBuilder 最大的区别在于，StringBuffer 是线程安全的，而 StringBuilder 是非线程安全的，但 StringBuilder 的性能却高于 StringBuffer，所以在单线程环境下推荐使用 StringBuilder，多线程环境下推荐使用 StringBuffer。

### 8.String str="i"与 String str=new String("i")一样吗？
不一样，因为内存的分配方式不一样。String str="i"的方式，java 虚拟机会将其分配到常量池中；而 String str=new String("i") 则会被分到堆内存中。

### 9.如何将字符串反转？
使用 StringBuilder 或者 stringBuffer 的 reverse() 方法。
```java
// StringBuffer reverse
StringBuffer stringBuffer = new StringBuffer();
stringBuffer.append("abcdefg");
System.out.println(stringBuffer.reverse()); // gfedcba
// StringBuilder reverse
StringBuilder stringBuilder = new StringBuilder();
stringBuilder.append("abcdefg");
System.out.println(stringBuilder.reverse()); // gfedcba
```
也可以自己遍历实现

### 10.String 类的常用方法都有那些？
* indexOf()：返回指定字符的索引。
* charAt()：返回指定索引处的字符。
* replace()：字符串替换。
* trim()：去除字符串两端空白。
* split()：分割字符串，返回一个分割后的字符串数组。
* getBytes()：返回字符串的 byte 类型数组。
* length()：返回字符串长度。
* toLowerCase()：将字符串转成小写字母。
* toUpperCase()：将字符串转成大写字符。
* substring()：截取字符串。
* equals()：字符串比较。

### 11.抽象类必须要有抽象方法吗？
不需要，抽象类不一定非要有抽象方法。

### 12.普通类和抽象类有哪些区别？
* 普通类不能包含抽象方法，抽象类可以包含抽象方法。
* 抽象类不能直接实例化，普通类可以直接实例化。

### 13.抽象类能使用 final 修饰吗？
不能，定义抽象类就是让其他类继承的，如果定义为 final 该类就不能被继承，这样彼此就会产生矛盾，所以 final 不能修饰抽象类，如下图所示，编辑器也会提示错误信息。

### 14.接口和抽象类有什么区别？
* 实现：抽象类的子类使用 extends 来继承；接口必须使用 implements 来实现接口。
* 构造函数：抽象类可以有构造函数；接口不能有。
* main 方法：抽象类可以有 main 方法，并且我们能运行它；接口不能有 main 方法。
* 实现数量：类可以实现很多个接口；但是只能继承一个抽象类。
* 访问修饰符：接口中的方法默认使用 public 修饰；抽象类中的方法可以是任意访问修饰符。

### 15.java 中 IO 流分为几种？
按功能来分：输入流（input）、输出流（output）。

按类型来分：字节流和字符流。

字节流和字符流的区别是：字节流按 8 位传输以字节为单位输入输出数据，字符流按 16 位传输以字符为单位输入输出数据。

### 16.BIO、NIO、AIO 有什么区别？
* BIO：Block IO 同步阻塞式 IO，就是我们平常使用的传统 IO，它的特点是模式简单使用方便，并发处理能力低。
* NIO：New IO 同步非阻塞 IO，是传统 IO 的升级，客户端和服务器端通过 Channel（通道）通讯，实现了多路复用。
* AIO：Asynchronous IO 是 NIO 的升级，也叫 NIO2，实现了异步非堵塞 IO ，异步 IO 的操作基于事件和回调机制。

### 17.Files的常用方法都有哪些？
* Files.exists()：检测文件路径是否存在。
* Files.createFile()：创建文件。
* Files.createDirectory()：创建文件夹。
* Files.delete()：删除一个文件或目录。
* Files.copy()：复制文件。
* Files.move()：移动文件。
* Files.size()：查看文件个数。
* Files.read()：读取文件。
* Files.write()：写入文件。

***

### 拓展
#### 说说对常量池对理解
常量池有两种，一种是编译阶段生成的在 *.class 文件中的常量池(Constant Pool Table)。另一种是运行时在方法区中的运行时常量池(Runtime Constant Pool)。
* 在 *.class 文件中，最头的4个字节用于存储魔数(Magic Number)，用于确定一个文件是否能被JVM接受，再接着4个字节用于存储版本号，前2个字节存储次版本号，后2个存储主版本号，再接着是用于存放常量的常量池，由于常量的数量是不固定的，所以常量池的入口放置一个U2类型的数据(constant_pool_count)存储常量池容量计数值。
常量池主要用于存放两大类常量：字面量(Literal)和符号引用量(Symbolic References)，字面量相当于Java语言层面常量的概念，如文本字符串，声明为final的常量值等，符号引用则属于编译原理方面的概念，包括了如下三种类型的常量：类和接口的全限定名;字段名称和描述符;方法名称和描述符.

* 运行时常量池是方法区的一部分。*.cLass 文件中除了有类的版本、字段、方法、接口等描述信息外，还有一项信息是常量池，用于存放编译期生成的各种字面量和符号引用，这部分内容将在类加载后进入方法区的运行时常量池中存放。

运行时常量池相对于 *.class 文件中的常量池的另外一个重要特征是具备动态性，Java语言并不要求常量一定只有编译期才能产生，也就是并非预置入 *.class 文件中的常量池的内容才能进入方法区的运行时常量池，运行期间也可能将新的常量放入池中，这种特性被开发人员利用比较多的就是String类的intern()方法。

常量池的作用：
常量池是为了避免频繁的创建和销毁对象而影响系统性能，其实现了对象的共享。
例如字符串常量池，在编译阶段就把所有的字符串文字放到一个常量池中。
（1）节省内存空间：常量池中所有相同的字符串常量被合并，只占用一个空间。
（2）节省运行时间：比较字符串时，==比equals()快。对于两个引用变量，只用==判断引用是否相等，也就可以判断实际值是否相等。

#### 说说什么是堆内存
* 是被线程共享的一块内存区域，创建的对象和数组都保存在 Java 堆内存中，也是垃圾收集器进行 垃圾收集的最重要的内存区域。由于现代 VM 采用分代收集算法, 因此 Java 堆从 GC 的角度还可以 细分为: 新生代(Eden 区、From Survivor 区和 To Survivor 区)和老年代。

这里又涉及到 GC 相关的内容。一个点一个的深挖下去都是一片汪洋。
