## 3.2 程序计数器(PC寄存器)

JVM 中的程序计数寄存器（Program Counter Register）中，Register的命名源于CPU的寄存器，*寄存器存储指令相关的现场信息。CPU 只有把数据装载到寄存器才能够运行*。这里，并非是广义上所指的物理寄存器，或许将其翻译为 PC 计数器（或指令计数器）会更加贴切（也称为程序钩子），并且也不容易引起一些不必要的误会。<mark>JVM 中的 PC 寄存器是对物理 PC 寄存器的一种抽象模拟</mark>。

![image-20200705155551919](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202301091102191.png)

### 3.2.1 作用

**PC寄存器用来存储指向下一条指令的地址，也就是即将要执行的指令代码。由执行引擎读取下一条指令。**

![image-20200705155728557](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202301091103199.png)

PC寄存器每个线程有一份，代码(指令)会分配到栈的空间当中，一个栈帧对应着一个方法。在这其中具体的指令都有一个行号的标识。*PC寄存器就相当于这个行号的标识*。执行引擎就会根据这个地址就去执行这个指令，执行完这个指令再去PC寄存器里再取下一条的指令执行。



**总结**：PC寄存器就是存放具体代码翻译的指令代码所在的行号标识地址，执行引擎会读取即将执行指令的地址，执行完毕后又来继续获取下一条要执行的指令地址。在这个过程中就是一个不断替换的过程。

-----

1. 它是一块很小的内存空间，几乎可以忽略不记。也是<mark>运行速度最快的存储区域</mark>。
2. 在JVM规范中，<mark>每个线程都有它自己的程序计数器(记录自己线程执行到哪了)，是线程私有的，生命周期与线程的生命周期保持一致</mark>。

3. <mark>任何时间一个线程都只有一个方法在执行，也就是所谓的当前方法</mark>。程序计数器会存储当前线程正在执行的Java方法的JVM指令地址；或者，如果是在执行native方法，则是未指定值（undefined)。(如果是本地方法栈，那么记录的地址就是未指定的值)
4. 它是程序控制流的指示器，分支、循环、跳转、异常处理、线程恢复等基础功能都需要依赖这个计数器来完成。
5. 字节码解释器工作时就是通过改变这个计数器的值来选取下一条需要执行的字节码指令。
6. 它是唯一一个在 Java 虚拟机规范中没有规定任何 OutofMemoryError 情况的区域。



### 3.2.2 举例说明

```java
public static void main(String[] args) {
        int i = 10;
        int j = 20;
        int k = i + j;

        String s = "abc";
        System.out.println(i);
        System.out.println(k);
    }
```

*查看反编译文件cmd代码：javap -verbose 类名.class*

字节码文件：

![image-20230109143856608](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202301091438751.png)

PC寄存器存储当前线程执行方法的指令地址，然后执行引擎会读取PC寄存器里指令地址所代表的操作指令，比如这里5对应着istore_2就表示执行保存到局部变量表的操作。执行引擎会把字节码指令翻译成对应的机器指令，机器指令就会被CPU识别做出运算。



---

ldc：表示从常量池中取常量，后面的#2表示就对应后面的内容，也对应着常量池中的偏移地址。

![image-20230109145538317](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202301091455405.png)

这里由于在常量池中又指向了一个地址，这之间就有两个操作，所以在上面偏移地址中10过了就是12。

---

getstatic：获取之类类的静态域。这里对应常量池地址#3,。

![image-20230109150609028](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202301091506125.png)

一层一层指向。然后获取流打印输出。



### 3.2.3 面试问题

问题1：**使用 PC 寄存器存储字节码指令地址有什么用呢？为什么使用 PC 寄存器记录当前线程的执行地址呢？**

答：

1. 因为 CPU 需要不停的*切换*各个线程，这时候切换回来以后，就得知道接着从哪开始继续执行。

2. JVM 的字节码解释器就需要通过改变 PC 寄存器的值来*明确*下一条应该执行什么样的字节码指令。

![image-20200705161409533](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202301091524075.png)

----

问题2：**PC 寄存器为什么被设定为私有的？每个线程只有一份？**

​	我们都知道所谓的多线程在一个特定的时间段内只会执行其中某一个线程的方法，CPU 会不停地做任务切换，这样必然导致经常中断或恢复，如何保证分毫无差呢？<mark>为了能够准确地记录各个线程正在执行的当前字节码指令地址，最好的办法自然是为每一个线程都分配一个 PC 寄存器</mark>，这样一来*各个线程之间便可以进行独立计算，从而不会出现相互干扰的情况*。



由于 CPU 时间片轮限制，众多线程在并发执行过程中，任何一个确定的时刻，一个处理器或者多核处理器中的一个内核，只会执行某个线程中的一条指令。



这样必然导致经常中断或恢复，如何保证分毫无差呢？每个线程在创建后，都会产生自己的程序计数器和栈帧，程序计数器在各个线程之间互不影响。



-----

**CPU 时间片**

CPU 时间片即 CPU 分配给各个程序的时间，每个线程被分配一个时间段，称作它的时间片。

**在宏观上**：我们可以同时打开多个应用程序，每个程序并行不悖，同时运行。

**在微观上**：由于只有一个CPU(核)，一次只能处理程序要求的一部分，如何处理公平，一种方法就是引入时间片，每个程序轮流执行。

![image-20200705161849557](https://img-blog.csdnimg.cn/img_convert/bbab7cdab74c493af70b423f06e6ff86.png)

并行：同时执行。

并发：CPU快速切换多个线程，看着像并行实则一瞬间只会执行一个。