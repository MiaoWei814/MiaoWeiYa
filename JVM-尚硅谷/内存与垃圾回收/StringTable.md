# 10. StringTable

## 10.1. String的变化

- String：字符串，使用一对""引起来表示
  - String s1="abcd" // 字面量的定义方式
  - String s2=new String("abcd")//new类的形式

- String 声明为 final 的，不可被继承
- String 实现了 Serializable 接口：表示字符串是支持序列化的。
  - 跨进程传输数据如果以String来进行传输，天然就没有问题因为已经实现了序列化机制。

- String 实现了 Comparable 接口：表示 string 可以比较大小，也可拿来排序
- String 在 jdk8 及以前内部定义了 final char[] value 用于存储字符串数据。JDK9 时改为 byte[]

这是JDK1.8的：

![image-20230531161637931](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305311616972.png)

这是JDK1.9的：

![image-20230531162201253](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305311622309.png)

由之前的char数组修改为byte数组！

### 10.1.1. String 在 jdk9 中存储结构变更

官网地址：[JEP 254: Compact Strings (java.net)](http://openjdk.java.net/jeps/254)

![image-20230531162834843](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305311628890.png)

Latin-1字符：拉丁字符

ISO-8859-1: 欧洲码

UTF-16：两个字节

```
认为jdk8使用char类型2个字节来存储，发现很多时候根本用不完有一半空间没被使用，为了节约空间就采用byte一个字节来存储。事实证明内存占用减少、GC活动的大幅减少。

由于我们的汉字本身就需要使用两个字节来存储，所以：
注意：从UTF-16两个字节的char数组更改为byte数组之外，再加一个字符编码标识。--- 也就是说如果你是ISO-8859-1/Latin-1那么就会用一个字节去存，如果是其他字符集那么就用两个字节去存。
```

 

ps：是将字符内容的编码字符进行修改！就是说你字符串过来了这时会根据编码标记判断此字符是用一个字节来存储还是两个字节来存储。

结论：String 再也不用 char[] 来存储了，改成了 byte [] 加上`编码标记`，节约了一些空间

```java
public final class String implements java.io.Serializable, Comparable<String>, CharSequence {
    @Stable
    private final byte[] value;
}
```

### 10.1.2. String 的基本特性

String：代表**不可变的字符序列**。简称：不可变的特性。

- 当对字符串重新赋值时，需要重写指定内存区域赋值，不能使用原有的 value 进行赋值。
- 当对现有的字符串进行连接操作时，也需要重新指定内存区域赋值，不能使用原有的 value 进行赋值。
- 当调用 string 的 replace()方法修改指定字符或字符串时，也需要重新指定内存区域赋值，不能使用原有的 value 进行赋值。

案例：

```java
//处于方法区中栈帧中，其中S1和S2都处于局部变量表中，而“abc”是存储在堆中地址，这里S1就指向了这个地址。
@Test
public void test1() {
    String s1 = "abc";//字面量定义的方式，"abc"存储在字符串常量池中
    String s2 = "abc";
    s1 = "hello";
    System.out.println(s1 == s2);//判断地址：false
    System.out.println(s1);//hello
    System.out.println(s2);//abc

}

// 由于一般创建数组后长度就定死了，不允许扩容的 一般要扩容就要重新创建一个数组。
// 字符串常量池中只要修改都要重新创建！--体现出不可变性！
@Test
public void test2() {
    String s1 = "abc";
    String s2 = "abc";
    s2 += "def";
    System.out.println(s2);//abcdef
    System.out.println(s1);//abc
}

// 返回的是修改之后的字符，而s1本身没有改变。也体现出了不可变性！
@Test
public void test3() {
    String s1 = "abc";
    String s2 = s1.replace('a', 'm');
    System.out.println(s1);//abc
    System.out.println(s2);//mbc
}

@Test
public void test4() {
    String s1 = "abc"; // 指向常量池中的地址
    String s2 = new String("abc"); //new的都在堆中，既然在堆中都要开辟一个内存空间。然后去常量池中去找“abc”，找到就存储其常量池的地址
    boolean x = s1 == s2;
    System.out.println(x); // false
}
```

ps：需要强调的是字符串常量池当中绝对不允许放相同的字符串



通过字面量的方式（区别于 new）给一个字符串赋值，此时的字符串值声明在字符串常量池中。

### 10.1.3 String底层HashTable结构

先上面试题：

```java
public class StringExer {
    String str = new String("good");
    char[] ch = {'t', 'e', 's', 't'};

    public void change(String str, char ch[]) {
        str = "test ok";
        ch[0] = 'b';
    }

    public static void main(String[] args) {
        StringExer ex = new StringExer();
        ex.change(ex.str, ex.ch);
        System.out.println(ex.str); // good
        System.out.println(ex.ch);  // best
    }
}
```

![image-20230601145510495](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202306011455664.png)

解读：

1. 核心一句话：只要修改都会重新创建对象。这里虽然数据传递进去了但由于作用域的关系，这里的str是指的是当前形参而非外面的类变量。
2. 而ch数组传递时由于地址都是一样的，所以修改的是同一个堆中对象地址。

**在Java中数组也是对象，所以在堆中是有地址的！**

ps：如果传递进去后str改为this.str，那么这里就真正修改了其数据。真正要注意的就是数组是对象，传递的内存地址是一样的！

----

**字符串常量池是不会存储相同内容的字符串的**

```
如何保证存储不会重复？
	在Java中为了去重我们一般使用Set，而像HashSet其底层就是一个Map。而key呢就是Object，value就是对应的值。其实我想说的是可以理解为这里常量池这里底层就是一个map！
```



String 的字符串常量池是一个固定大小的`Hashtable`，默认值大小长度是 1009。如果放进 String Pool 的 String 非常多，就会造成 Hash 冲突严重，从而导致链表会很长，而链表长了后直接会造成的影响就是当调用 String.intern 时性能会大幅下降。

ps：这个不能扩容的所以容易出现计算hash在同一个坑位，然后就得遍历这个链表往后添加，性能很低！



使用`-XX:StringTableSize`可设置 StringTable 的长度

- 在 jdk6 中 StringTable 是固定的，就是 1009 的长度，所以如果常量池中的字符串过多就会导致效率下降很快。StringTablesize 设置没有要求

- 在 jdk7 中，StringTable 的长度默认值是 60013，StringTablesize 设置没有要求

- 在 JDK8 中，设置 StringTable 长度的话，1009 是可以设置的最小值

案例：

```java
//测试StringTableSize参数
System.out.println("我来打个酱油");
try {
    Thread.sleep(1000000);
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

在cmd命令中输入`jps`找到对应的启动程序名称，然后输入查看JVM命令参数配置`jinfo -flag StringTableSize 31136	`

![image-20230601153741267](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202306011537321.png)

但是如果我们手动的设置了启动参数大小为1000，那么再看：

![image-20230601154248950](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202306011542005.png)

要求最低也得1009！

> 测试StringTable长度的效率对比

```java
public class StringTest2 {
    public static void main(String[] args) {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader("words.txt"));
            long start = System.currentTimeMillis();
            String data;
            while((data = br.readLine()) != null){
                data.intern(); //如果字符串常量池中没有对应data的字符串的话，则在常量池中生成
            }

            long end = System.currentTimeMillis();

            System.out.println("花费的时间为：" + (end - start));//1009:143ms  100009:47ms
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(br != null){
                try {
                    br.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

论证：数组长度变长对于性能来说 提升较大！



## 10.2. String 的内存分配

在 Java 语言中有 8 种基本数据类型和一种比较特殊的类型 String。这些类型为了使它们在运行过程中速度更快、更节省内存，`都提供了一种常量池的概念`。

常量池就类似一个 Java 系统级别提供的缓存。8 种基本数据类型的常量池都是系统协调的，<mark>String 类型的常量池比较特殊。它的主要使用方法有两种。</mark>

- 直接使用双引号声明出来的 String 对象会直接存储在常量池中。

- 如果不是用双引号声明的 String 对象，可以使用 String 提供的 intern()方法。这个后面重点谈

**存储历史：**

Java 6 及以前，字符串常量池存放在永久代

Java 7 中 Oracle 的工程师对字符串池的逻辑做了很大的改变，即将<mark>字符串常量池的位置调整到 Java 堆内</mark>

- 所有的字符串都保存在堆（Heap）中，和其他普通对象一样，这样可以让你在**进行调优应用时仅需要调整堆大小就可以了。**

- 字符串常量池概念原本使用得比较多，但是这个改动使得我们有足够的理由让我们重新考虑在 Java 7 中使用`String.intern()`。

Java8 元空间，字符串常量在堆

![image-20200711093546398](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305311557330.png)

![image-20200711093558709](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305311557575.png)

**StringTable 为什么要调整？**

官网地址：[Java SE 7 Features and Enhancements (oracle.com)](https://www.oracle.com/java/technologies/javase/jdk7-relnotes.html#jdk7changes)

> **Synopsis:** In JDK 7, interned strings are no longer allocated in the permanent generation of the Java heap, but are instead allocated in the main part of the Java heap (known as the young and old generations), along with the other objects created by the application. This change will result in more data residing in the main Java heap, and less data in the permanent generation, and thus may require heap sizes to be adjusted. Most applications will see only relatively small differences in heap usage due to this change, but larger applications that load many classes or make heavy use of the `String.intern()` method will see more significant differences.

简介：在JDK7中，内部字符串不再分配在Java 堆的永久代中，而是分配在 Java 堆的主要部分（称为年轻代和老年代），与应用程序创建的其他对象一起。这种变化将导致更多的数据驻留在主 Java 堆中，而更少的数据在永久代中，因此可能需要调整堆的大小。大多数应用程序将看到由于这一变化而导致的堆使用的相对较小的差异，但<mark>加载许多类或大量使用 String.intern()方法的大型应用程序将看到更明显的差异</mark>。

原因：

永久代->堆空间

1. permSize默认比较小，如果存放大量的字符串就有可能会导致永久代OOM。
2. 垃圾回收频率低(基本不动永久代)。也就意味了不用的字符串达不到及时的回收。



## 10.3. String 的基本操作

```java
@Test
public void test1() {
    System.out.println();//2133
    System.out.print1n("1"); //2134
    System.out.println("2");
    System.out.println("3");
    System.out.println("4");
    System.out.println("5");
    System.out.println("6");
    System.out.println("7");
    System.out.println("8");
    System.out.println("9");
    System.out.println("10"); //2144
    
    //如下的字符串“1”到“10”不会再次加载
    System.out.println("1");  //2144
    System.out.println("2");  //2144
    System.out.println("3");
    System.out.println("4");
    System.out.println("5");
    System.out.print1n("6");
    System.out.print1n("7");
    System.out.println("8");
    System.out.println("9");
    System.out.println("10");//2144
}
```

**会先去字符串常量池中寻找，如果有就不会再去字符串常量池中重新生成一遍了**

第一轮执行“10”此时内存中加载的String类为2144个，再执行完一轮此时就不会再去生成新的String

ps：

![image-20230601172825444](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202306011728503.png)



Java 语言规范里要求完全相同的字符串字面量，应该包含同样的 Unicode 字符序列（包含同一份码点序列的常量），并且必须是指向同一个 String 类实例。只能有一个！

```java
class Memory {
    public static void main(String[] args) {//line 1
        int i= 1;//line 2
        Object obj = new Object();//line 3
        Memory mem = new Memory();//Line 4
        mem.foo(obj);//Line 5
    }//Line 9
    private void foo(Object param) {//line 6
        // 这里调用toString()方法返回是个字符串，而这个字符串就存放在字符常量池当中，记录着当前对象的类型和它具体存储的地址。然后进行打印。
        String str = param.toString();//line 7
        System.out.println(str);
    }//Line 8
}
```

![image-20210511111607132](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305311557612.png)

## 10.4. 字符串拼接操作

- 常量与常量的拼接结果在常量池，原理是**编译期优化**
- 常量池中不会存在相同内容的变量
- 只要其中有一个是变量，结果就在堆中。**变量拼接的原理是 StringBuilder**
- 如果拼接的结果调用 intern()方法，则主动将常量池中还没有的字符串对象放入池中，并返回此对象地址

### 10.4.1 面试题

**举例 1**

```java
  public static void test1() {
      // 都是常量，前端编译期会进行代码优化
      // 通过idea直接看对应的反编译的class文件，会显示 String s1 = "abc"; 说明做了代码优化
      String s1 = "a" + "b" + "c";
      String s2 = "abc";

      // true，有上述可知，s1和s2实际上指向字符串常量池中的同一个值
      System.out.println(s1 == s2);
      System.out.println(s1.equals(s2)); //true
  }
```

反编译文件：

![image-20230602094027361](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202306020940500.png)

再查看字节码指令：

![image-20230602094338479](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202306020943551.png)

通过字节码指令可知所获取的常量池的地址是一样的！

**举例 2**

```java
public static void test5() {
    String s1 = "javaEE";
    String s2 = "hadoop";

    String s3 = "javaEEhadoop";
    String s4 = "javaEE" + "hadoop";
    //如果拼接符号的前后出现了变量，则相当于在堆空间中new String()，具体的内容为拼接的结果：javaEEhadoop
    String s5 = s1 + "hadoop";
    String s6 = "javaEE" + s2;
    String s7 = s1 + s2;

    System.out.println(s3 == s4); // true 编译期优化，所以地址相同
    System.out.println(s3 == s5); // false s1是变量，不能编译期优化
    System.out.println(s3 == s6); // false s2是变量，不能编译期优化
    System.out.println(s3 == s7); // false s1、s2都是变量
    System.out.println(s5 == s6); // false s5、s6 不同的对象实例
    System.out.println(s5 == s7); // false s5、s7 不同的对象实例
    System.out.println(s6 == s7); // false s6、s7 不同的对象实例

    //intern():判断字符串常量池中是否存在javaEEhadoop值，如果存在，则返回常量池中javaEEhadoop的地址；
    //如果字符串常量池中不存在javaEEhadoop，则在常量池中加载一份javaEEhadoop，并返回此对象的地址。
    String s8 = s6.intern();
    System.out.println(s3 == s8); // true intern之后，s8和s3一样，指向字符串常量池中的"javaEEhadoop"
}
```

ps：

1. 常量与常量进行拼接，那么就会在编译器就会优化拼接在一起，那么在实际的字节码中会共用一个常量池地址。
2. 拼接出现变量，都相当于new了一个对象。那么只要new了对象就会在堆中开辟新的空间-新的引用地址。 

- 其内部的实现原理实际上是先创造一个StringBuilder对象，用于盛装所有我们所需要拼接的字符串，对每个字符串都调用一次append( )方法，最后再调用toString( )方法，生成字符串拼接完成的结果。

### 10.4.2 拼接操作的底层原理

**举例 4**

```java
public void test3(){
    String s1 = "a";
    String s2 = "b";
    String s3 = "ab";
    /*
        如下的s1 + s2 的执行细节：(变量s是我临时定义的）
        1. StringBuilder s = new StringBuilder();
        2. s.append("a")
        3. s.append("b")
        4. s.toString()  --> 约等于 new String("ab")

        补充：在jdk5.0之后使用的是StringBuilder,在jdk5.0之前使用的是StringBuffer
     */
    String s4 = s1 + s2;
    System.out.println(s3==s4);
}
```

**字节码**

我们拿例 4 的字节码进行查看，可以发现`s1 + s2`实际上是 new 了一个 StringBuilder 对象，并使用了 append 方法将 s1 和 s2 添加进来，最后调用了 toString 方法赋给 s4

![image-20230602111703403](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202306021117495.png)

**举例 3**



```java
/*
    1. 字符串拼接操作不一定使用的是StringBuilder!
       如果拼接符号左右两边都是字符串常量或常量引用，则仍然使用编译期优化，即非StringBuilder的方式。
    2. 针对于final修饰类、方法、基本数据类型、引用数据类型的量的结构时，能使用上final的时候建议使用上。
*/
public void test6(){
    String s0 = "beijing";
    String s1 = "bei";
    String s2 = "jing";
    String s3 = s1 + s2;
    System.out.println(s0 == s3); // false s3指向对象实例，s0指向字符串常量池中的"beijing"
    
    String s7 = "shanxi";
    final String s4 = "shan";
    final String s5 = "xi";
    String s6 = s4 + s5;
    System.out.println(s6 == s7); // true s4和s5是final修饰的，编译期就能确定s6的值了
}
```

- 不使用 final 修饰，即为变量。如 s3 行的 s1 和 s2，会通过 new StringBuilder 进行拼接
- 使用 final 修饰，即为常量。会在编译器进行代码优化。<mark>在实际开发中，能够使用 final 的，尽量使用</mark>

final修饰的变量在JVM中的类加载器子系统中的链接环节中的“准备“环节。原本是将静态变量会进行默认值的初始化，但是如果加了final直接进行显式初始化。赋值的时机会更早一点。

![image-20230602132614808](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202306021326875.png)



### 10.4.3 拼接操作与append操作的效率对比



**字符串拼接操作性能对比**

```java
    /*
    体会执行效率：通过StringBuilder的append()的方式添加字符串的效率要远高于使用String的字符串拼接方式！
    详情：1. StringBuilder的append()的方式：自始至终中只创建过一个StringBuilder的对象
          使用String的字符串拼接方式：创建过多个StringBuilder和String的对象
         2. 使用String的字符串拼接方式：内存中由于创建了较多的StringBuilder和String的对象，内存占用更大；如果进行GC，需要花费额外的时间。

     改进的空间：在实际开发中，如果基本确定要前前后后添加的字符串长度不高于某个限定值highLevel的情况下,建议使用构造器实例化：
               StringBuilder s = new StringBuilder(highLevel);//new char[highLevel]
     */
    @Test
    public void test6(){

        long start = System.currentTimeMillis();
//        method1(100000);//耗时4014ms
        method2(100000);//耗时7ms

        long end = System.currentTimeMillis();
        System.out.println("花费的时间为：" + (end - start));
    }

    public void method1(int highLevel){
        String src = "";
        for(int i = 0;i < highLevel;i++){
            src = src + "a";//每次循环都会创建一个StringBuilder、String(StringBuilder底层调用toString就会new一个String)
        }
    }

    public void method2(int highLevel){
        //只需要创建一个StringBuilder
        StringBuilder src = new StringBuilder();
        for (int i = 0; i < highLevel; i++) {
            src.append("a");
        }
    }
}
```

那么，在实际开发中，对于需要<mark>多次或大量拼接</mark>的操作，在不考虑线程安全问题时，我们就应该尽可能<mark>使用 StringBuilder 进行 append 操作</mark>



除此之外，还有那些操作能够帮助我们提高字符串方面的运行效率呢？

StringBuilder 空参构造器的初始化大小为 16。那么，如果提前知道需要拼接 String 的个数，就应该直接使用<mark>带参构造器</mark>指定 capacity，以减少扩容的次数（扩容的逻辑可以自行查看源代码）

```java
// 底层也是使用的char数组进行存储
char[] value;

// 默认初始容量为16，每次容量不够就会copy新的一个数组并且计算重新存入
public StringBuilder() {
   super(16);
}

//一开始就设置容量大小就减少了每次因数组容量不够而重新扩容的问题
AbstractStringBuilder(int capacity) {
   value = new char[capacity];
}
```





## 10.5. intern()的使用

官方 API 文档中的解释

![image-20230602135226139](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202306021352208.png)

当调用 intern 方法时，如果池子里已经包含了一个与这个 String 对象相等的字符串，正如 equals(Object)方法所确定的，那么池子里的字符串会被返回。否则，这个 String 对象被添加到池中，并返回这个 String 对象的引用。

由此可见，对于任何两个字符串 s 和 t，当且仅当 s.equals(t)为真时，s.intern() == t.intern()为真。

所有字面字符串和以字符串为值的常量表达式都是 interned。

返回一个与此字符串内容相同的字符串，但保证是来自一个唯一的字符串池。

**核心：只要equals相等，那么使用intern比较时必定就为真！**

---

intern 是一个 native 方法，调用的是底层 C 的方法

![image-20230602135055937](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202306021350012.png)

如果不是用双引号声明的 String 对象，可以使用 String 提供的 intern 方法，它会从字符串常量池中查询当前字符串是否存在，若不存在就会将当前字符串放入常量池中。

```java
String myInfo = new string("I love atguigu").intern();
```

也就是说，如果在任意字符串上调用 String.intern 方法，那么其返回结果所指向的那个类实例，必须和直接以常量形式出现的字符串实例完全相同。因此，下列表达式的值必定是 true

```java
("a"+"b"+"c").intern() == "abc"
```

通俗点讲，Interned string 就是确保字符串在内存里只有一份拷贝，这样可以节约内存空间，加快字符串操作任务的执行速度。注意，这个值会被存放在字符串内部池（String Intern Pool）

![image-20210511145542579](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305311557551.png)





### 10.5.1. JDK6 vs JDK7/8

**面试题：**

```java
/**
 * 题目：
 * new String("ab")会创建几个对象？看字节码，就知道是两个。
 *     一个对象是：new关键字在堆空间创建的
 *     另一个对象是：字符串常量池中的对象"ab"。 字节码指令：ldc  （类加载完后会在字符串常量池中创建hello字符串实例）
 *
 *
 * 思考：
 * new String("a") + new String("b")呢？
 *  对象1： new StringBuilder()
 *  对象2： new String("a")
 *  对象3： 常量池中的"a"
 *  对象4： new String("b")
 *  对象5： 常量池中的"b"
 *
 *  深入剖析： StringBuilder的toString():
 *      对象6 ：new String("ab")
 *       强调一下，toString()的调用，在字符串常量池中，没有生成"ab"
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  20:38
 */
public class StringNewTest {
    public static void main(String[] args) {
//        String str = new String("ab");

        String str = new String("a") + new String("b");
    }
}
```

ps：比较踩坑的点就在于此字符串得看之前是否常量池存在过，如果不存在就会创建生成一个对象

**注意：**`toString()`方法，本质是调用`new String()`重新创建一个String对象，创建的`ab`字符串不是常量，所以不会放入常量池，String类的本质是char数组

![image-20230602164457166](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202306021644229.png)

**这是一道有关于jdk版本的不同答案的面试题：**

jdk6之前：String存放在方法区中。

jdk7/8：String存放在堆空间当中。

```java
/**
 * 如何保证变量s指向的是字符串常量池中的数据呢？
 * 有两种方式：
 * 方式一： String s = "shkstart";//字面量定义的方式
 * 方式二： 调用intern()
 *         String s = new String("shkstart").intern();
 *         String s = new StringBuilder("shkstart").toString().intern();
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  18:49
 */
public class StringIntern {
    public static void main(String[] args) {

        String s = new String("1");
        s.intern();//调用此方法之前，字符串常量池中已经存在了"1"
        String s2 = "1";
        System.out.println(s == s2);//jdk6：false   jdk7/8：false

        String s3 = new String("1") + new String("1");//s3变量记录的地址为：new String("11")
        //执行完上一行代码以后，字符串常量池中，是否存在"11"呢？答案：不存在！！因为11是在StringBuilder中的char数组中
        s3.intern();//在字符串常量池中生成"11"。如何理解：jdk6:创建了一个新的对象"11",也就有新的地址。
                                            //         jdk7:此时常量中并没有创建"11",而是创建一个指向堆空间中new String("11")的地址
        String s4 = "11";//s4变量记录的地址：使用的是上一行代码代码执行时，在常量池中生成的"11"的地址
        System.out.println(s3 == s4);//jdk6：false  jdk7/8：true
    }
}
```

**所以说：intern无论如何都会指向常量池中的地址，并且将该变量的持有者的地址进行变更。**

![image-20210511152240683](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305311557643.png)

![image-20200711145925091](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305311557556.png)

总结 String 的 intern()的使用：

JDK1.6 中，将这个字符串对象尝试放入串池。

- 如果串池中有，则并不会放入。返回已有的串池中的对象的地址
- 如果没有，会把此<mark>对象复制一份</mark>，放入串池，并返回串池中的对象地址

JDK1.7 起，将这个字符串对象尝试放入串池。

- 如果串池中有，则并不会放入。返回已有的串池中的对象的地址
- 如果没有，则会把<mark>对象的引用地址</mark>复制一份，放入串池，并返回串池中的引用地址

**练习 1**

![image-20200711150859709](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305311557675.png)

![image-20200711151326909](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305311557082.png)

**练习 2**

![image-20200711151433277](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305311557105.png)

### 10.5.2. intern 的效率测试：空间角度

我们通过测试一下，使用了 intern 和不使用的时候，其实相差还挺多的

```java
public class StringIntern2 {    static final int MAX_COUNT = 1000 * 10000;    static final String[] arr = new String[MAX_COUNT];    public static void main(String[] args) {        Integer [] data = new Integer[]{1,2,3,4,5,6,7,8,9,10};        long start = System.currentTimeMillis();        for (int i = 0; i < MAX_COUNT; i++) {            // arr[i] = new String(String.valueOf(data[i%data.length]));            arr[i] = new String(String.valueOf(data[i%data.length])).intern();        }        long end = System.currentTimeMillis();        System.out.println("花费的时间为：" + (end - start));        try {            Thread.sleep(1000000);        } catch (Exception e) {            e.getStackTrace();        }    }}// 运行结果不使用intern：7256ms使用intern：1395ms
```

**结论**：对于程序中大量使用存在的字符串时，尤其存在很多已经重复的字符串时，使用 intern()方法能够节省内存空间。

大的网站平台，需要内存中存储大量的字符串。比如社交网站，很多人都存储：北京市、海淀区等信息。这时候如果字符串都调用 intern()方法，就会很明显降低内存的大小。

## 10.6. StringTable 的垃圾回收

```java
public class StringGCTest {    /**     * -Xms15m -Xmx15m -XX:+PrintGCDetails     */    public static void main(String[] args) {                for (int i = 0; i < 100000; i++) {            String.valueOf(i).intern();        }    }}
```

运行结果

```java
[GC (Allocation Failure) [PSYoungGen: 4096K->504K(4608K)] 4096K->1689K(15872K), 0.0581583 secs] [Times: user=0.00 sys=0.00, real=0.06 secs] [GC (Allocation Failure) [PSYoungGen: 4600K->504K(4608K)] 5785K->2310K(15872K), 0.0015621 secs] [Times: user=0.00 sys=0.00, real=0.00 secs] [GC (Allocation Failure) [PSYoungGen: 4600K->504K(4608K)] 6406K->2350K(15872K), 0.0034849 secs] [Times: user=0.00 sys=0.00, real=0.00 secs] Heap PSYoungGen      total 4608K, used 1919K [0x00000000ffb00000, 0x0000000100000000, 0x0000000100000000)  eden space 4096K, 34% used [0x00000000ffb00000,0x00000000ffc61d30,0x00000000fff00000)  from space 512K, 98% used [0x00000000fff00000,0x00000000fff7e010,0x00000000fff80000)  to   space 512K, 0% used [0x00000000fff80000,0x00000000fff80000,0x0000000100000000) ParOldGen       total 11264K, used 1846K [0x00000000ff000000, 0x00000000ffb00000, 0x00000000ffb00000)  object space 11264K, 16% used [0x00000000ff000000,0x00000000ff1cd9b0,0x00000000ffb00000) Metaspace       used 3378K, capacity 4496K, committed 4864K, reserved 1056768K  class space    used 361K, capacity 388K, committed 512K, reserved 1048576K
```

## 10.7. G1 中的 String 去重操作

官网地址：[JEP 192: String Deduplication in G1 (java.net)](http://openjdk.java.net/jeps/192)

> ## Motivation
>
> Many large-scale Java applications are currently bottlenecked on memory. Measurements have shown that roughly 25% of the Java heap live data set in these types of applications is consumed by `String` objects. Further, roughly half of those `String` objects are duplicates, where duplicates means `string1.equals(string2)` is true. Having duplicate `String` objects on the heap is, essentially, just a waste of memory. This project will implement automatic and continuous `String` deduplication in the G1 garbage collector to avoid wasting memory and reduce the memory footprint.

目前，许多大规模的 Java 应用程序在内存上遇到了瓶颈。测量表明，在这些类型的应用程序中，大约 25%的 Java 堆实时数据集被`String'对象所消耗。此外，这些 "String "对象中大约有一半是重复的，其中重复意味着 "string1.equals(string2) "是真的。在堆上有重复的`String'对象，从本质上讲，只是一种内存的浪费。这个项目将在 G1 垃圾收集器中实现自动和持续的`String'重复数据删除，以避免浪费内存，减少内存占用。

---

注意这里说的重复，指的是在堆中的数据，而不是常量池中的，因为常量池中的本身就不会重复

背景：对许多 Java 应用（有大的也有小的）做的测试得出以下结果：

- 堆存活数据集合里面 string 对象占了 25%
- 堆存活数据集合里面重复的 string 对象有 13.5%
- string 对象的平均长度是 45

许多大规模的 Java 应用的瓶颈在于内存，测试表明，在这些类型的应用里面，<mark>Java 堆中存活的数据集合差不多 25%是 String 对象</mark>。更进一步，这里面差不多一半 string 对象是重复的，重复的意思是说： `stringl.equals(string2)= true`。<mark>堆上存在重复的 String 对象必然是一种内存的浪费</mark>。这个项目将在 G1 垃圾收集器中实现自动持续对重复的 string 对象进行去重，这样就能避免浪费内存。

**实现**

1. 当垃圾收集器工作的时候，会访问堆上存活的对象。<mark>对每一个访问的对象都会检查是否是候选的要去重的 String 对象</mark>
2. 如果是，把这个对象的一个引用插入到队列中等待后续的处理。一个去重的线程在后台运行，处理这个队列。处理队列的一个元素意味着从队列删除这个元素，然后尝试去重它引用的 string 对象。
3. 使用一个 hashtable 来记录所有的被 String 对象使用的不重复的 char 数组。当去重的时候，会查这个 hashtable，来看堆上是否已经存在一个一模一样的 char 数组。
4. 如果存在，String 对象会被调整引用那个数组，释放对原来的数组的引用，最终会被垃圾收集器回收掉。
5. 如果查找失败，char 数组会被插入到 hashtable，这样以后的时候就可以共享这个数组了。

**命令行选项**

```shell
# 开启String去重，默认是不开启的，需要手动开启。 UseStringDeduplication(bool)  # 打印详细的去重统计信息 PrintStringDeduplicationStatistics(bool)  # 达到这个年龄的String对象被认为是去重的候选对象StringpeDuplicationAgeThreshold(uintx)
```



### 常见面试题

