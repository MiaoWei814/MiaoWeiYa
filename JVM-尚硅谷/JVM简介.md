# 1. JVM 与 Java 体系结构

## 1.1 前言

我们经常会遇到过这些问题：

- 运行着的线上系统突然卡死，系统无法访问，甚至直接 OOM【Out of Memory内存溢出】
- 想解决线上 JVM GC 问题，但却无从下手
- 新项目上线，对各种 JVM 参数设置一脸茫然，直接默认吧然后就G了
- 每次面试之前都要重新背一遍 JVM 的一些原理概念性的东西，然而面试官却经常问你在实际项目中如何调优 VM 参数，如何解决 GC、OOM 等问题，一脸懵逼

![image-20200704111417472](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212161619337.png)

我们写的应用程序是建立在相关开发框架之上，然后开发框架是由Java的API不断的封装建立的。而我们对于最底层的jvm虚拟机的知识相关甚少！

​	**SSM、微服务等上层技术不是重点，基础技术才是最重要的，SSM也都是由基础技术不断构建封装的！**

​	计算机系统体系对我们来说越来越远，在不了解底层实现方式的前提下，通过高级语言很容易编写程序代码。但事实上计算机并不认识高级语言。

## 1.2 JVM 简介

`write once, run anywhere`--> 写好一个就可以运行在任意位置！

![image-20200704151731216](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212161722111.png)

每个语言都需要转换成字节码文件，最后转换的字节码文件都能通过 Java 虚拟机进行运行和处理，突出**跨平台**的特征

![image-20200704152052489](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212161722411.png)

- 随着 Java7 的正式发布，Java 虚拟机的设计者们通过 JSR-292 规范基本实现在<mark>Java 虚拟机平台上运行非 Java 语言编写的程序。</mark>
- Java 虚拟机根本不关心运行在其内部的程序到底是使用何种编程语言编写的，<mark>它只关心“字节码”文件</mark>。也就是说 Java 虚拟机拥有语言**无关性**，并不会单纯地与 Java 语言“终身绑定”，只要其他编程语言的编译结果满足并包含 Java 虚拟机的内部指令集、符号表以及其他的辅助信息，它就是一个有效的字节码文件，就能够被虚拟机所识别并装载运行。

`JAVA不是强大的语言，但是JVM是最强大的虚拟机`

**字节码**

- 我们平时说的 java 字节码，指的是用 java 语言编译成的字节码。准确的说任何能在 jvm 平台上执行的字节码格式都是一样的。所以应该统称为：<mark>jvm 字节码</mark>。
- 不同的编译器，可以编译出相同的字节码文件，字节码文件也可以在不同的 JVM 上运行。
- Java 虚拟机与 Java 语言并没有必然的联系，它只与特定的二进制文件格式—Class 文件格式所关联，Class 文件中包含了 Java 虚拟机指令集（或者称为字节码、Bytecodes）和符号表，还有一些其他辅助信息 

## 1.3 虚拟机与 Java 虚拟机

**虚拟机**

所谓虚拟机（Virtual Machine），就是一台虚拟的计算机。它是一款软件，用来执行一系列虚拟计算机指令。大体上，虚拟机可以分为系统虚拟机和程序虚拟机。

- 大名鼎鼎的 Visual Box，Mware 就属于系统虚拟机，它们<mark>完全是对物理计算机的仿真</mark>，提供了一个可运行完整操作系统的软件平台。
- 程序虚拟机的典型代表就是 Java 虚拟机，它<mark>专门为执行单个计算机程序而设计</mark>，`在 Java 虚拟机中执行的指令我们称为 Java 字节码指令`。

无论是系统虚拟机还是程序虚拟机，在上面运行的软件都被限制于虚拟机提供的资源中。

**Java 虚拟机**

- Hotspot虚拟机

- Java 虚拟机是一台执行 Java 字节码的虚拟计算机，它拥有独立的运行机制，其运行的 Java 字节码也未必由 Java 语言编译而成。
- JVM 平台的各种语言可以共享 Java 虚拟机带来的跨平台性、优秀的垃圾回器，以及可靠的即时编译器。
- <mark>Java 技术的核心就是 Java 虚拟机</mark>（JVM，Java Virtual Machine），因为所有的 Java 程序都运行在 Java 虚拟机内部。

作用

- _Java 虚拟机就是二进制字节码的运行环境_，负责装载字节码到其内部，解释/编译为对应平台上的机器指令执行。每一条 Java 指令，Java 虚拟机规范中都有详细定义，如怎么取操作数，怎么处理操作数，处理结果放在哪里。

特点

- 一次编译，到处运行
- 自动内存管理
- 自动垃圾回收功能

> **JVM 的位置**

![image-20200704183048061](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212220927034.png)

JVM 是运行在操作系统之上的，它与硬件没有直接的交互

我们在不同的操作系统上安装的JDK也会导致JVM是有区别的！

## 1.4 JVM 的整体结构

![image-20200704183436495](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212220927236.png)

- HotSpot VM 是目前市面上高性能虚拟机的代表作之一。
- 它采用解释器与即时编译器并存的架构。



首先Java虚拟机是用来解释Class字节码文件(java编译器将.java生成.class文件)的，

1. 这里使用加载器将字节码文件加载到内存当中生成一个大的Class对象，这个过程中会涉及到加载、链接、初始化。
2. 中间数据区里要记住的是：多线程共享方法区和堆。而其他java栈、本地方法栈、程序计数器的线程独有一份的。
3. 执行引擎里就有：解释器、JIT即时编译器、垃圾回收器。作用就在于将class这些高级语言转换为操作系统能识别到的机器指令。

![Java 类的静态变量存放在哪块内存中？_java](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305111530841.webp)



## 1.5 Java 代码执行流程

![image-20221222095141397](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212220951515.png)

​	首先java源文件经过Java编译器("前端"编译器)生成字节码文件，在这个编译环节过程中任何一阶段失败了就生成不了字节码文件。一直到Java虚拟机中有一个JIT编译器("后端"编译器)生成机器指令来提供操作系统辨认。





> 其实我们也可以开发一门语言，只要遵守生成字节码文件就可以拿到java虚拟机上去运行使用！

## 1.8 JVM 的架构模型

Java 编译器输入的指令流基本上是一种基于<mark>栈的指令集架构</mark>，另外一种指令集架构则是基于<mark>寄存器的指令集架构</mark>。



​	HotSpotVM当中任何操作都需要一个入栈和出栈的操作，也就是栈来管理运行，由此可见HotSpotVm是基于栈的指令集架构。



具体来说：这两种架构之间的区别：

**基于栈式架构的特点**

- 设计和实现更简单，适用于资源受限的系统，比如Java
- 避开了寄存器的分配难题：使用零地址指令方式分配
- 指令流中的指令大部分是零地址指令，其执行过程依赖于操作栈。指令集更小，编译器容易实现
- 不需要硬件支持(内存级别的)，可移植性更好，更好实现跨平台
- 指令集更小但花费较多！是8位的！

**基于寄存器架构的特点**

- 典型的应用是 x86 的二进制指令集：比如传统的 PC 以及 Android 的 Davlik 虚拟机
- 指令集架构则完全依赖硬件，可移植性差
- 性能优秀和执行更高效
- 指令集较大，但花费更少的指令去完成一项操作，是16位的！
- 指令由CPU来执行，耦合度较高！
- 在大部分情况下，基于寄存器架构的指令集往往都以一地址指令、二地址指令和三地址指令为主，而基于栈式架构的指令集却是以零地址指令为主

**举例：**

同样执行 2+3 这种逻辑操作，其指令分别如下：

基于栈的计算流程（以 Java 虚拟机为例）：

```java
iconst_2 //常量2入栈
istore_1
iconst_3 // 常量3入栈
istore_2
iload_1
iload_2
iadd //常量2/3出栈，执行相加
istore_0 // 结果5入栈
```

而基于寄存器的计算流程

```java
mov eax,2 //将eax寄存器的值设为1
add eax,3 //使eax寄存器的值加3
```

可以发现：基于栈的指令较多但指令集较小！而寄存器指令集较大但花费较少

----

新建项目然后写一个main方法：

```java
public class StackStruTest {
    public static void main(String[] args) {
        int i = 2 + 3;
    }
}
```

然后在运行在指定out目录下找到对应的class文件，然后在当前目录下执行cmd命令`javap -v 文件名`用于查看反编译文件：

![image-20221222105312613](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212221053707.png)

----

现在又修改一下：

```java
public class StackStruTest {
    public static void main(String[] args) {
        int i = 2;
        int j = 3;
        int k = i + j;
    }
}
```

查看反编译后的结果：

![image-20221222110031537](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212221100626.png)

> 查看的时候可以去掉“i",然后就理解啥意思了！



**总结**

<mark>由于跨平台性的设计，Java 的指令都是根据栈来设计的。</mark>不同平台 CPU 架构不同，所以不能设计为基于寄存器的。`优点是跨平台，指令集小，编译器容易实现，缺点是性能下降，实现同样的功能需要更多的指令`。

时至今日，尽管嵌入式平台已经不是 Java 程序的主流运行平台了（准确来说应该是 HotSpotVM 的宿主环境已经不局限于嵌入式平台了），那么为什么不将架构更换为基于寄存器的架构呢？答: 首先栈的架构设计实现较为简单、栈不局限于任何环境！

 

## 1.9 JVM 的生命周期

**虚拟机的启动**

Java 虚拟机的启动是通过**引导类加载器**（bootstrap class loader）创建一个初始类（initial class）来完成的，这个类是由虚拟机的具体实现指定的。

**虚拟机的执行**

- 一个运行中的 Java 虚拟机有着一个清晰的任务：执行 Java 程序。

- 程序开始执行时他才运行，程序结束时他就停止。

- <mark>执行一个所谓的 Java 程序的时候，真真正正在执行的是一个叫做 Java 虚拟机的进程。</mark>

  ![image-20221222111950556](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212221119637.png)

  启动的时候执行cmd命令`jps`就可查看当前程序当中的进程！

  ![image-20221222112300708](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212221123788.png)

**虚拟机的退出**

有如下的几种情况：

- 程序正常执行结束
- 程序在执行过程中遇到了异常或错误而异常终止
- 由于操作系统用现错误而导致 Java 虚拟机进程终止
- 某线程调用 Runtime 类或 system 类的 exit 方法，或 Runtime类的halt方法，并且 Java 安全管理器也允许这次 exit 或 halt 操作。两者只是名字不一样最终都是去调用Shutdown.exit来断掉当前进程。
- 除此之外，JNI（Java Native Interface）规范描述了用 JNI Invocation API 来加载或卸载 Java 虚拟机时，Java 虚拟机的退出情况。