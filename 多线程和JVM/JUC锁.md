# JUC笔记

## 1.概念

JUC全称是：java.util.concurrent 

顾名思义这是一个包名，并且是一个工具类!

文档：

![image-20220526162212566](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202205261622593.png)

使用这个的原因：

按照之前的创建线程我们使用的是Thread，可是在实际的业务开发过程中，我们是无法通过普通的线程Thread去创建，但是这种效率并不是很高！而Runnable做的事情也无非就是重写run()然后丢进Thread类里去执行，而Runnable接口是没有返回值的，效率来讲相较比Callable来说比较低！而且功能也没有Callable强大！

### 1.1 进程与线程

进程：一个程序，是执行程序的一次执行过程；比如：qq.exe

线程：一个进程中有若干个线程，但是一个进程至少有一个线程，而线程是CPU调度器的执行单位！

> Java默认有几个线程？mian、gc 一个用户线程和一个守护线程

面试题：Java能开启线程吗？答案：不行！

```java
thread.start();
//点击start()进去

public synchronized void start() {
        //加入线程组进去
        group.add(this);

        boolean started = false;
        try {
            //点击start0()进去
            start0();
            started = true;
        } finally {
           //....
        }
}
//调用native本地方法也就是C++去执行的，因为Java是无法直接操作硬件的！
private native void start0();
理论：所以说我们的创建线程，其实就是调用C++去操作硬件去创建一个线程，然后开启通过CPU调度去执行！
```

> 并发、并行

并发（多线程操作同一个资源）

- CPU一核，模拟出来多条线程。在同一时间段，多个任务都在执行。宏观上是同时执行，微观上是顺序地交替执行，并发不一定等于并行。

并行（多个人一起行走）

- CPU多核，多个线程可以同时执行。单位时间内，多个任务同时执行

理解：并发是指一个处理器同时处理多个任务。
	 并行是指多个处理器或者是多核的处理器同时处理多个不同的任务。
并发是逻辑上的同时发生，而并行是物理上的同时发生

```java
//用Java来查看本机有多少处理器
public class Test1 {
    public static void main(String[] args) {
        //获取CPU的核数
        //CPU密集型、IO密集型
        System.out.println(Runtime.getRuntime().availableProcessors());
    }
}
//12
```

> 并发编程的本质：充分利用CPU的资源

线程到底是五种状态还是六种状态：

5种：从操作系统层面上来讲：

1. 新建状态（New）
2. 就绪状态（read）
3. 运行状态（Running)
4. 阻塞状态（blocked)
5. 死亡状态（terminated)

6种：Java线程的生命周期来讲：

```java
public enum State {
    // 新建状态
    NEW,

    // 运行状态（调用Thread.start())
    RUNNABLE,

    /**
     * 阻塞状态
     * Object.wait();获取到锁，等待进行synchronized方法、代码块
     */
    BLOCKED,

    /**
     *  等待状态(死等)
     *  Object.wait()
     *  Thread.join()
     *  LockSupport.park()
     */
    WAITING,

    /**
     *  限时等待状态（超时等）
     *  Thread.sleep(long)
     *  Object.wait(long)
     *  Thread.join(long)
     *  LockSupport.parkUntil()
     *  LockSupport.parkNanos()
     */
    TIMED_WAITING,

    // 终止状态
    TERMINATED;
}
```

> wait/sleep区别？

不同点：

1. 来自不同的类：wait是Object类，而sleep是Thread类的方法
2. 锁的释放不同：wait会释放锁、而sleep不会释放锁（抱着锁睡觉）
3. 使用的位置不同：wait只能在同步代码块中、而sleep可以在任何地方

共同点：

1. 两者都需要捕获异常：wait需要捕获异常，而sleep也需要捕获异常
2. 都是会让线程进入阻塞状态



一般我们如果想让程序睡眠一定时间，之前我们用的Thread.sleep去睡眠，那么接下来我们就可以使用JUC的工具类来操作

```java
TimeUnit.SECONDS.sleep(1); //中间是单位
```

## 2.Lock锁

> 记住：
>
> 1.线程就是一个单独的资源类，没有任何附属的操作！拿来即用，里面就包含 1.属性、2.方法
>
> 2.并发：多线程同时操作同一个资源类，并把资源放在线程中

先看官方给的源码和文档：

1. 使用方式：

   ![image-20220620174629721](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202206201746833.png)

2. 实现类:

   ![image-20220620175039946](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202206201750003.png)

​	我们工作开发过程中大多数常用的是【可重复锁】

3. 公平与非公平锁：

   ![image-20220620175544961](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202206201755011.png)

   解释：1.**公平锁**：非常公平，必须按照先来后到进行排队

   ​	 2.**非公平锁**：不公平，可以插队！（默认）

   理解：可以理解为现在有两个进程。A进程需消耗30h，B进程需消耗3s，如果按照公平锁的方式，如果先执行的是A进程那么就需要等30个小时才会执行B进程，也就是说CPU不会按照随机的那种，而非公平锁就	是按照CPU的调度进行选择！

	### 2.1 synchronized与Lock区别

1. synchronized是关键字，lock是接口

2. synchronized自动释放锁（执行完代码块自动释放锁哪怕遇到异常），lock需要手动释放锁（必须在finaly中释放锁不然遇到异常容易造成**死锁**）

3. synchronized无法判断获取锁的状态，Lock可以判断是否获取到了锁

4. synchronized线程被占用后续线程进行阻塞，Lock不一定会阻塞

   ```java
   1.synchronized第一个线程遇到执行体中，但还没释放锁那么后续线程就会处于锁等待
   2.Lock有一个方法
       lock.tryLock() //该方法会尝试获取到锁，获取到锁返回true，没获取到锁返回false，不会进行锁等待
   3.常见代码方式：
       Lock lock = ...; 
   	if (lock.tryLock()) {  //尝试获取锁，如果没获取到就执行else里面的逻辑
           try { 
               // manipulate protected state 
           } finally { 
               lock.unlock(); 
           } 
       } else { 
           // perform alternative actions 
       }   
   ```

5. synchronized 可重入锁，不可以中断（因为是关键字）非公平；Lock 可重入锁，可以中断(可以判断锁) 非公平(既可以非公平也可以公平)

6. synchronized 适合少量的代码同步问题，Lock适合大量的代码同步问题

7. synchronized 只有两种方式，而Lock灵活度较高！

    

### 2.2 生产者与消费者问题

> 传统的方式：synchronized与this.wait()与this.notifyAll()

```java
public class A {
    public static void main(String[] args) {
        Data data = new Data();
        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "A").start();

        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "B").start();

        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "C").start();

        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "D").start();
    }
}

class Data {
    private int number = 0;

    public synchronized void increment() throws InterruptedException {
        if (number != 0) {
            //等待
            this.wait();
        }
        number++;
        System.out.println(Thread.currentThread().getName() + "=》" + number);
        this.notifyAll();
    }

    //-1
    public synchronized void decrement() throws InterruptedException {
        if (number == 0) {
            this.wait();
        }
        number--;
        System.out.println(Thread.currentThread().getName() + "=》" + number);
        this.notifyAll();
    }
}
```

整个过程是通过wait()等待和notifyAll()来等待和唤醒来进行切换生产者与消费者之间的问题的！

看输出：

```
A=》1
B=》0
//......
B=》0
C=》1
A=》2
C=》3
D=》2
D=》1
D=》0
B=》-1
D=》-2
D=》-3
D=》-4
D=》-5
D=》-6
D=》-7
D=》-8
C=》-7
A=》-6
```

可以发现怎么就会负数了呢？其实这里就会提及一个概念`虚假唤醒`

解释：

```
虚假唤醒：
	在一个线程没有被其他线程调用notify()、notifyAll()方法进行通知，或者被中断、或者等待超时。那么这个线程依然可以从挂起状态变为可运行状态（也就是被唤醒）---》这就是所谓的虚假唤醒
自我理解：
	当一个条件满足时，很多线程都被唤醒了，但是只有其中部分是有用的唤醒，其它的唤醒都是无用功。可以理解为超时进了一批货然后用广播通知了所有人，但只能卖给1个人，导致很多人是无用唤醒！
为什么if就会出现虚假唤醒：
	因为if只会执行一次，执行完就会接着往下执行if()外边的，也就是说该线程被唤醒得到锁之后就会执行if语句块之外的代码！
个人想法执行流程：
	当用notifyAll()唤醒所有线程，这时只有一个线程能竞争到锁，因为锁只有一把那么其他线程就会往下进行资源消费，在wait()处唤醒就开而if只会执行一次，执行完会接着往下执行if(){}后边的逻辑。而把wait()放在while循环中，那么即使有些线程被无用唤醒那么也会无限循环去获取条件让自身线程进行wait()阻塞等待！wait被唤醒也要循环去判断条件然后进行阻塞等待！
资料：
	就是用if判断的话，唤醒后线程会从wait之后的代码开始运行，但是不会重新判断if条件，直接继续运行if代码块之后的代码，而如果使用while的话，也会从wait之后的代码运行，但是唤醒后会重新判断循环条件，如果不成立再执行while代码块之后的代码块，成立的话继续wait
```

解决：

```java
将if改为while->就是不停的去测试该线程被唤醒的条件是否满足！
synchronized (obj) {
  //do something
  while (条件不满足){
    obj.wait();
  }
}
```

画图解析：

![image-20220621105731774](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202206211057838.png)

> 现代版的：Lock与await()、signal()

![image-20220621144824309](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202206211448411.png)

```
注意官方文档中有这样这句话：
	Lock替换synchronized方法和语句的使用，Condition取代了对象监视器方法的使用。
所以本质上来讲跟Synchronized是一样的！
await()->wait()-》阻塞等待
signal()->notify()-》唤醒等待的一个线程
signalAll()->notifyAll()->唤醒等待的全部线程
```

代码：

```java
package com.example.springbootproject.juc;

import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class B {
    public static void main(String[] args) {
        Data2 data = new Data2();
        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "A").start();

        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "B").start();

        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "C").start();


        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "D").start();
    }
}

class Data2 {
    private int number = 0;

    Lock lock = new ReentrantLock();
    Condition condition = lock.newCondition();


    public void increment() throws InterruptedException {
        lock.lock();
        try {
            while (number != 0) {
                //等待
                condition.await(); //阻塞等待
            }
            number++;
            System.out.println(Thread.currentThread().getName() + "=》" + number);
            condition.signalAll(); //唤醒全部
        } finally {
            lock.unlock();
        }
    }

    //-1
    public void decrement() throws InterruptedException {
        lock.lock();
        try {
            while (number == 0) {
                //等待
                condition.await();
            }
            number--;
            System.out.println(Thread.currentThread().getName() + "=》" + number);
            condition.signalAll();
        } finally {
            lock.unlock();
        }
    }
}
```

结果：

![image-20220621154109173](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202206211541849.png)

可以发现从结果上来讲确实达到了消费者和生产者之间的问题，并没有造成数据异常的问题，但现在又有新的问题，那就是阻塞等待被唤醒的是随机的，也就是说是`无序`的，那么能不能有序呢？答案是可以的！

> Condition:精准的通知和唤醒

代码：

```java
package com.example.springbootproject.juc;

import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class C {
    public static void main(String[] args) {
        Data3 data = new Data3();
        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    data.printB();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "B").start();

        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    data.printA();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "A").start();


        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    data.printC();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "C").start();
    }
}

class Data3 {
    private int number = 1;

    Lock lock = new ReentrantLock();
    //new了三个监视器
    Condition condition1 = lock.newCondition(); 
    Condition condition2 = lock.newCondition();
    Condition condition3 = lock.newCondition();


    public void printA() throws InterruptedException {
        lock.lock();
        try {
            while (number != 1) {
                //condition1阻塞等待
                condition1.await();
            }
            System.out.println(Thread.currentThread().getName() + "=》" + "AAAAAAAAAAA");
            number = 2;
            //condition2唤醒
            condition2.signal();
        } finally {
            lock.unlock();
        }
    }

    public void printB() throws InterruptedException {
        lock.lock();
        try {
            while (number != 2) {
                //等待
                condition2.await();
            }
            System.out.println(Thread.currentThread().getName() + "=》" + "BBBBBBBBBBBB");
            number = 3;
            condition3.signal();
        } finally {
            lock.unlock();
        }
    }

    public void printC() throws InterruptedException {
        lock.lock();
        try {
            while (number != 3) {
                //等待
                condition3.await();
            }
            System.out.println(Thread.currentThread().getName() + "=》" + "CCCCCCCCCCCCCCC");
            number = 1;
            condition1.signal();
        } finally {
            lock.unlock();
        }
    }
}
```

执行过程：

因为初始值为1

1. 假设CPU调度走B
   - 此时因为B!=2走进代码体让B线程陷入阻塞等待
   - 然后CPU调度走C，而此时C也会满足!=3的条件陷入阻塞等待
   - 还剩下一个A，此时满足!=1的条件，此时打印输出语句，然后唤醒B线程
2. 此时B线程被唤醒
   - 从`condition2.await();`醒后循环判断条件满足!=2
   - 打印输出语句，然后赋值初始值
   - 唤醒C线程
3. 此时C线程被唤醒
   - 重复B线程的执行流程
   - 最后唤醒A线程
4. 重复循环执行！有序进行。无论谁先调用都会按照执行顺序进行精准通知！

> 设置多个监视器去监视对应的资源，这样就保证了精准的调用

> Condition比传统的方式除了是替换还有升级，那就是精准通知



## 3. 八锁现象

理解八锁现象就得先理解：

1. 如何判断锁的是谁？
2. 知道什么是锁？

`锁只会锁的是对象、Class`

**通过案例来深刻理解锁，其实就是理解锁的8个问题**

> 1. 标准情况下两个线程先打印 发短信还是打电话？
> 2. 在sendSms方法内加延迟4秒 结果是发短信还是打电话？

```java
public class Test1 {
    public static void main(String[] args) {
        //同一个资源类
        Phone phone = new Phone();
        //A线程
        new Thread(()->{phone.sendSms();},"A").start();

        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
		//B线程
        new Thread(()->{phone.call();},"B").start();
    }
}

class Phone{
    //synchronized锁的对象是方法的调用者，是iphone
    //两个方法用的是同一个锁，谁先拿到谁执行！
    public synchronized void sendSms(){
        //这里第二个问题加延迟：
        //try {
           // TimeUnit.SECONDS.sleep(4);
        //} catch (InterruptedException e) {
          //  throw new RuntimeException(e);
        //}
        System.out.println("发短信");
    }

    public synchronized void call(){
        System.out.println("打电话");
    }
}
//结果：
//发短信
//打电话
```

解释：

1. 无论我们点多少次运行那么都是相同的结果，其根本原因在于`锁的存在`，可以发现`synchronized`修饰的方法锁的对象是方法的调用者，由于这个两个方法都是用的`同一把锁`所以说谁先拿到就谁先执行！
2. 第二个问题在第一个基础上回答，这里加了延迟不会影响结果，问题就在于谁先拿到锁后进入方法进行sleep休眠，注意`sleep休眠是不会释放锁`的，注意这里锁住的对象只有一个，那么也就是说只是延迟了片刻就执行！

补充：这里我个人认为并不是每次都是相同结果，因为现在电脑性能较好核数增加导致现在看起来每次按顺序调用都是执行1.发短信 2.打电话（因为多核的原因大概率会顺序执行，再加上这里还延迟一秒，更大机会是顺序执行），从技术理论上来讲确实有可能会出现1.打电话 2.发短信 因为线程启动并不会立即执行而是会等待CPU的调度！所以不存在多个线程调用同一个对象就会按照顺序执行或者说先调用的先执行，而是因为电脑的多核问题是可以并行通过的，所以很大概率来讲是按照顺序执行的！

求助：这里我咨询相关大佬得出：

 	1. 首先这里会先执行“发短信”是因为有延迟一秒，也正是因为这1秒本身来说就可以做很多的事情，也就是理解为在这1秒之前CPU就已经调度执行了！所以就没必要去跟第二个线程去竞争！
 	2. 如果去掉这个延迟那么也是先打印“发短信”是因为两个线程启动完毕之后，等待CPU调度执行但是这里因为第一个线程因为顺序的问题先启动那么它被先被执行的概率比较大！就好比是比如现在有4个跑道，A在1号跑道已经开始跑了，B才再2号跑道开始跑，虽然是并行状态但是A的启动时间更早一点！

**因为线程本身理论较多所以每个人都有自己的理解，所以这里融合一下得出精华即可！**

> 3. 增加一个普通方法，是先打印发短信还是hello？

```java
public class Test1 {
    public static void main(String[] args) {
        Phone phone = new Phone();
        
        new Thread(() -> {phone.sendSms();}, "A").start();

        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        
        new Thread(() -> {phone.hello();}, "B").start();


    }
}

class Phone {
    public synchronized void sendSms() {
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        System.out.println("发短信");

    }
	//这里没有锁！不是同步方法，不受锁的影响
    public void hello() {
        System.out.println("hello");
    }
}
//结果：
//1.hello
//2.发短信
```

解释：这里是因为发短信有延迟虽然没有释放锁，但hello()方法没有上锁意义上来讲是不受影响的，但是如果这里去掉了延迟，那么依然是1.发短信、2.hello，因为两个线程启动随CPU调度执行其中一个，而加锁是为了防止并发！

> 4. 两个对象两个同步方法，是先执行发短信还是打电话？

```java
public class Test1 {
    public static void main(String[] args) {
        Phone phone = new Phone();
        Phone phone1 = new Phone();
        new Thread(() -> {phone.sendSms();}, "A").start();


        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        new Thread(() -> {phone1.call();}, "B").start();


    }
}
class Phone {
    public synchronized void sendSms() {
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        System.out.println("发短信");

    }

    public synchronized void call() {
        System.out.println("打电话");
    }
}
//结果：
//1.打电话
//2.发短信
```

解释：因为synchronized方法锁住的是对象，而这里new了两个对象那么一个对象就对应一把锁所以这里就有两把锁，所以这里两个方法的锁是一个平行的关系，也就是说首先两个线程启动CPU调度选择其中一个，如果是发短信，那么就会延迟，这时另外一个线程就开始执行，不会受到另外一个对象锁的影响！因为压根就锁不住！

> 5. 增加两个静态的同步方法，只有一个对象，是先打印的发短信还是打电话？

```java
public class Test1 {
    public static void main(String[] args) {
        Phone phone = new Phone();
        new Thread(() -> {phone.sendSms();}, "A").start();


        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        new Thread(() -> {phone.call();}, "A").start();
    }
}

class Phone {
    //这里static修饰，表示类一加载就有了，所以这里锁的是Class模板
    public static synchronized void sendSms() 1{
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        System.out.println("发短信");

    }

    public static synchronized void call() {
        System.out.println("打电话");
    }
}
//结果：
//发短信
//打电话
```

解释：因为这里是static静态修饰，所以类一加载就有了那么这里锁的就不是之前new出来的对象了，而是class模板，而一个类的class模板又是唯一的，所以结论：锁住的是class并且是唯一的！

> 6. 两个对象，增加两个静态的同步方法，先打印 发短信还是打电话？

```java
public class Test1 {
    public static void main(String[] args) {
        Phone phone1 = new Phone();
        Phone phone2 = new Phone();
        new Thread(() -> {phone1.sendSms();}, "A").start();


        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        new Thread(() -> {phone2.call();}, "A").start();
    }
}

class Phone {
    //这里static修饰，表示类一加载就有了，所以这里锁的是Class模板
    public static synchronized void sendSms() 1{
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        System.out.println("发短信");

    }

    public static synchronized void call() {
        System.out.println("打电话");
    }
}
//结果：
//发短信
//打电话
```

解释：无论呢new了多少个对象，static修饰并且是同步方法块 那么就是锁住的是class模板，而模板又是唯一的，所以这里始终锁的都是同一个

> 7. 1个静态的同步方法，1个普通的同步方法，一个对象， 先打印发短信还是打电话？

```java
public class Test1 {
    public static void main(String[] args) {
        Phone phone = new Phone();
        new Thread(() -> {phone.sendSms();}, "A").start();


        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        new Thread(() -> {phone.call();}, "A").start();
    }
}

class Phone {
    public static synchronized void sendSms() {
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        System.out.println("发短信");

    }

    public  synchronized void call() {
        System.out.println("打电话");
    }
}
//结果：
//打电话
//发短信
```

解释：这里就能明显知道发短信是静态同步方法锁的是class模板，而打电话只是普通的同步方法锁住的是对象，两者锁的都不一样，所以发短信有延迟然后CPU切换为这个线程执行！

> 8. 1个静态的同步方法，1个普通的同步方法，2个对象， 先打印发短信还是打电话？

```java
public class Test1 {
    public static void main(String[] args) {
        Phone phone1 = new Phone();
        Phone phone2 = new Phone();
        new Thread(() -> {phone1.sendSms();}, "A").start();


        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        new Thread(() -> {phone2.call();}, "A").start();
    }
}

class Phone {
    public static synchronized void sendSms() {
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        System.out.println("发短信");

    }

    public  synchronized void call() {
        System.out.println("打电话");
    }
}
//结果：
//打电话
//发短信
```

解释：还是一样锁的不一样，区别不大！

> 小结

锁到底是锁的是什么？

锁无非就是锁的是`new`和`static`，前者比如像this这些代表着一个具体的对象，而后者static指的class模板是全局唯一的！

对象-》可以有多个

class-》全局唯一

## 4.不安全的集合类(重点)

在一些并发中如果用到集合，那么集合也是存在并发安全的，所以也需要注意下！

### 4.1 List

先看一组不安全的！

```java
public class list {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();

        for (int i = 0; i < 10; i++) {
            new Thread(()->{
                list.add(UUID.randomUUID().toString().substring(0, 5));
                System.out.println(list);
            },String.valueOf(i)).start();
        }
    }
}
```

异常:

![image-20220623092015398](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202206230920574.png)

这里循环创建线程去添加到这个list中，可以发现这里报了异常`ConcurrentModificationException并发修改异常`

#### Vector()

> 解决方案： 1、new Vector();

将ArrayList替换为Vector之后运行就正常了，那为什么呢？点进源码来看看：

![image-20220623093947306](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202206230939718.png)

此时就会发现add()方法被加上了synchronized，之前就说synchronized修饰方法锁住的是对象的调用者，所以此时我们无论循环创建线程模拟并发，因为操作的是同一个对象，只有一把锁无论谁先执行那么另一个就得等待执行完毕等待锁释放才能进入，当然要记住被频繁加锁`性能比较差的！`

问题：为什么现在常用的是ArrayList而不是Vector？

```
来看一下两个结构出现的版本：
Vector：JDK1.0就出来了
ArrayList：add()方法是随着版本一起出来的，是在JDK1.2
```

所以就真的是开发者他们就真的没想到吗？不是的！

而且这里开发者也提到了：

![image-20220623095544060](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202206230955494.png)

作者知道不安全但考虑了性能优化的问题进行均衡！这里它也提到专门有线程安全的List进行使用！

#### Collections.synchronizedList()

> 解决方案： 2、List list = Collections.synchronizedList(new ArrayList(...));

注意：这里由于synchronizedList是Collections的静态内部类，所以我们要通过Collections来调用!

看下官方文档推荐的写法：

![image-20220623100336240](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202206231003639.png)

​	SynchronizedList是一个基于`同步锁`实现的List，是Collections工具类里面的一个静态类，与ArrayList相比，它是线程安全的，与Vector相比他性能又要好一些，`因为Vector实现同步锁是基于方法级别，而SynchronizedList实现同步锁是在代码级别范围更小，性能更高效`。

看一下关键源码：

```java
 static class SynchronizedList<E> extends SynchronizedCollection<E> implements List<E> {
        private static final long serialVersionUID = -7754090372962971524L;
		//一个final修饰的list，表示list一旦传递了引用就不可变。也就是说原本的参数list引用传递给SynchronizedList的list并不可变
        final List<E> list;
		
        SynchronizedList(List<E> list) {
            //调用父类SynchronizedCollection的构造方法
            super(list);
            this.list = list;
        }
 }
```

点击进入`super(list)`去父类中查看：

```java
static class SynchronizedCollection<E> implements Collection<E>, Serializable { 
    	private static final long serialVersionUID = 3053995032091335093L;

        final Collection<E> c;  // Backing Collection
        final Object mutex;     //这里mutex是synchronized本地的对象作为同步锁 

        SynchronizedCollection(Collection<E> c) {
            //判断非空就复制
            this.c = Objects.requireNonNull(c);
            //使用当前对象来作为一个互斥锁
            mutex = this;
        }
		//....
}
```

再来看Collections.synchronizedList关键方法_同步：

```java
		public int size() {
     		synchronized (mutex) {return c.size();}
        }
        public boolean isEmpty() {
            synchronized (mutex) {return c.isEmpty();}
        }
        //....
        public Iterator<E> iterator() {
            return c.iterator(); // Must be manually synched by user!
        }

        public boolean add(E e) {
            synchronized (mutex) {return c.add(e);}
        }
        public boolean remove(Object o) {
            synchronized (mutex) {return c.remove(o);}
        }
```

> 但是有一个地方很值得注意的是迭代器的使用是没有上锁的，而且源码是标注了必须用户自己去做同步处理，也就是说需要自己手动加锁!
>
> 并且在官方文档示例中也提到迭代器是需要在外面手动加锁的！

如：

```java
List list = Collections.synchronizedList(new ArrayList());
//注意：synchronized持有的监视器对象必须是synchronized (list),即包装后的list,使用其他对象如synchronized (new Object())会使add,remove等方法与迭代方法使用的锁不一致，无法实现完全的线程安全性
//必须对list进行加锁
synchronized (list) {
  Iterator i = list.iterator();
  while (i.hasNext())
      foo(i.next());
}
```

那问题来了为什么作者不在里面加锁呢？

​	**未加锁**：是因为同步容器类的数据量很大，迭代占用时间较长，而迭代过程中可能会出现别的线程修改了容器的数据，而这样迭代的时候可能会抛出异常(比如迭代到索引是最后一个元素的时候，这时候另外一个线程删除了这个线程，这个时候就会抛出数组越界的异常)。

​	**加锁**：但是迭代过程中加锁，那么就会出现性能问题，其中一种解决方案就是克隆容器，再迭代克隆的容器(克隆期间需要加锁)，但是克隆也是耗费CPU性能的。所以来讲确实没有十全十美的办法！

总的来说就是作者认为不加锁但是数据量大耗时长容易被修改数据造成异常，而加锁就会加重耗费CPU性能，所以他选择放开，让用户根据自己业务实际情况来考虑！

注意：

我们在Java中的foreach循环也是迭代器进行循环的：

```java
private void forEachTest(){
    List<Integer> list=new ArrayList();
    
    Integer var3;
    for(Iterator var2=list.iterator();var2.hasNext();var3=(Integer)var2.next()){
        ;
    }
}
//这是反编译class看到的，所以可以发现如果用了Collections.SynchronizedCollection线程容器类的话就要注意了，避免引起ConcurrentModificationException并发修改异常
```

#### CopyOnWriteArrayList()

> 解决方案：3、List<String> list = new CopyOnWriteArrayList<>();

注意这是JUC并发包下的线程安全集合；

CopyOnWrite写入时复制，这里在计算机领域中还统称为COW，是计算机程序设计领域的一种优化策略；

来看下这个类的定义和数据结构：

```java
public class CopyOnWriteArrayList<E> implements List<E>, RandomAccess, Cloneable, java.io.Serializable {
    private static final long serialVersionUID = 8673264195747942595L;

    /** The lock protecting all mutators */
    transient final ReentrantLock lock = new ReentrantLock();

    /** The array, accessed only via getArray/setArray. */
    // 存储数据的array数组，注意此处是用volatile修饰的
    private volatile transient Object[] array;
}
```

再来看add核心方法：

```java
public boolean add(E e) {
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
            //获取旧数组
            Object[] elements = getArray();
            int len = elements.length;
            //使用数组复制的方式将旧数组复制为新的数组并且长度+1
            Object[] newElements = Arrays.copyOf(elements, len + 1);
            //将存入的数据放入到新数组中去
            newElements[len] = e;
            //将新数组覆盖旧数组
            setArray(newElements);
            return true;
        } finally {
            lock.unlock();
        }
}
```

可以发现：

1. add方法通过ReentrantLock保证同一时刻最多只有一个线程向list中添加元素，肯定是线程安全的！
2. 并不是直接往数组中添加元素，而是开辟新数组，把元素插入新数组，再用新数组替换旧数组

思考：既然ReentrantLock已经保证了线程安全，为什么还需要开辟新数组？

​	因为volatile修饰数组时，仅能保证**数组的引用**具有volatile语义。也就是说volatile修饰的数组，即使数组中的元素被改变了，也不会触发可见性

想要解决这个问题有两种办法：

1. 使用AtomicIntegerArray或者AtomicLongArray
2. 修改数组的内存地址，也就是对数组进行重新赋值

> 可以得出都是通过 ReentrantLock+Volatile+数组拷贝来保证线程安全的！

优点：

1. 线程安全
2. 提高了“读”操作的并发度，并且不会造成线程安全问题
3. 迭代不会发生ConcurrentModificationException异常，因为迭代的是原数组，而所有的变化都发生在原数组的副本上，对于迭代器来说迭代的集合结构并不会发生变化

缺点：

1. 虽然底层是数组但是没有扩容这一说法，导致每次add都会开辟新的数组！相当于浪费了一倍的空间
2. 每次add和remove都会进行加锁和拷贝数组，所以效率是比较低的！
3. 无法保证实时性，因为“读”和“写”不在同一个数组，且“读”操作没有加互斥锁，所以无法保证`强一致性`，只能保证`最终一致性`

注意：最好不要在循环中add()方法，否则就会产生耗时操作:

```java
/**
 * 循环 + add vs addAll
 */
public class CopyOnWriteArrayListDemo {

    private static final int COUNT = 100000;
    private static final List<Integer> list1 = new CopyOnWriteArrayList<>();
    private static final List<Integer> list2 = new CopyOnWriteArrayList<>();

    public static void main(String[] args) {
        List<Integer> dataList = new ArrayList<>(COUNT);
        for (int i = 0; i < COUNT; i++) {
            dataList.add(i);
        }

        testCopyOnWriteArrayList(dataList);
    }

    private static void testCopyOnWriteArrayList(List<Integer> dataList) {
        long time1 = System.currentTimeMillis();
        for (Integer data : dataList) {
            list1.add(data);
        }
        long time2 = System.currentTimeMillis();
        System.out.println("循环+add 耗时：" + (time2 - time1) / 1000.0 + " 秒");
        list2.addAll(dataList);
        long time3 = System.currentTimeMillis();
        System.out.println("addAll  耗时：" + (time3 - time2) / 1000.0 + " 秒");
    }
}
//结果：
循环+add 耗时：3.846 秒
addAll  耗时：0.004 秒
```

可以直观的发现如果循环10万条数据去不断的copy开辟新的空间，就会造成耗时操作！所以最好使用addAll()只需要开辟一次避免了不停的耗时操作！

CopyOnWriteArrayList利用**ReentrantLock + volatile + 数组拷贝**实现了线程安全的ArrayList。在特定的场景下使用CopyOnWriteArrayList既能保证线程安全，又能有较好的表现。

### 4.2 Set

Set也是一组不安全的集合，比如这里：

```java
public class SetTest {
    public static void main(String[] args) {
        Set<String> set = new HashSet<>();
        
		//循环创建线程去修改
        for (int i = 0; i < 10; i++) {
            new Thread(() -> {
                set.add(UUID.randomUUID().toString().substring(0, 5));
                System.out.println(set);
            }, String.valueOf(i)).start();
        }
    }
}
```

![image-20220625113858112](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202206251138352.png)

可以发现这里出现并发修改异常，也就导致在并发环境下Set不是安全的！

#### Collections.synchronizedSet()

> 解决方案：1、Set<String> synchronizedSet = Collections.synchronizedSet(new HashSet<>());

这个本质跟List的Collections.synchronizedList是一样的，利用顶部集合工具类Connections来处理！

![image-20220625114948715](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202206251149817.png)

这就是`synchronizedSet`的全部了，而其中的add这些操作方法都是由父类`SynchronizedCollection`来控制。

#### CopyOnWriteArraySet()

> 解决方案：2.Set<String> synchronizedSet = new CopyOnWriteArraySet<>();

我们来看下这个构造方法：

```java
public class CopyOnWriteArraySet<E> extends AbstractSet<E> implements java.io.Serializable {
    private static final long serialVersionUID = 5457747651344034263L;

    private final CopyOnWriteArrayList<E> al;

    /**
     * Creates an empty set.
     */
    public CopyOnWriteArraySet() {
        al = new CopyOnWriteArrayList<E>();
    }
}
```

此时我们就可以发现这里set的构造方法指的是new了一个`CopyOnWriteArrayList`！

```java
//CopyOnWriteArraySet
public boolean add(E e) {
    return al.addIfAbsent(e);
}
```

再点进去：

```java
//CopyOnWriteArrayList
 public boolean addIfAbsent(E e) {
        Object[] snapshot = getArray();
        return indexOf(e, snapshot, 0, snapshot.length) >= 0 ? false :
            addIfAbsent(e, snapshot);
}

    /**
     * A version of addIfAbsent using the strong hint that given
     * recent snapshot does not contain e.
     */
    private boolean addIfAbsent(E e, Object[] snapshot) {
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
            Object[] current = getArray();
            int len = current.length;
            if (snapshot != current) {
                // Optimize for lost race to another addXXX operation
                int common = Math.min(snapshot.length, len);
                for (int i = 0; i < common; i++)
                    if (current[i] != snapshot[i] && eq(e, current[i]))
                        return false;
                if (indexOf(e, current, common, len) >= 0)
                        return false;
            }
            Object[] newElements = Arrays.copyOf(current, len + 1);
            newElements[len] = e;
            setArray(newElements);
            return true;
        } finally {
            lock.unlock();
        }
}
```

得出：这里set其实本质上还是调用的是CopyOnWriteArrayList中的方法，这是外面套了一层壳而已！都是一样的！

其他：

来看一下HashSet的源码：

```java
public HashSet() {
      map = new HashMap<>();
}

public boolean add(E e) {
        return map.put(e, PRESENT)==null;
}
```

> 得出：Set本质是HashMap，而Set在add的过程中key就是传递的值，而value是固定的Object！

### 4.3 Map

