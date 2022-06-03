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

> 记住：线程就是一个单独的资源类，没有任何附属的操作！拿来即用，里面就包含 1.属性、2.方法