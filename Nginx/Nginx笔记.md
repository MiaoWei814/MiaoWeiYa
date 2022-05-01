# Nginx-反向代理

## 1. 概念

### 1.1 前言

1. 单体项目

![image-20220423151630517](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20220423151630517.png)

​	经典就是将项目打包成jar包放在服务器上，用户去访问服务器上tomcat返回查询返回！这种呢并发量小，适合项目初期

2. 第二阶段

这种项目到一定发展的阶段就无法支持接下来的使用了，因为用户量的增加导致并发增加，那么传统的单体服务器就无法支撑这个并发，那么此时我们就横向扩展，一个服务器不够那我们多增加几台，可是增加了之后用户在选择不同的服务器就要做选择，那么此时对用户来说体验也不好并且众所都知session是无法共享的！所以此时我们就需要在中间增加一个`代理服务器`,通过一个代理服务器来进行选择性的将请求分发到其中一个服务器！

![image-20220423155443920](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20220423155443920.png)

我们希望这个代理服务器可以帮助我们接收用户的请求，然后将用户的请求按照规则帮我们转发到不同的服务器节点上。这个过程用户是无感知的，用户并不知道是哪个服务器返回的结果，我们还希望他可以按照服务器的性能提供不同的权重选择，保证最佳体验！所以我们使用了Nginx。

### 1.2 解释

1. 什么是Nginx？
   - Nginx是一个`高性能`的HTTP和反向代理`Web服务器`，同时也提供了IMAP/POP3/`SMTP`服务。
2. 特点：
   - 占用内存少、并发能力强、启动容易可以不间断运行！

理解：

1. 高性能：说明请求和响应是很快的！
2. web服务器：说明本身它就是一个服务器，用于反向代理转发请求
3. SMTP：说明可以用来提供邮件等服务

> 官方统计：Nginx能够支持高达5千个并发连接数的响应
>
> tips:Tomcat最多也就支持五六百个

### 1.3 正反向代理

其实无论是正反向代理，其实都是一个关键字 `代理`，其实无非就是替别人干活的意思，在日常生活中很多场景：美团跑腿、游戏代练、海外代购等等，这些都算！只不过正向和反向只是一个`相对`的概念而已！

1. 正向代理：

   ![image-20220423163348003](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20220423163348003.png)

   解释：当我们访问海外的服务器比如美国的服务器那么就会被拦截拒绝，那么此时我们开启一个VPN那么就可以访问了，那么此时就你的客户端去访问VPN代理服务器，而这个VPN的服务器呢比如就在香港，那么就会访问香港，而香港呢就可以直接访问外网就去访问美国的服务器，

   作用：正向代理隐藏了用户，用户的请求被代理服务器接收代替，到了服务器，服务器并不知道用户是谁

   ```java
   理解：伪造了客户端身份的，就是正向代理
   	某用户小A，不想要某网站发现他的ip登录过，使用代理ip以后，他在互联网中的所作所为，就好像都是那个代理IP在做的，
   代理ip代替着小A，互联网中的各个server们只知道代理ip来过，不知道小A来过
   又比如一些网站禁止一个ip访问太频繁，但client方的小B又需要频繁获取这个网站的信息，那么"正向代理"的代理ip就起作用了，小B只需要频繁更换代理ip来伪装是很多client访问网站就行了。    
   ```

   生活中场景：

   ​	比如，想买烟的未成年和不准卖烟给未成年的烟店老板，一个是client，一个是server，这个未成年需要买烟的话，就不能以自己的身份来买，所以他需要叫个代跑腿的成年小哥，这个时候这个跑腿小哥就是未成年小伙伪造的"client"，属于`正向代理`

2. 反向代理:

![image-20220423165905605](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20220423165905605.png)

解释：反向代理说白了就是伪造隐藏了服务器，对外只公布了一个代理服务器，对用户来说只访问这个代理就足以，哪怕这个代理服务器挂了那么只需要再加一个代理服务器就可以继续工作了；

举个生活上的例子：

​	就好比我们我们生活中的中介一样的意思！比如用户只需要找到这个房产中介就能了解到附近区域的房子，而中介就能根据爱好去匹配对应的房源，对于房源房东来说，我只需要接收你这个中介的请求就行了，而对于用户来说就不用去挨个挨个找房东！哪怕这个中介临时有事，对于房东来说立马找个中介又可以了！

作用：用户请求过多，服务器会有一个处理的极限。所以使用反向代理服务器接受请求，再用均衡负载将请求分布给多个真实的服务器。既能提高效率还有一定的安全性。

> 总结：
>
> 1. 正向代理隐藏的是用户，反向代理隐藏的是服务器
> 2. client使用正向代理隐藏了自己的真实身份，server用反向代理保护了server的安全
> 3. 个人感觉比较经典的正向代理就是代理ip，反向代理就是 前台nginx转发后台主服务器的架构了

​	正向代理其实就是用户主动使用代理，这个代理对用户来说是透明的，对用户来说，这个代理属于正向。

​	反向代理是与正向代理相反，这个代理是用户不知道的，都是在服务器端自己做的处理，这个代理对用户来说，是反向的。重点区分是要`看这个代理是在用户端还是在服务端`。

相关解释链接：

1. https://blog.csdn.net/qq_40112630/article/details/80392172
2. https://blog.csdn.net/weixin_44404384/article/details/114675894

## 2.负载均衡策略

Nginx提供的负载均衡策略有2种：

- 内置策略和扩展策略。
  1. 内置策略为轮询，加权轮询，Ip hash。
  2. 扩展策略。。。。（全凭想象力）

1. 轮询：

   ![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy4d33dfac-1949-4b2d-abb8-fe0b6e65b8dc.png)

​		每一个服务器按照顺序来一个一个分发，都不落下！

2. 加权轮询

   ![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudyb1e3e440-4159-4259-a174-528b56cb04b2.png)

​		比如第三个服务器处理性能比较好，那么权重就更高，那么如果有很多请求打进来的话那么大量的请求进入到第三个服务器，而只有一部分请求才会依次按照权重进行分发到不同权重的服务器！这样可以保证我们服务器的性能最大化！

3. iphash

   ![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy64acb9a3-cd1a-4c0e-a1fa-9b220046a95a.png)

​	这个呢主要是对客户端请求的ip进行hash操作，然后根据hash结果将同一个客户端ip的请求分发给同一台服务器进行处理，可以解决session不共享的问题。不过现在一般不用session，一般用Redis作为数据共享的方式

4. 动静分离

   ![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudyedb1bbd6-e530-4aba-8fde-68658a10e73f.png)

​		在我们的开发过程中，有些请求是不需要后台去处理的，碧如：css、HTML、jpg等等，这些就不需要经过后台处理的文件就称为静态文件。让动态网站里的动态网页根据一定的规则吧不变的资源和经常变的资源区分开来，动静资源做好了拆分之后，我们可以就可以根据静态资源的特点将其做`缓存操作`，来提高资源响应的速度！

```
什么是动静分离：
	比如在前端中的boostrap还有jquery等js文件，每次加载我们都需要从jar中获取都是非常麻烦的！那么此时就会有类似于缓存的操作，那么也就是说一些静态资源的东西就可以直接从Nginx里面返回了，来提高一定的访问速度！
```

## 3.安装

### 3.1 window

下载地址: http://nginx.org/en/download.html 

下载之后安装包很小直接解压到本地硬盘中即可(目录要没有中文的，否则就会启动报错)

下载界面:

![image-20220501090126440](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202205010901588.png)

> 一般我们都是去"conf"文件夹里修改配置文件即可

快速启动：

1. 在cmd里运行然后执行`nginx.exe`就可以看到运行，（直接点击Nginx.exe运行也可以，只不过一瞬就消失）

2. 浏览器执行（出现这个界面就是启动成功）：

   ![image-20220501090912770](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202205010909848.png)

3. 热更新：

   ```nginx
   当我们修改了nginx的配置文件nginx.conf 时，不需要关闭nginx后重新启动nginx，只需要执行命令 nginx -s reload 即可让改动生效
   ```

4. 结束运行

   - 输入nginx命令 `nginx -s stop`(快速停止nginx) 或 `nginx -s quit`(完整有序的停止nginx)

   - 使用taskkill `taskkill /f /t /im nginx.exe`

     ```c#
     taskkill是用来终止进程的，
     /f是强制终止 .
     /t终止指定的进程和任何由此启动的子进程。
     /im示指定的进程名称 .
     ```

注意：这个命令是在`Nginx.exe`所在的目录下打开cmd命令窗口执行，如果直接关闭运行的Nginx黑窗口那么是无法关闭Nginx的！

----

看下为什么要打开地址`localhost`就能打开Nginx的默认界面？

答：

​	首先我们打开目录`conf`中找到配置文件`nginx.conf`然后并打开：

![image-20220501091854768](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202205010918810.png)

此时我们就知道了原来Nginx默认监听的是端口`80`,然后`localhost`默认是带有80的，所以当我们浏览器打开时就会触发打开本地的`/html/index.html`然后打开

### 3.2 Linux

https://www.kuangstudy.com/bbs/1353634800149213186

有需要就再过来看



## 4. 配置文件详解

这里我们打开配置文件`nginx.conf`:

```nginx
//全局配置-这里配置全局生效
#user  nobody;  //指定用户
worker_processes  1;

//指定错误日志
#error_log  logs/error.log; 
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;
//指定进程
#pid        logs/nginx.pid;


//监听的事件
events {
    //最大连接数
    worker_connections  1024;
}

//配置http的配置，这里可以配置多个server
http {
    //http的全局配置文件
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    upstream xx{
        // 负载均衡配置
    }
    
    server {
        //监听端口80，服务名称为localhost
        listen       80;
        server_name  localhost;
        //代理
        #charset koi8-r;
        #access_log  logs/host.access.log  main;
        location / {
            root   html;
            index  index.html index.htm;
        }
        #error_page  404              /404.html;
        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
}
```

这里贴出具体的配置文件详解：

https://www.jianshu.com/p/18eeee0911a0

https://www.jianshu.com/p/ad4c21ab2266

用到的时候来翻一翻即可

## 5.快速实战

1. 首先启动一个服务，然后改变端口启动两个：

   ![image-20220501141706452](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202205011417500.png)

​		![image-20220501141717155](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202205011417195.png)

2. 配置文件：

   ![image-20220501142855994](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202205011428043.png)

   上面是负载均衡，下面那个是反向代理，也就是说当用户访问80的这个端口那么就会被监听，此时就会被代理，然后进行负载均衡选择其中一个服务进行代理请求；

3. 效果（访问http://localhost:80）：

   ![image-20220501143031565](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202205011430620.png)

疑惑：我也在想为什么在代理的时候要加http，不加的话会怎么样？来试试

- 我去除后热更新失败：

  ![image-20220501143254850](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202205011432892.png)

- 然后我尝试在负载均衡那里加上http呢？

  ![image-20220501143320224](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202205011433267.png)

果然：必须要有一个合法的前缀才可以！

注意：

在配置文件中这里：

![image-20220501143845420](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202205011438466.png)

`/manage`和`/beokaMhost`这两个，也就是说访问指定路径下后执行不同的方法里，比如当访问“80”这个端口后访问路径“/manage”然后就会访问指定文件夹中的，然后如果访问路径“/beokaMhost”后那么就会执行代理访问路径`http://47.108.95.24:8005/`这个路径（当然如果这里代理的名字是负载均衡upstream中的那么就会去获取对应名字中的服务地址进行替换）

画图解析：

![image-20220501150414459](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202205011504535.png)

## 6.动静分离

动静分离是一个很常见的技术，也就是为了减少web端资源消耗的一种方式；

开始：

1. web端进行打包（如何打包就百度吧，这里分享个链接：https://blog.csdn.net/weixin_60208344/article/details/122266644）

   这是打包后的样子：

   ![image-20220501150748874](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202205011507930.png)

2. 放到nginx的HTML文件夹中：

   ![image-20220501150900686](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202205011509735.png)

3. 配置文件：

   ```nginx
   server {
           listen       10001;  //监听这个端口
           server_name  47.108.95.24;
       
    		location /manage {  //访问这个路径前缀下
              alias C:/Users/Administrator/Desktop/nginx/nginx-1.18.0/html/dist;  //访问到指定文件夹下
              index index.html index.htm;  //默认打开index.html
              try_files $uri $uri/ /manage/index.html; 
   		}
       
   		location /beokaMhost/ { //访问这个路径下的请求那么就会执行反向代理进行请求到指定的网络路径下
           	proxy_pass  http://47.108.95.24:8085/; //由于后台就只有一个所以这里就直接代理了，如果有多个可以进行负载均衡
   		}
   }
   
   静态资源： //即监听本服务器上的 10001 端口，当请求的资源以 /manage 为前缀时，将请求直接转发至访问本服务器上C:/Users/Administrator/Desktop/nginx/nginx-1.18.0/html/dist 目录下的资源，即直接将该目录下的静态资源返回。 
   动态资源：//当请求的资源以 /beokaMhost 为前缀时，将请求转发至 47.108.95.24:8085 服务器上，由该服务器来处理动态请求，我们将后台项目部署在了该服务器上。
   ```

   配置文件解释：

   ```nginx
   alias：
   	用于指定请求资源的真实路径
   try_files：
   	按照某种规则去查找文件
   ```

   ![image-20220501152355226](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202205011523293.png)

画图解析：

![image-20220501153320188](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202205011533252.png)

