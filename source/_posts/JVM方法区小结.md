---
title: JVM方法区小结
author: 杨鹏
img: >-
  https://cdn.jsdelivr.net/gh/zhazhapeng10003/cdn-speed2@1.0/pictures2/xingqiuren.jpg
top: false
cover: false
summary: " "
toc: true
mathjax: true
comments: true
tags:
  - JVM
  - 方法区
categories: 编程基础
abbrlink: 6544
date: 2020-05-27 19:51:06
coverImg:
password:
---
**<font color=blue>JVM整体架构图放在底部</font>**
### 运行时数据区结构图
<center>
<img src="https://img-blog.csdnimg.cn/20200527144701142.png" alt="" width="" height="200" align="bottom" />
</center>

----------------
### 从线程共享与否的角度来看

<center>
<img src="https://img-blog.csdnimg.cn/20200527151331445.png" alt="" width="" height="500" align="bottom" />
</center>

-----
### 栈、堆、方法区的交互关系
<center>
<img src="https://img-blog.csdnimg.cn/20200527154158851.png" alt="" width="" height="500" align="bottom" />
</center>

-------------
### 方法区内部结构

<center>
<img src="https://img-blog.csdnimg.cn/20200527161019540.png" alt="" width="" height="500" align="bottom" />
</center>

### 方法区（Method Area）存储什么？
>它用于存储已被虚拟机加载的<font size = 1>**类型信息、常量、静态变量、即时编译器编译后的代码缓存**</font>等。
> &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;-----《深入理解Java虚拟机》
![](https://img-blog.csdnimg.cn/20200527161829101.png)
#### 类型信息
&ensp;&ensp;&ensp;&ensp;对每个加载的类型（类class、接口 interface、枚举enum、注解 annotation），JVM必须在方法区中存储以下类型信息： 
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>这个类型的完整有效名称（全名=包名 **.** 类名）
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>这个类型直接父类的完整有效名（对于interface或是java **.** lang **.** Object，都没有父类）
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>这个类型的修饰符（public，abstract，final的某个子集）
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>这个类型直接接口的一个有序列表 
#### 域（Filed）信息
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>JVM必须在方法区中保存类型的所有域的相关信息以及域的声明顺序。
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>域的相关信息包括：域名称、域类型、域修饰符（ public， private，protected， static，final， volatile， transient的某个子集） 
#### 方法（Method）信息
JVM必须保存所有方法的以下信息，同域信息一样包括声明顺序：
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>方法名称
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>方法的返回类型（或void）
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>方法参数的数量和类型（按顺序）
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>方法的修饰符（public，private，protected，static，final，synchronized，native， abstract的一个子集）
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>方法的字节码（ bytecodes）、操作数栈、局部变量表及大小（ abstract 和 native方法除外）
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>异常表（ abstract和 native方法除外）
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;**<font size = 3>🗡** </font>每个异常处理的开始位置、结束位置、代码处理在程序计数器中的偏移地址、
被捕获的异常类的常量池索引 

#### non-final的类变量（静态变量）
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>静态变量和类关联在一起，随着类的加载而加载，它们成为类数据在逻辑上的一部分。 
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>类变量被类的所有实例共享，即使没有类实例时你也可以访问它。 
#### 补充说明：全局常量：static final
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>被声明为final的类变量的处理方法则不同，每个全局常量在编译的时候就会被分配了。
#### 运行时常量池
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>运行时常量池（ Runtime Constant Pool）是方法区的一部分。 
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>常量池表（ Constant Pool Table）是Class文件的一部分，<font color=red>用于存放编译期生成的各种字面量与符号引用<font color=blue>，这部分内容将在类加载后存放到方法区的运行时常量池中。</font></font> 
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>运行时常量池，在加载类和接口到虚拟机后，就会创建对应的运行时常量池。 
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>JVM为每个已加载的类型（类或接口）都维护一个常量池。池中的数据项像数组项一样，是通过索引访问的。 
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>运行时常量池中包含多种不同的常量，包括编译期就已经明确的数值字面量，也包括到运行期解析后才能够获得的方法或者字段引用。此时不再是常量池中的符号地址了，这里换为真实地址。 
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;🗡运行时常量池，相对于Class文件常量池的另一重要特征是：具备动态性。 
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>运行时常量池类似于传统编程语言中的符号表（symbol table），但是它所包含的数据却比符号表要更加丰富一些。 
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>当创建类或接口的运行时常量池时，如果构造运行时常量池所需的内存空间超过了方法区所能提供的最大值，则JM会抛OutOfMemoryError异常。 

#### <font color=red>方法区的演进细节！</font>
1.首先明确：只有HotSpot才有永久代。
2.HotSpot中方法区的变化：

jdk版本| 具体变化
-------- | -----
jidk1.6及之前  | <font color=red>有永久代（permanent generation），静态变量存放在永久代上</font>
jdk1.7 | <font color=red>有永久代，但已经逐步“去永久代”，字符串常量池、静态变量移除，保存在堆中</font>
jdk1.8及之后  | <font color=red>无永久代，类型信息、字段、方法、常量保存在本地内存的元空间，但字符串常量池、静态变量仍在堆</font>

#### 永久代为什么要被元空间替换？
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>为永久代设置空间大小是很难确定的： 
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;在某些场景下，如果动态加载类过多，容易产生Perm区的OOM。比如某个实际网web工程中，因为功能点比较多，在运行过程中，要不断动态加载很多类，经常出现致命错误。<font color = red >Exception in thread dubbo client x.x connector java. lang. OutotMemory Error： PermGen space</font> 
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;而元空间和永久代之间最大的区别在于：元空间并不在虚拟机中，而是使用本地内存。因此，默认情况下，元空间的大小仅受本地内存限制。 
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>对永久代进行调优是很困难的。 
#### 方法区的垃圾回收
**方法区的垃圾收集主要回收两部分内容：常量池中废弃的常量和不再使用的类型。** 
&ensp;&ensp;&ensp;&ensp;**<font size = 3>•** </font>判定一个常量是否“废弃”还是相对简单，而要判定一个类型是否属于“不再被使用的类”的条件就比较苛刻了。需要同时满足下面三个条件： 
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;🗡该类所有的实例都已经被回收，也就是Java堆中不存在该类及其任何派生子类的实例。 
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;🗡加载该类的类加载器已经被回收，这个条件除非是经过精心设计的可替换类加载器的场景，如OSGi、JSP的重加载等，否则通常是很难达成的。 
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;🗡该类对应的java.1angC1ass对象没有在任何地方被引用，无法在任何地方通过反射访问该类的方法。 
**<font color = red>注：</font>** 满足上述三个条件之后，仅仅只是“被允许”回收。 
#### 字符串常量池（String Table为什么要调整位置）
&ensp;&ensp;&ensp;&ensp;jdk7中将 StringTable放到了堆空间中。因为永久代的回收效率很低，在full gc的时候才会触发。而fullgc是老年代的空间不足、永久代不足时才会触发。 
&ensp;&ensp;&ensp;&ensp;这就导致 StringTable回收效率不高。而我们开发中会有大量的字符串被创建，回收效率低，导致永久代内存不足。放到堆里，能及时回收内存。


------
### JVM架构图

<center>
<img src="https://img-blog.csdnimg.cn/20200527155128840.png" alt="" width="" height="1200" align="bottom" />
</center>

**<font size =5>Peace!**