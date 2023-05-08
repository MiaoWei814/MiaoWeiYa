# 调试玩法

只记录有用的，常规玩法就忽略不计！

断点常用截图：

![img](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305041110736.png)



## 方法断点

> 查看方法进入时和退出时参数变化

![image-20230504110410780](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305041104042.png)

![image-20230504110524129](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305041105203.png)

> 接口上打断点自动跳转到对应的实现类

![image-20230504110627858](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305041106904.png)

比如这种：

![img](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305041111425.png)

## 异常断点

步骤：

1. 比如这段代码 明眼上会报错空指针，然后你运行了一次报错了 但是你不知道在哪报的错 这时

![image-20230504110720062](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305041107106.png)

2. ![image-20230504110815545](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305041108592.png)

3. ![image-20230504110822511](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305041108562.png)

4. 这里搜索你刚刚或者说你觉得会报什么异常，比如空指针 或者你自定义的异常也可以搜得到

   ![image-20230504110843912](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305041108949.png)

![image-20230504110849823](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305041108874.png)

5. 在程序运行时如果发生异常报错了 那么就直接给你打断点急刹车 你就知道在哪行报错了

## 字段断点

![img](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305041113421.png)

一个成员变量被多方引用时，它可以精准的找到谁读取、修改了它的值。

示例：

![img](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305041113582.gif)

可以看到，精准的定位到 hello 被赋值的位置



## 避免操作资源

1. 第一步：

![image-20230504113329238](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305041133276.png)

2. 第二步：

![image-20230504113628173](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305041136245.png)

3. 最终结果：

   ![image-20230504113711145](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305041137174.png)

## 图标含义

### 1. 回到断点处

![image-20230504114251404](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305041142429.png)

在后面阅读源代码的场景中，当进入断点后我们自己跳转深沉处的代码行去了，那么想要回到当前断点处就可以直接点击此处图标就可回，这样就避免了去挨个挨个找对应的代码文件中了。

### 2. 执行到指定行

![image-20230504133627249](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305041336286.png)

鼠标悬停在某一代码行，点击此图标即可快速让断点执行到某处！

## Stream流调试

```java
public static void streamChain(){
        Arrays.asList(1, 2, 3, 45).stream()
                .filter(i -> i % 2 == 0 || i % 3 == 0)
                .map(i -> i * i)
                .forEach(System.out::println);
    }
```

1. 具体断点：

   ![image-20230504135350923](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305041353955.png)

这种就是类似于for循环内的断点了

2. 行断点:

   当打了行断点后注意看此处就有个图标：*跟踪当前链条*

   ![image-20230504140429720](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305041404747.png)

​	![image-20230504140815678](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305041408720.png)

就可以流的过滤之间的变化，并且左下角换个视角：

![image-20230504140841087](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202305041408126.png)