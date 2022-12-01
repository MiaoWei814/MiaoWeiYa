#  1. Docker概述

## 1.1 Docker为什么会出现？

1. 首先一款产品要经过从开发–上线这个过程。其中就会出现两套环境（线上环境-测试环境）！而不同的环境导致不同的应用配置会非常麻烦
2. 没有Docker之前经常会听到一个声音：“为什么在我的电脑上就可以运行”！并且因为版本更新经常会导致服务不可使用！
3. 环境配置是十分的麻烦，每一个机器都要部署环境(集群Redis、ES、Hadoop…) !费事费力。且不能跨平台，开发环境用Windows最后还要发布到Linux！
4. 之前发布项目是将jar包放在服务器然后单独在服务器部署环境，如果换其他机器也要同样部署，很麻烦，而现在发布一个项目( jar + (Redis+MySQL+JDK+ES这些环境) ),带上环境一起安装打包！



**传统：**开发jar，运维来做！

**现在：**开发打包部署上线，一套流程做完！



**安卓流程：**写好代码后加上本地环境变量如本地数据库，那么首先发布（应用商店）一 张三使用apk一安装即可用！用户不用单独去安装环境这些 直接打包就能使用

**docker流程：**java-jar（环境）— 打包项目帯上环境（镜像）— ( Docker仓库：商店）—下载我们发布的镜像 —直接运行即可！-将我们的环境作为镜像，需要时直接下载完整环境使用！



Docker给以上的问题，提出了解决方案！

![image-20200610142308099](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211251037661.png)

Docker的思想来源于集装箱！

隔离：Docker核心思想！打包装箱！每个箱子都是相互隔离的。

**好处**：

1. 多个应用可以端口可以相同，避免了端口冲突，并且不用考虑环境的问题
2. docker容器包含了应用程序的代码、运行环境、依赖库、配置文件等必需的资源。容器之间达到<mark>进程级别的隔离</mark>，在容器中的操作，不会影响道宿主机和其他容器，这样就不会出现应用之间相互影响的情形！---`隔离强`

3. 实现开发、测试和生产环境的统一化和标准化，镜像作为标准的交付件无论在哪个机器上环境都能一致！---`可移植性`

4. 可以将服务器压榨到极致. 我们平时跑项目的时候, 要启动mysql, redis, tomcat等全部跑起来很浪费内存空间. 而docker由于相互之间是隔离的, 那么可以利用起来很小的空间, 将服务器压榨到极致.---`压榨内存到极致`

Docker通过隔离机制可以将服务器利用到极致！



本质：所有的技术都是因为出现了一些问题，我们需要去解决，才去学习！

## 1.2 Docker的历史

2010年，几个的年轻人，就在美国成立了一家公司 **dotcloud**

做一些pass的云计算服务！LXC（Linux Container容器）有关的容器技术！

Linux Container容器是一种内核虚拟化技术，可以提供轻量级的虚拟化，以便隔离进程和资源。

他们将自己的技术（容器化技术）命名就是 Docker

Docker刚刚延生的时候，没有引起行业的注意！dotCloud，就活不下去！

**开源**

2013年，Docker开源！

越来越多的人发现docker的优点！火了。Docker每个月都会更新一个版本！

2014年4月9日，Docker1.0发布！

docker为什么这么火？**十分的轻巧！**

在容器技术出来之前，我们都是使用虚拟机技术！



虚拟机：在window中装一个VMware，通过这个软件我们可以虚拟出来一台或者多台电脑！笨重！

<mark>虚拟机也属于虚拟化技术，Docker容器技术，也是一种虚拟化技术！</mark>

```shell
VMware : linux centos 原生镜像（一个电脑！） 隔离、需要开启多个虚拟机！ 几个G 几分钟
docker: 隔离，镜像（最核心的环境 4m大小 + jdk + mysql）十分的小巧，运行镜像就可以了！小巧！ 几个M 秒级启动！
```

> 主要目标： 
>
> ​	"Build, ship and Run Any App, Anywhere", 即: 构建,分发,部署,运行等, 一次操作, 在各个app,各个地方都可以运行. 做到:"**一次封装, 到处运行**"

> 所谓隔离：可以理解为集装箱的打包装箱. 每一个箱子都是互相隔离的. 使用docker, 我们就不用担心冲突问题了, 也不用担心环境问题了. 我们可以直接通过docker来进行部署.

> 聊聊Docker

Docker基于Go语言开发的！开源项目！

docker官网：https://www.docker.com/

![image-20221125103823947](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211251038041.png)

文档：https://docs.docker.com/ Docker的文档是超级详细的！

仓库：https://hub.docker.com/

## 1.3 Docker能干嘛

> 之前的虚拟机技术

![image-20200610144126122](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211251125474.png)

Kernel:内核

Lib:依赖所需要的库

App:应用程序



解读：模拟一台电脑，内存非常非常大，占用资源也很多！

> 虚拟机技术缺点

1、 资源占用十分多

2、 冗余步骤多，比如开机等等

3、 启动很慢！

> 容器技术

`容器化技术不是模拟一个完整的操作系统`

比如我们要模拟Contos7那么我们就只需要模拟它的内核而已

![image-20200610144338073](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211251125714.png)

比较Docker和虚拟机技术的不同：

- 传统虚拟机，虚拟出一套容器内的应用直接运行在宿主机硬件，<mark>运行一个完整的操作系统</mark>，然后在这个系统上安装和运行软件 
- 容器内的应用直接运行在宿主机内，<mark>容器是没有自己的内核的</mark>，也没有虚拟我们的硬件，所以就轻便了
- 每个容器间是相互隔离的，每个容器内都有一个属于自己的文件系统，互不影响



传统虚拟机就是复制一份完整的操作系统电脑，然后你在上面开始安装新的软件

容器化技术只模拟宿主内核，在这上面进行简化！

```
1. 是容器化技术！
2. 可以节省我们的资源。比如在原来电脑上可以装3个Redis就要卡死，现在就可以装30个还不卡，可以集群扩展！可以整个扩展服务器的资源
```



> DevOps (开发、运维)

**应用更快速的交付和部署**

传统：一堆帮助文档，安装程序

Docker：打包镜像发布测试，一键运行

**更便捷的升级和扩缩容**

使用了Docker之后，我们部署应用就和搭积木一样！

项目打包为一个镜像，扩展服务器A! 服务器B。(比如升级redis版本，那么其他环境下只需要重新打包镜像即可运行)

**更简单的系统运维**

在容器化之后，我们的开发，测试环境都是高度一致的。

**更高效的计算资源利用**

Docker是内核级别的虚拟化，可以在一个物理机上运行很多个容器实例！服务器的性能可以被压榨到极致。（比如1核2G使用容器化技术可以运行几十个Tomcat，做集群完全没有问题）

---



# 2. Docker安装

## 2.1 Docker的基本组成

![image-20200610145818895](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211251126602.png)

左边：客户端-类似于redis的client

中间：服务器-类似于redis的server

右边：仓库

```
1. 客户端可以用命令来操作容器：run:运行容器、pull：拉取容器、build：构建一个容器
2. 构建完就在中间服务器运行。这里守护进程要去运行就得运行一个镜像，而这个images可以理解成Java的class类模板，而class是唯一的，而container就可以理解为Java的对象(可以有多个)。举个例子：Tomcat就是一个镜像，然后就可以有多个容器，就可以形成集群，本质还是通过镜像来运行
3. 这些镜像或容器最终都来自于远程仓库。
```

**镜像（image）：**

docker镜像就好比是一个模板，可以通过这个模板来创建容器服务，tomcat镜像==>run==>tomcat01容器（提供服务器），通过这个镜像可以创建多个容器（最终服务运行或者项目运行就是在容器中的）。

**容器（container）:**

Docker利用容器技术，独立运行一个或者一组应用，通过镜像来创建的。

启动，停止，删除，基本命令

目前就可以把这个容器理解为就是一个简易的 Linux系统。

**仓库（repository）:**

仓库就是存放镜像的地方！

仓库分为公有仓库和私有仓库。(很类似git)

Docker Hub是国外的。

阿里云…都有容器服务器 (配置镜像加速!)



理解：有三种角色分别是客户端、服务端、仓库端。仓库是存储镜像的地方，而服务端是通过仓库的镜像来创建出多个容器对象，而容器对象就好比是一个简单的Linux系统，我们就在容器对象里运行项目。

## 2.2 安装Docker

> 环境准备

1.Linux要求内核3.0以上

2.CentOS 7



> 环境查看

```shell
#系统内核要求3.0以上
[root@localhost ~]# uname -r
3.10.0-1062.el7.x86_64

#系统版本
[root@localhost ~]# cat /etc/os-release 
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"

```



> 安装

帮助文档：

https://docs.docker.com/engine/install/centos/

1. 卸载旧版本-直接复制即可

   ```shell
   yum remove docker \
                     docker-client \
                     docker-client-latest \
                     docker-common \
                     docker-latest \
                     docker-latest-logrotate \
                     docker-logrotate \
                     docker-engine
   ```

   ![image-20221125153716570](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211251537660.png)

```shell



#2.需要的安装包（安装 yum-utils 包(它提供了 yum-config-manager 实用程序)并设置存储库。）
yum install -y yum-utils

#3.设置镜像的仓库
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
#上述方法默认是从国外的，不推荐

#推荐使用国内的阿里云镜像-比较快
yum-config-manager \
    --add-repo \
    https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
  
#4. 更新软件包索引 
yum makecache fast

#5. 安装docker docker-ce 社区版 而ee是企业版
yum install docker-ce docker-ce-cli containerd.io # 这里我们使用社区版即可

#6. 启动docker
systemctl start docker

#7. 使用docker version 查看是否安装成功
docker version
```

![image-20221125154725520](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211251547641.png)


```shell
docker run hello-world
```

![image-20221125155403679](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211251554863.png)

```shell
#8.查看一下下载的hello-world镜像
[root@localhost /]# docker images
```

![image-20221125172910352](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211251729412.png)

了解：卸载docker

```shell
#1.卸载依赖
yum remove docker-ce docker-ce-cli containerd.io

#2. 删除资源
rm -rf /var/lib/docker
# /var/lib/docker 	是docker的默认工作路径！
```

## 2.3 阿里云镜像加速

**1、登录阿里云找到容器服务——>镜像加速器**

![image-20200610155156310](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211261342117.png)

**2、配置使用**

```shell
sudo mkdir -p /etc/docker

sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://184yrtp4.mirror.aliyuncs.com"]
}
EOF

sudo systemctl daemon-reload

sudo systemctl restart docker
```



## 2.4 底层原理

> docker run 流程图
>

![image-20200610160609037](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211281627363.png)

> 底层原理和运行机制：

​	Docker的服务架构类型是Client-Server模式，客户端发送请求到服务端，服务器作为守护进程在后台运行，通过socket进行链接，响应客户端的请求调用运行对应的容器，容器之间相互隔离，隔离机制是Namespace，安全性相对VM较低！

![image-20200610161147612](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docker基础入门\Docker概述(1).assets%5Cimage-20200610161147612.png)

> 容器的实质是进程，与宿主机上的其他进程是共用一个内核，但与直接在宿主机执行的进程不同，容器进程运行在属于自己的独立的命名空间。命名空间隔离了进程间的资源，使得a,b进程可以看到S资源，而c进程看不到。

## 2.5 与VM比较

首先来看两组图：

<img src="https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211281659449.png" alt="img" style="zoom:25%;" />

<img src="https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211281700382.png" alt="img" style="zoom:25%;" />

1、Docker有着比虚拟机更少的抽象层

```
由于docker不需要虚拟机 (虚拟化程序) 实现硬件资源虚拟化，运行在docker容器上的程序直接使用的都是实际物理机的硬件资源。因此在CPU、内存利用率上docker将会在效率上有明显优势。
```

2、Docker利用的是宿主机的内核，vm需要Guest Os(宿主机操作系统os内核)。

```
当新建一个容器时，docker不需要和虚拟机一样重新加载一个操作系统内核。进而避免引寻、加载操作系统内核返回等比较费时费资源的过程，当新建一个虚拟机时，虚拟机软件需要加载OS，返回新建过程是分钟级别的。而docker由于直接利用宿主机的操作系统，则省略了返回过程，因此新建一个docker容器只需几秒钟
```

![img](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211281709467.png)

左边是我们的**虚拟机**，两边都是硬件，假设都是同一台笔记本，两边都有主机的操作系统，左边（Hypervisor）可以看作是VMware，上面的每个Guest OS可以看作是centos7、Ubuntu、Linux Redhat等，<mark>每个系统都要重量级的加载网络配置、硬件资源虚拟化等等，然后再上面加载各种第三方的libs库，再上面才跑各种应用。相当于整个系统都在安装，整个系统都搬过来了。</mark>



而**docker**直接利用宿主机的操作系统，这样Docker Engine就很轻量地做了一层容器虚拟化，每个容器跑在docker上，**相当于我就在window上装了个软件，这个软件上面又运行了另外一些软件**，容器里运行了redis、mongodb、nginx。

![img](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211281712810.png)



理解：比如左边要跑两个Centos7就要虚拟两个虚拟机就会非常非常慢，而docker没有这一层的概念就直接在主机上安装了docker这个服务，而所有的东西运行在docker服务里。传统的VM是一个完整的虚拟机，而现在变成了一个容器！





---



# 3. Docker的相关命令

## 3.1 帮助命令

```shell
docker version     # 显示docker的版本信息
docker info        # 显示docker的系统信息，包括镜像和容器的数量
docker 命令 --help  # 帮助命令
```

官方帮助文档的地址：https://docs.docker.com/engine/reference/commandline/build/

中文命令大全：https://www.cssjs.cn/doc/3/21.html

## 3.2 镜像命令

> **docker images** 查看所有本地的主机上的镜像

选项：

- docker images -a 列出本地所有境像
- docker images -q 只显示镜像id

```
列表头说明：
REPOSITORY 镜像仓库源、TAG 镜像标签版本号、IMAGE ID 镜像ID、CREATED 镜像创建时间、SIZE 镜像大小
```

> **docker search**在Docker Hub上搜索镜像

选项：

- --filter , -f：基于给定条件过滤输出。
- --format：使用模板格式化显示输出
- --limit：Max number of search results ，默认值25

<mark>例如：--filter=STARS=3000  #搜索出来的镜像就是STARS大于3000的</mark>

![image-20221129142807328](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211291428401.png)

> **docker pull**从镜像仓库中拉取或者更新指定镜像

![image-20221129144321612](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211291443730.png)

```shell
# 指定版本下载
[root@localhost /]# docker pull mysql:5.7
5.7: Pulling from library/mysql
8559a31e96f4: Already exists   # 联合文件系统的好处：上面下载过的MySQL与5.7版本的MySQL有相同的文件时不需要重复下载，极大的节省内存
d51ce1c2e575: Already exists 
c2344adc4858: Already exists 
fcf3ceff18fc: Already exists 
16da0c38dc5b: Already exists 
b905d1797e97: Already exists 
4b50d1c6b05c: Already exists 
d85174a87144: Pull complete 	# 只需要更新新的东西
a4ad33703fa8: Pull complete 
f7a5433ce20d: Pull complete 
3dcd2a278b4a: Pull complete 
Digest: sha256:32f9d9a069f7a735e28fd44ea944d53c61f990ba71460c5c183e610854ca4854
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
```

> **docker rmi**删除镜像

```shell
[root@localhost /]# docker rmi -f 镜像id  				 #删除指定镜像
[root@localhost /]# docker rmi -f 镜像id 镜像id 镜像id  	  	   #删除多个镜像
[root@localhost /]# docker rmi -f $(docker images -aq)              #删除全部镜像
```

> **doker tag**创建镜像标签。
>
> 用处：因为每一个版本不同，可能随着业务量的产生，需求的迭代更新，不同版本所需要的环境不同。当某个场景需要用到哪个版本时，我们可以随时切换，只需要切换版本即可

```shell
格式：docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```

![image-20221129145041405](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211291450477.png)

> **docker history**查看指定镜像的创建历史

## 3.3 容器命令

> **docker run**创建一个新的容器并运行一个命令

- -d: 后台运行容器，并返回容器ID；
- -i: 以交互模式运行容器，通常与 -t 同时使用；
- -P: 随机端口映射，容器内部端口随机映射到主机的端口
- -p: 指定端口映射，格式为：主机(宿主)端口:容器端口
- -t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用；
- --name="nginx-lb": 为容器指定一个名称；
- -m :设置容器使用内存最大值；
- --volume , -v: 绑定一个卷
- -e username="ritchie": 设置环境变量；
- -m :设置容器使用内存最大值；



例如：使用镜像nginx:latest以交互模式启动一个容器,在容器内执行/bin/bash命令。

```sh
docker run -it nginx:latest /bin/bash
```

注意：常见的坑，docker 容器使用后台运行，就必须要有一个前台进程，docker发现没有应用（对外提供的应用）会自动停止nginx,容器启动后，发现自己没有提供服务，就会立刻停止，就是没有程序了，所以应该是docker run -it 镜像名，然后不停止退出：Ctrl+p+q，当然还有一种办法能正常后台启动且不进去，那就是<mark>docker run -dit nginx</mark>

踩个坑：

```shell
 # -it 表示创建一个伪终端，这样就可以进入到容器里面
 # 后面的/bin/bash的作用是表示载入容器后运行bash ,docker中必须要保持一个进程的运行，要不然整个容器启动后就会马上kill itself，这个/bin/bash就表示启动容器后启动bash。
 docker run -it centos
 docker run -it centos /bin/bash
 
 对比之后发现其实第一种方式默认启动命令就是下面这个，所以两者是相等的！
 # cmd /bin/bash 作用是启动一个挂起的进程，以免容器没有进程挂起也close掉
```

![image-20221201134317744](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212011343913.png)



> **docker attach 容器id**连接到正在运行中的容器。
>
> 进入容器正在执行的终端，不会启动新的进程,类似于共享屏幕的概念

> **docker exec**在运行的容器中执行命令
>
> 进入容器后开启一个新的终端，可以在里面操作（常用）

选项：

- -d :分离模式: 在后台运行
- -i :即使没有附加也保持STDIN 打开
- -t :分配一个伪终端

![image-20221129173740780](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211291737868.png)

> **docker ps**列出正在运行的容器信息

选项：

- -a :显示所有的容器，包括未运行的。
- -f :根据条件过滤显示的内容。
- --format :指定返回值的模板文件。
- -l :显示最近创建的容器。
- -n :列出最近创建的n个容器。
- --no-trunc :不截断输出。
- -q :静默模式，只显示容器编号。
- -s :显示总的文件大小。

![image-20221129151944219](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211291519297.png)

参数解释：

- CONTAINER ID: 容器 ID。
- IMAGE: 使用的镜像。
- COMMAND: 启动容器时运行的命令。
- CREATED: 容器的创建时间。
- STATUS: 容器状态。
- PORTS: 容器的端口信息和使用的连接类型（tcp\udp）。
- NAMES: 自动分配的容器名称。

> **退出容器**

```shell
exit   # 直接退出容器，停止容器（如果容器是以后台方式启动，这里exit后也会ps查到，所以需要在外面使用stop来停止容器运行）
Ctrl + p + q  # 容器不停止退出
```

> **rm**移除容器

```shell
docker rm 容器id				   # 删除指定容器，不能删除正在运行的容器，如果要强制删除 rm -f
docker rm -f $(docker ps -aq)    # 删除所有容器 
docker rm -v redis	# 移除容器和它的卷
docker ps -a -q|xargs docker rm  # 删除所有容器
```

> **启动和停止容器的操作**

```shell
docker start 容器id     # 启动容器
docker restart 容器id   # 重启容器
docker stop 容器id      # 停止当前正在运行的容器
docker kill 容器id      # 强制停止当前正在运行的容器
docker parse 容器id #暂停容器
docker unpause 容器id # 暂停容器进程
```

> **docker logs**获取容器的日志

选项：

- -f : 跟踪日志输出
- --since :显示某个开始时间的所有日志
- -t : 显示时间戳
- --tail :仅列出最新N条容器日志

查看容器mynginx从2016年7月1日后的最新10条日志。

```shell
docker logs --since="2016-07-01" --tail=10 mynginx
```

> **docker top**显示容器运行进程

![image-20221129174430188](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211291744264.png)

> **docker port**列出指定的容器的端口映射

![image-20221129175133361](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211291751430.png)



> **docker inspect**获取容器/镜像的元数据

选项：

- --format , -f :指定返回值的模板文件。
- --size , -s :显示总的文件大小。
- --type :为指定类型返回JSON。

例如：获取容器的镜像名称

![image-20221129175719007](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211291757153.png)

> **docker wait**阻塞运行直到容器完全停止，然后打印出它的退出代码

![image-20221130091403825](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211300914949.png)





## 3.4 常用其他命令

**从容器内拷贝文件到主机上**

```shell
docker cp 容器id:容器内目标文件路径  目的主机路径

# 查看当前主机目录
[root@localhost home]# ls
ztx

# 进入docker容器内部
[root@localhost home]# docker attach ce989f90023d
[root@ce989f90023d /]# cd /home/
[root@ce989f90023d home]# ls

# 在容器内新建一个文件
[root@ce989f90023d home]# touch test.java
[root@ce989f90023d home]# exit
exit
[root@localhost home]# docker ps -a
CONTAINER ID     IMAGE      COMMAND       CREATED           STATUS                PORTS           NAMES
ce989f90023d     centos  "/bin/bash"  44 minutes ago  Exited (0) 46 seconds ago               nifty_johnson

# 将docker内文件拷贝到主机上
[root@localhost home]# docker cp ce989f90023d:/home/test.java /home
[root@localhost home]# ls
test.java  ztx
[root@localhost home]# 

# 拷贝是一个手动过程，未来我们使用 -v 卷的技术，可以实现自动同步 
```

> **docker stats**查看Docker的CPU的状态

选项：

- **--all , -a :**显示所有的容器，包括未运行的。
- **--format :**指定返回值的模板文件。
- **--no-stream :**展示当前状态就直接退出了，不再实时更新。
- **--no-trunc :**不截断输出。

![image-20221130132550831](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211301325923.png)

列表头：

- **CONTAINER ID** 与 **NAME**: 容器 ID 与名称。

- **CPU %**与**MEM %**：容器使用的 CPU 和内存的百分比。

- **MEM USAGE / LIMIT:** 容器正在使用的总内存，以及允许使用的内存总量。

- **NET I/O:** 容器通过其网络接口发送和接收的数据量。

- **BLOCK I/O:** 容器从主机上的块设备读取和写入的数据量。

- **PIDs:** 容器创建的进程或线程数。

  



## 3.5 小结（all 图示命令）

![image-20200611085918923](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211241557262.png)

```shell
  attach    	      # 当前shell下attach连接指定运行的镜像
  build            # 通过Dockerfile定制镜像
  commit     #提交当前容器为新的镜像
  cp          #从容器中拷贝指定文件或目录到宿主机中
  create    			  # 创建一个新的容器，同run,但不启动容器
  diff        #查看docker容器的变化
  events        # 从docker服务获取容器实时事件
  exec           # 在已存在的容器上运行命令
  export      # 导出容器的内容流作为一个tar归档文件[对应import]
  history                # 展示一个镜像形成历史
  images                              # 列出系统当前的镜像
  import      # 从tar包中的内容创建一个新的文件系统镜像[对应export]
  info             # 显示系统相关信息
  inspect      # 查看容器详细信息
  kill            # 杀死指定的docker容器
  load         # 从一个tar包加载一个镜像[对应save]
  login       			  # 注册或者登录一个docker源服务器
  logout     		  # 从当前Docker registry退出
  logs        		  # 输出当前容器日志信息
  pause       	     # 暂停容器
  port       # 查看映射端口对应容器内部源端口
  ps       					  # 列出容器列表
  pull       # 从docker镜像源服务器拉取指定镜像或库镜像
  push           # 推送指定镜像或者库镜像至docker源服务器
  rename      				  # 给docker容器重新命名
  restart    		  # 重启运行的容器
  rm          			  # 移除一个或者多个容器
  rmi         		  # 移除一个或者多个镜像[无容器使用该镜像时才可删除，否则需删除相关容器才可继续或 -f 强制删除]
  run         	  # 创建一个新的容器并运行一个命令
  save        # 保存一个镜像为一个tar包[对应load]
  search      	  # 在docker hub中搜索镜像
  start      	  # 启动容器
  stats        # 实时显示容器资源使用统计
  stop      	  # 停止容器
  tag          # 给源中镜像打标签
  top         	  # 查看容器中运行的进程信息
  unpause      # 取消暂停容器
  update     	  # 更新一个或多个容器配置
  version   	  # 查看docker版本号 
  wait         # 截取容器停止时的退出状态值
```

# 4. 作业练习

## 4.1 Docker 安装Nginx

> 1、先去仓库搜索这个镜像
>
> 2、拉取下载这个镜像
>
> 3、run运行这个镜像容器
>
> 4、本地测试访问

```shell
1. docker search nginx
2. docker pull nginx
3. docker run -dit --name nginx01 -p 3344:80 nginx # 以后台运行且指定宿主机端口和容器内部端口
3. curl localhost:3344 #curl执行发送各种http请求
```

![image-20221130093634768](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211300936003.png)

注：如果想在公网访问的话那么则需要在阿里云控制台中的安全组开放端口，就可以访问这个端口。

如果需要修改对应的Nginx的配置文件，则需要以下这个步骤：

![image-20221130094206844](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211300942986.png)



**端口暴露的概念**

![image-20200611085948617](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211241558128.png)

**思考问题：**我们每次改动nginx配置文件，都需要进入容器内部？十分麻烦，我要是可以在容器外部提供一个映射路径，达到在容器外部修改文件名，容器内部就可以自动修改？-v 数据卷 技术！



## 4.2 Docker来装一个tomcat

```shell
# 官方文档
docker run -it --rm tomcat:9.0

# 我们之前的启动都是后台，停止了容器之后，容器还是可以查到 docker run -it --rm,一般用来测试，用完就删除

# 下载再启动
docker pull tomcat

# 启动运行
docker run -d -p 3355:8080 --name tomcat01 tomcat

#测试访问没有问题，但是一访问就是404,：官方的tomcat是阉割版的！


# 进入容器
[root@localhost /]# docker exec -it tomcat01 /bin/bash

# 发现问题：1、linux命令少了 2、webapps内没有内容（这是阿里云镜像的原因：默认是最小镜像，所有不必要的都删除）
# 保证最小可运行环境
#解决方法：将webapps.dist目录下内容拷至webapps下
root@c435d5b974a7:/usr/local/tomcat# cd webapps
root@c435d5b974a7:/usr/local/tomcat/webapps# ls
root@c435d5b974a7:/usr/local/tomcat/webapps# cd ..
root@c435d5b974a7:/usr/local/tomcat# ls
BUILDING.txt  CONTRIBUTING.md  LICENSE	NOTICE	README.md  RELEASE-NOTES  RUNNING.txt  bin  conf  lib  logs  native-jni-lib  temp  webapps  webapps.dist  work
root@c435d5b974a7:/usr/local/tomcat# cd webapps.dist/
root@c435d5b974a7:/usr/local/tomcat/webapps.dist# ls
ROOT  docs  examples  host-manager  manager
root@c435d5b974a7:/usr/local/tomcat/webapps.dist# cd ..
root@c435d5b974a7:/usr/local/tomcat# cp -r webapps.dist/* webapps    这个目录下的所有文件复制到指定目录下
root@c435d5b974a7:/usr/local/tomcat# cd webapps
root@c435d5b974a7:/usr/local/tomcat/webapps# ls
ROOT  docs  examples  host-manager  manager
```

拷贝完成就可以访问了：

![image-20200611090019494](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211241606607.png)

**思考问题：**我们以后要部署项目，如果每次都要进入容器是不是十分麻烦？我要是可以在容器外部提供映射路径，webapps,我们在外部放置项目，就自动同步到内部就好了！

并且如果一个容器内放置有MySQL等那么一旦容器被误删，这安全性得不到一个保障！



## 4.3 部署es+kibana

```shell
# es 暴露的端口很多！
# es 十分耗内存
# es 的数据一般需要放置到安全目录！挂载
# --net somenetwork？网络配置
# -e "discovery.type=single-node"  集群类型选择为单个节点

# 启动 elasticsearch
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.6.2

# 启动了 Linux就可卡住了   docker stats 查看cpu的状态
# es 是十分耗内存的

# 测试一下es是否成功了
[root@localhost /]# curl localhost:9200
{
  "name" : "83b0d5dca26e",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "MjhNfYTvRVui1UCrAwMdqw",
  "version" : {
    "number" : "7.6.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
    "build_date" : "2020-03-26T06:34:37.794943Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}

# 查看docker容器占用资源情况
```

![image-20200611124706727](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211301359625.png)

```shell
# 赶紧关闭容器，增加内存限制，修改配置文件 -e 环境配置修改，最多占64m的内存，最大占512m内存
docker run -d --name elasticsearch02 -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2
 
 # 查看docker容器占用资源情况
```

![image-20200611124755826](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211301359004.png)

```shell
[root@localhost /]# curl localhost:9200
{
  "name" : "5a262b522bbf",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "rGMaCpVXScGaZcv_UtK3gQ",
  "version" : {
    "number" : "7.6.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
    "build_date" : "2020-03-26T06:34:37.794943Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```



> 作业4：使用 kibana 连接 es ? 思考网络如何才能连接过去！

![image-20200611125352717](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211301358349.png)

众所周知容器互相隔离，所以这里直接访问是不行的，所以我们需要借助当前共用的Linux环境，使用内网的ip进行转发

# 5. 可视化

- portainer（先用这个）

- Rancher （CI/CD再用）-持续集成-持续部署

  

## 5.1 什么是portainer ?

Docker图形化界面管理工具！提供一个后台面板供我们操作！

```shell
# 这是本地卷和权限挂载的一些命令
docker run -d -p 8088:9000 \
--restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
```

外部访问测试：http://ip:8088/

通过它来访问了;

![image-20200611141621853](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211301358742.png)

选择本地的：

![image-20200611142004773](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211301358025.png)

进入之后的面板：

![image-20200611144838665](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211301358701.png)

![image-20200611144900114](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211301359137.png)

可视化面板我们平时不会使用，大家自己测试玩玩即可！



# 6. Docker镜像讲解

## 6.1 镜像的概念和UFS联合文件系统

> 镜像概念：

镜像是一种轻量级、可执行的独立<mark>软件包</mark>，用来打包软件运行环境和基于运行环境开发的软件，他包含运行某个软件所需的所有内容，<mark>包括代码、运行时库、环境变量和配置文件。</mark>

所有应用，直接打包docker镜像，就可以直接跑起来！

**如何得到镜像**

- 从远程仓库下载
- 别人拷贝给你
- 自己制作一个镜像 DockerFile

> UnionFs （联合文件系统）

UnionFs（联合文件系统）：Union文件系统（UnionFs）是一种分层、轻量级并且高性能的文件系统，他支持<mark>对文件系统的修改作为一次提交来一层层的叠加</mark>，同时可以将不同目录挂载到同一个虚拟文件系统下。Union文件系统是 Docker镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。



特性：

​	一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录。`它将多个物理位置不同的文件目录联合起来，挂载到某一个目录下，形成一个抽象的文件系统。`

1. 类似于git一样记录着每一次的提交，而这里联合文件系统就会记录它每一次修改（看是新增还是叠加）。
   - 比如说tomcat底层会用到Linux的内核，那么在构建完后又开始构建MySQL，那么在联合文件系统就会去看是否同样会用到linux的，有的话就直接拿来用了-共用！极大的节省了内存和空间！

```
docker的镜像实际上由一层一层的文件系统组成，它在远程仓库中将多个不同物理位置的文件目录都挂载到比如MySQL的目录下形成文件系统（镜像），这样在pull拉取下载的时候，一行一行记录下载，既然是挂载那么也会去找对应的具体的文件目录下进行下载，不过在下载之前文件系统就会查看本地是否有本次构建镜像需要的文件，如果有 就省略这次拉取的文件直接从本地共用--节省内存空间
```



## 6.2 Docker镜像加载原理

> Docker镜像加载原理

**docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。**

`boots(boot file system）`:（系统启动需要引导加载）-无论是什么镜像这部分都是公用的！

​	主要包含 bootloader(加载器)和 Kernel(内核), bootloader主要是<mark>引导加载</mark> kernel后系统就运行起来了, Linux刚启动时会加载bootfs文件系统，在 Docker镜像的最底层是 boots。这一层与我们典型的Linux/Unix系统是一样的，包括bootloader和 Kernel。<mark>当boot加载完成之后整个内核就都在内存中了</mark>，此时内存的使用权已由 bootfs转交给内核，此时系统也会卸载bootfs。<mark>类似于电脑黑屏-> "加载" ->开机进入系统。进入后就会将“加载”给卸载掉，已经没用了</mark>

`rootfs（root file system)`:

​	系统开机后就会变成基本的Linux的文件系统，包含的就是典型 Linux系统中的/dev,/proc,/bin,/etc等标准目录和文件。 <mark>rootfs(容器)就是各种不同的操作系统发行版</mark>，比如 Ubuntu, Centos等等。
![image-20200611162007055](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211301459037.png)

解读：内核=> 内核+images =>内核+images+images

```
boots表示内核的意思。可以发现镜像越多层级就越高，但底下的层是公用的！
```

平时我们安装进虚拟机的CentOS都是好几个G，为什么Docker这里才200M？![image-20200611162057734](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211301628603.png)

​	<mark>对于一个精简的OS(内核), rootfs可以很小，只需要包合最基本的命令、工具和程序库就可以了，因为底层直接用Host(主机)的kernel，自己只需要提供rootfs就可以了。</mark>由此可见对于不同的Linux发行版， boots基本是一致的， rootfs会有差別，因此不同的发行版可以公用bootfs.

虚拟机是分钟级别，容器是秒级！





## 6.3 分层理解

> 分层的镜像

我们可以去下载一个镜像，注意观察下载的日志输出，可以看到是一层层的在下载！

![image-20221130170009790](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211301700980.png)



**思考：为什么Docker镜像要采用这种分层的结构呢？**

最大的好处，我觉得莫过于资源共享了！比如有多个镜像都从相同的Base镜像构建而来，那么宿主机只需在磁盘上保留一份base镜像，同时内存中也只需要加载一份base镜像，这样就可以为所有的容器服务了，而且镜像的每一层都可以被共享。

查看镜像分层的方式可以通过docker image inspect 命令

![image-20221130170241231](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211301702383.png)

可以发现这里每一行就对应刚刚下载的每一个步骤，比如第一个步骤安装centos在这之前就已经做过了，所以这里就直接是“Already exists"已经存在了，而剩下安装Redis的步骤正在一步一步的安装的。每安装一次就是一次记录！





**理解：**

<mark>所有的 Docker镜像都起始于一个基础镜像层，当进行修改或添加新的内容时，就会在当前镜像层之上，创建新的镜像层。</mark>

举一个简单的例子，假如基于 Ubuntu Linux16.04创建一个新的镜像，这就是新镜像的第一层；如果在该镜像中添加 Python包，就会在基础镜像层之上创建第二个镜像层；如果继续添加一个安全补丁，就会创建第三个镜像层。

该镜像当前已经包含3个镜像层，如下图所示（这只是一个用于演示的很简单的例子）。

![image-20200611163818495](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211301733839.png)

在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的组合，理解这一点非常重要。下图中举了一个简单的例子，每个镜像层包含3个文件，而整体的大镜像包含了来自两个镜像层的6个文件。

![image-20200611164322267](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211301733422.png)

上图中的镜像层跟之前图中的略有区別，主要目的是便于展示文件。

下图中展示了一个稍微复杂的三层镜像，在外部看来整个镜像只有6个文件，这是因为最上层中的文件7是文件5的一个更新版。

![image-20200611164447964](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211301733133.png)

这种情況下，上层镜像层中的文件覆盖了底层镜像层中的文件。这样就使得文件的更新版本作为一个新镜像层添加到镜像当中。

Docker通过存储引擎（新版本采用快照机制）的方式来实现镜像层堆栈，并保证多镜像层对外展示为统一的文件系统。

Linux上可用的存储引撃有AUFS、 Overlay2、 Device Mapper、Btrfs以及ZFS。顾名思义，每种存储引擎都基于 Linux中对应的文件系统或者块设备技术，井且每种存储引擎都有其独有的性能特点。

Docker在 Windows上仅支持 windowsfilter 一种存储引擎，该引擎基于NTFS文件系统之上实现了分层和CoW [1]。



下图展示了与系统显示相同的三层镜像。所有镜像层堆叠并合并，对外提供统一的视图。

![image-20221130171348025](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211301713150.png)



> 特点

Docker镜像都是*只读*的，当容器执行run命令启动时，一个新的*可写层*加载到镜像的顶部-容器层！

这一层就是我们通常说的容器层，容器之下的都叫镜像层！![image-20200611165355825](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211301733381.png)



总结：

​	也就是说所谓的容器与镜像，其实都是镜像，当我们pull拉取下来的时候并以run运行起来，这时就会在镜像的顶层加一层可写层(容器层)。而我们现在的操作都在容器层的，所以如果要部署属于自己的项目等等，那么就需要在容器层部署好以后将整个提交为一个镜像再进行拉取提交部署！

​	换个角度来讲镜像其实就是分层文件系统，每有一个文件修改那么都会新加一层，而分层文件管理系统就是将不同位置的文件目录挂载到一起，也就是理解为镜像提供给外部的视图就是只有一层合并镜像！

![image-20221201094513663](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212010945792.png)



如何提交一个自己的镜像？

## 6.4 commit镜像

> **docker commit**:从容器创建一个新的镜像

选项：

- -a :提交的镜像作者；
- -c :使用Dockerfile指令来创建镜像；
- -m :提交时的说明文字；
- -p :在commit时，将容器暂停。

```shell
# 命令和git原理类似
docker commit -m="描述信息" -a="作者" 容器id 目标镜像名:[版本TAG]
```

实战测试

```shell
#1、启动一个默认的tomcat
#2、发现这个默认的tomcat是没有webapps应用的，镜像的原因。官方的镜像默认webapps下面是没有文件的！
#3、我自己将webapp.dist下文件拷贝至webapps下
#4、将我们操作过的容器通过commit提交为一个镜像！我们以后就可以使用我们修改过的镜像了，这就是我们自己的一个修改的镜像
```

![image-20221201110442271](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212011104423.png)

注：如果你想要保存当前容器的状态，就可以通过commit来提交，获得一个镜像，就好比我们我们使用虚拟机的快照。

# 7. 容器数据卷
## 7.1 概念

**docker的理念：**将应用和环境打包成一个镜像！然后发布-启动-运行（变成容器）！



问题：如果数据都放在容器中，那么我们容器删除，数据就会丢失！

期望：MySQL数据可以存储在本地、数据可以持久化！



*容器数据卷*：容器之间可以有一个数据共享的技术！Docker容器中产生的数据，同步到本地(Linux)！这就是卷技术！目录的挂载，将我们容器内的目录，**挂载**到Linux上面！

![image-20221201140100288](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212011401393.png)

**为什么使用数据卷：容器的持久化和同步操作！容器间也是可以数据共享的！**

## 7.2 使用数据卷

> 方式一：直接使用命令来挂载

```shell
docker run -it -v 主机目录:容器目录

# 测试
[root@localhost home]# docker run -it -v /home/ceshi:/home  centos  /bin/bash

# 启动起来的时候，我们可以通过docker inspect 容器id 来查看挂载情况：（见下图）
```

![image-20221201143206055](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212011432171.png)

测试文件的同步

![image-20221201143943394](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212011439566.png)

**双向同步**

在容器内指定目录下添加或修改一个文件，会同步到主机指定目录下！反之，在主机目录下做相关操作，也会同步到容器对应的目录下！

再来测试！

1、停止容器

2、宿主机修改文件

3、启动容器

4、容器内的数据依旧是同步的！



**注意**：如果容器已经被删除掉了那么是无法同步的！

**好处**：我们以后修改只需要在本地修改即可，容器内会自动同步！

## 7.3 实战：安装MySQL

思考：MySQL的数据持久化的问题！

```shell
# 获取镜像
[root@localhost home]# docker pull mysql:5.7

# 运行容器，需要做数据挂载！ # 安装mysql,需要配置密码，这是要注意的点！
# 官方测试：docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag

# 启动我们的MySQL容器
-d	后台运行
-p	端口映射
-v	卷挂载
-e  环境配置
--name  容器名字
[root@localhost home]# docker run -dit -p 3310:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7
```

1. 测试链接成功

   ![image-20221201154039856](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212011540951.png)

2. 本地测试创建一个数据库“testAdd”，查看一下我们的映射的路径是ok!

   ![image-20221201154642703](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212011546835.png)

3. 假设我们将容器删除

   ![image-20221201160637401](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212011606590.png)

发现，我们挂载到本地的数据卷依旧没有丢失，这就实现了容器数据持久化功能！

## 7.4 具名和匿名挂载

```shell
# 匿名挂载
-v 容器内路径
docker run -d -P --name nginx01 -v /etc/nginx nginx


# 查看所有卷的情况
[root@localhost data]# docker volume ls
DRIVER              VOLUME NAME
local               2dd0379216c9ee4441ed56f8ce53461c19abe78b8cfd024ac5fbe07c3b8f09ba

# 这里发现，这种就是匿名挂载，我们在 -v 后只写了容器内的路径，没有写容器外的路径！

# 具名挂载
[root@localhost home]# docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx nginx
5ba5708389bf71b2156fdbcedc50a62b16ac27adb2a3dfac42c52e9da5ace79f
[root@localhost home]# docker volume ls
DRIVER              VOLUME NAME
local               juming-nginx

# 通过 -v 卷名：容器内路径
# 查看一下这个卷  # 先找到卷所在路径 docker volume inspect 卷名，如下图：
```

![image-20200611235522418](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200611235522418.png)

所有的docker容器内的卷，没有指定目录的情况下都是在**/var/lib/docker/volumes/xxxx/_data**下！
我们通过具名挂载可以方便的找到我们的一个卷，大多数情况使用 **具名挂载**

```shell
# 如何确定是具名挂载还是匿名挂载，还是指定路径挂载！
-v	容器内路径		       # 匿名挂载
-v	卷名:容器内路径	 	 # 具名挂载
-v	/宿主机路径:容器内路径   # 指定路径挂载！
```

拓展：

```shell
# 通过 -v 容器内路径：ro 或 rw   改变读写权限
ro	 #readonly 只读
rw	 #readwrite 可读可写

# 一旦创建容器时设置了容器权限，容器对我们挂载出来的内容就有限定了！
docker run -d -P --name nginx05 -v juming:/etc/nginx:ro nginx
docker run -d -P --name nginx05 -v juming:/etc/nginx:rw nginx

# 默认是 rw
# ro 只要看到ro就说明这个路径只能通过宿主机来操作，容器内部是无法操作！
```

## 初始Dockerfile

Dockerfile 就是用来构建 docker镜像的构建文件！命令脚本！ 先体验一下！

通过这个脚本可以生成镜像，镜像是一层一层的，脚本是一个个的命令，每个命令都是最终镜像的一层！

```shell
# 创建一个dockerfile文件，名字可以随机，建议 dockerfile

[root@localhost docker-test-volume]# vim dockerfile
# 文件中的内容：指令(大写) 参数
FROM centos

VOLUME ["volume01","volume02"]

CMD echo"----end----"

CMD /bin/bash

# 这里的每个命令，就是镜像的一层！
```

![image-20200612003052844](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200612003052844.png)

注意：我们这里的 dockerfile  是我们编写的文件名哦！

![image-20200612003717223](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200612003717223.png)

这两个卷和外部一定有两个同步的目录！

![image-20200612003946028](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200612003946028.png)

查看一下卷挂载在主机上的路径

**docker inspect 容器id**

![image-20200612004608027](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200612004608027.png)

测试一下刚才的文件是否同步出去了！

这种方式我们未来使用十分的多，因为我们通常会构建自己的镜像！

假设构建镜像的时候没有挂在卷，要手动镜像挂载即可： (参考上文**具名和匿名挂载**)

```shell
-v 卷名:容器内路径 
```

## 数据卷容器

**多个mysql同步数据！**

![image-20200612223759573](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200612223759573.png)

![image-20200612224621379](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200612224621379.png)

![image-20200612225358172](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200612225358172.png)

在docker03下创建docker03文件后，进入docker01发现也依旧会同步过来：

![image-20200612225641266](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200612225641266.png)

```shell
# 测试1：删除docker01后，docker02和docker03是否还可以访问原来docker01下创建的的文件？
# 测试1的结果为：依旧可以访问！！！

# 测试2：删除docker01后，docker02和docker03之间是否可以相互同步文件？
# 测试2的结果为：docket02和docker03之间一九可以完成同步！！！ 见下图：
```

![image-20200612231431551](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200612231431551.png)

![image-20200612231603498](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200612231603498.png)

**多个mysql实现数据共享**

```shell
➜  ~ docker run -d -p 3306:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7
➜  ~ docker run -d -p 3307:3306 -e MYSQL_ROOT_PASSWORD=123456 --name mysql02 --volumes-from mysql01  mysql:5.7
# 这个时候，可以实现两个容器数据同步！
```

**结论：**

容器之间的配置信息的传递，数据卷容器的生命周期一直持续到没有容器使用为止。

但是一旦你持久化到了本地，这个时候，本地的数据是不会删除的！

---



# DockerFile

## DockerFile介绍

`dockerfile`是用来构建docker镜像的文件！命令参数脚本！

**构建步骤：**

1、 编写一个dockerfile文件

2、 docker build 构建称为一个镜像

3、 docker run运行镜像

4、 docker push发布镜像（DockerHub 、阿里云仓库)

查看官方是怎么做的！

![image-20200612233951676](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200612233951676.png)

![image-20200612234022746](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200612234022746.png)

很多官方镜像都是基础包，很多功能没有，我们通常会自己搭建自己的镜像！

官方既然可以制作镜像，那我们也可以！

## DockerFile构建过程

**基础知识：**

1、每个保留关键字(指令）都是必须是大写字母

2、执行从上到下顺序

3、# 表示注释

4、每一个指令都会创建提交一个新的镜像曾，并提交！

![image-20200612234419262](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200612234419262.png)

Dockerfile是面向开发的，我们以后要发布项目，做镜像，就需要编写dockerfile文件，这个文件十分简单！

Docker镜像逐渐成企业交付的标准，必须要掌握！

DockerFile：构建文件，定义了一切的步骤，源代码

DockerImages：通过DockerFile构建生成的镜像，最终发布和运行产品。

Docker容器：容器就是镜像运行起来提供服务。


## DockerFile的指令

```shell
FROM			# 基础镜像，一切从这里开始构建
MAINTAINER		# 镜像是谁写的，姓名+邮箱
RUN				# 镜像构建的时候需要运行的命令
ADD				# 步骤：tomcat镜像，这个tomcat压缩包！ 添加内容
WORKDIR			# 镜像的工作目录
VOLUME			# 挂载的目录
EXPOSE          # 暴露端口配置，跟 -p 是一个道理
CMD				# 指定这个容器启动时要执行的命令,只有最后一个命令会生效，可悲替代
ENTRYPOINT		# 指定这个容器启动的时候要执行的命令，可以追加命令
ONBUILD			# 当构建一个被继承DockerFile 这个时候就会运行ONBUILD的指令。触发指令
COPY			# 类似ADD,将我们文件拷贝到镜像中
ENV				# 构建的时候设置环境变量，跟 -e 是一个意思

# CMD 和 ENTRYPOINT 的区别说明：（后面也会介绍）
# 若CMD 和 ENTRYPOINT 后跟的都是 ls -a 这个命令，当docker run 一个容器时，添加了 -l 选项，则CMD里的ls -a 命令就会被替换成-l;而ENTRYPOINT中的 ls -a会追加-l变成 ls -a -l  
```

![image-20200613000838850](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613000838850.png)

## 实战测试

Docker Hub中99%镜像都是从这个基础镜像过来的( **FROM scratch** )，然后配置需要的软件和配置来构建。

![image-20200613001130237](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613001130237.png)

> 创建一个自己的 centos

```shell
# 1、编写DockerFile文件，内容如下：
[root@localhost dockerfile]# cat mydockerfile-centos
FROM centos						
MAINTAINER ztx<123456@qq.com> 

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "----end----"
CMD /bin/bash

# 2、通过这个文件构建镜像
# 命令docker build -f dockerfile文件路径 -t 镜像名:[tag] .
[root@localhost dockerfile]# docker build -f mydockerfile-centos -t mycentos:0.1 .
....
Successfully built c987078b06cb
Successfully tagged mycentos:0.1

# 3、测试运行
```

**对比：**

**之前的原生的centos**

![image-20200613004551789](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613004551789.png)

**我们增加之后的镜像**

![image-20200613005056516](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613005056516.png)

注：net-tools 包含一系列程序，构成了 Linux 网络的基础。

我们可以列出本地镜像的变更历史：

![image-20200613005625844](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613005625844.png)

我们平时拿到一个镜像，可以研究一下它是怎么做的！



> CMD 和 ENTRYPOINT 的区别

**测试CMD**

```shell
# 编写dockerfile文件
$ vim dockerfile-test-cmd
FROM centos
CMD ["ls","-a"]
# 构建镜像
$ docker build  -f dockerfile-test-cmd -t cmd-test:0.1 .
# 运行镜像
$ docker run cmd-test:0.1
.
..
.dockerenv
bin
dev

# 想追加一个命令  -l 成为ls -al
$ docker run cmd-test:0.1 -l
docker: Error response from daemon: OCI runtime create failed: container_linux.go:349: starting container process caused "exec: \"-l\":
 executable file not found in $PATH": unknown.
ERRO[0000] error waiting for container: context canceled 
# cmd的情况下 -l 替换了CMD["ls","-l"]。 -l  不是命令,所以报错
```

**测试ENTRYPOINT**

```shell
# 编写dockerfile文件
$ vim dockerfile-test-entrypoint
FROM centos
ENTRYPOINT ["ls","-a"]
$ docker run entrypoint-test:0.1
.
..
.dockerenv
bin
dev
etc
home
lib
lib64
lost+found ...
# 我们的命令，是直接拼接在我们的ENTRYPOINT命令后面的
$ docker run entrypoint-test:0.1 -l
total 56
drwxr-xr-x   1 root root 4096 May 16 06:32 .
drwxr-xr-x   1 root root 4096 May 16 06:32 ..
-rwxr-xr-x   1 root root    0 May 16 06:32 .dockerenv
lrwxrwxrwx   1 root root    7 May 11  2019 bin -> usr/bin
drwxr-xr-x   5 root root  340 May 16 06:32 dev
drwxr-xr-x   1 root root 4096 May 16 06:32 etc
drwxr-xr-x   2 root root 4096 May 11  2019 home
lrwxrwxrwx   1 root root    7 May 11  2019 lib -> usr/lib
lrwxrwxrwx   1 root root    9 May 11  2019 lib64 -> usr/lib64 ....
```

Dockerfile中很多命令都十分的相似，我们需要了解它们的区别，我们最好的学习就是对比他们然后测试效果！

## 实战：Tomcat镜像

1、准备镜像文件tomcat压缩包，jdk压缩包！

![image-20200613151500712](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613151500712.png)

2、编写Dockerfile文件，官方命名: **Dockerfile** ，build会自动寻找这个文件，就不要 -f 指定了！

```shell
FROM centos
MAINTAINER kuangshen<123456@qq.com>

COPY readme.txt /usr/local/readme.txt

ADD jdk-8u161-linux-x64.tar.gz    /usr/local/
ADD apache-tomcat-8.0.53.tar.gz   /usr/local

RUN yum -y install vim
ENV MYPATH /usr/local
WORKDIR $MYPATH


ENV JAVA_HOME /usr/local/jdk1.8.0_161
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-8.0.53
ENV CATALINA_BASH /usr/local/apache-tomcat-8.0.53
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin

EXPOSE 8080

CMD /usr/local/apache-tomcat-8.0.53/bin/startup.sh && tail -F /usr/local/apache-tomcat-8.0.53/bin/logs/catalina.out

```

3、构建镜像

```shell
# docker build -t diytomcat .     diytomcat是定义的镜像名
```

4、启动镜像，创建容器

```shell
# docker run -d -p 9090:8080 --name kuangshentomcat02 -v /home/kuangshen/build/tomcat/test:/usr/local/apache-tomcat-8.0.53/webapps/test -v /home/kuangshen/build/tomcat/tomcatlogs/:/usr/local/apache-tomcat-8.0.53/logs diytomcat

```

5、访问测试

![image-20200613175551231](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613175551231.png)

6、发布项目（由于做了卷挂载，我们就可以直接在本地发布项目了）

在/home/kuangshen/build/tomcat/test目录下创建WEB-INF目录，在里面创建web.xml文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
                               http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
           version="2.5">

</web-app>
```

在回到test目录，添加一个index.jsp页面：

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>hello kuangshen</title>vim
</head>
<body>
Hello World!<br/>
<%
System.out.println("---my test web logs---");
%>
</body>
</html>
```

发现：test项目部署成功，可以直接访问！

![image-20200613180033633](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613180033633.png)

注意：这时进入/home/kuangshen/build/tomcat/tomcatlogs/目录下就可以看到日志信息了：

```shell
[root@localhost tomcatlogs]# cat catalina.out 
```

![image-20200613180355186](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613180355186.png)

之前一直访问失败是web.xml配置有问题，最后也是查看该日志提示，才得以解决！！！

我们以后开发的步骤：需要掌握Dockerfile的编写！我们之后的一切都是使用docker镜像来发布运行！

## 发布自己的镜像

> Docker Hub

1、地址 https://hub.docker.com/

2、确定这个账号可以登录

3、在我们服务器上提交自己的镜像

```shell
[root@localhost tomcat]# docker login --help

Usage:	docker login [OPTIONS] [SERVER]

Log in to a Docker registry.
If no server is specified, the default is defined by the daemon.

Options:
  -p, --password string   Password
      --password-stdin    Take the password from stdin
  -u, --username string   Username

# 登录dockerhub
[root@localhost tomcat]# docker login -u ztx115
Password: 
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```

4、登录完毕后就可以提交镜像了，就是一步 docker push

```shell
# push自己的镜像到服务器上！
[root@localhost tomcat]# docker push diytomcat
The push refers to repository [docker.io/library/diytomcat]
c5593011cd68: Preparing 
d3ce40b8178e: Preparing 
02084c67dcc9: Preparing 
2b7c1c6c89c5: Preparing 
0683de282177: Preparing 
denied: requested access to the resource is denied  # 拒绝

# push镜像的问题？
# 解决：增加一个tag         docker tag  指定镜像的id   dockerhub的用户名/镜像重命名:[tag]
[root@localhost tomcat]# docker tag bb64ab96b432 ztx115/tomcat:1.0
```

![image-20200613211709842](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613211709842.png)

**注意：镜像的重命名前一定要加当前的dockerhub的用户名，否则将会push失败！！！！**（如：把ztx115改成ztx,  push一定失败！）

```shell
# docekr push上去即可！  自己平时发布的镜像尽量带上版本号
[root@localhost tomcat]# docker push ztx115/tomcat:1.0
The push refers to repository [docker.io/ztx115/tomcat]
c5593011cd68: Pushed 
d3ce40b8178e: Pushed 
02084c67dcc9: Pushed 
2b7c1c6c89c5: Pushed 
0683de282177: Pushed 
1.0: digest: sha256:b6733deccf85ad66c6f4302215dd9ea63e1579817f15a099b5858785708ed408 size: 1372
```

![image-20200613210147709](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613210147709.png)

发现，提交时也是按照镜像的层级来进行提交的！



> 发布到阿里云镜像服务上（狂神视频截图）

1、登录阿里云

2、找到容器镜像服务

3、创建命名空间

![image-20200613212823736](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613212823736.png)

4、创建容器镜像仓库

![image-20200613213014849](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613213014849.png)

![image-20200613213135466](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613213135466.png)

![image-20200613213222587](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613213222587.png)

5、浏览阿里云

![image-20200613214159792](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613214159792.png)



使用阿里云容器镜像的参考官方指南即可！！！（即上图）

## 小结

![image-20200613214846464](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613214846464.png)

---



# Docker网络

##  理解Docker0

清空所有环境

> 测试

![image-20200613224119526](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613224119526.png)

```shell
# 问题： docker是如何处理容器网络访问的？
```

![image-20200613220806390](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613220806390.png)

```shell
# [root@localhost /]# docker run -d -P --name tomcat01 tomcat

# 查看容器的内部网络地址   ip addr,  发现容器启动的时候会得到一个 eth0@if43 ip地址，docker分配的！
[root@localhost /]# docker exec -it tomcat01 ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
42: eth0@if43: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever

# 思考：linux能不能ping通docker容器内部！
[root@localhost /]# ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.476 ms
64 bytes from 172.17.0.2: icmp_seq=2 ttl=64 time=0.099 ms
64 bytes from 172.17.0.2: icmp_seq=3 ttl=64 time=0.105 ms
...
# linux 可以ping通docker容器内部
```

> 原理

1、我们每启动一个docker容器，docker就会给docker容器分配一个ip，我们只要装了docker，就会有一个docker01网卡。

桥接模式，使用的技术是veth-pair技术！

再次测试 ip addr，发现多了一对网卡 : 

![image-20200613224311838](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613224311838.png)

2、再启动一个容器测试，发现又多了一对网卡！！！

![image-20200613224610781](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613224610781.png)

```shell
# 我们发现这个容器带来网卡，都是一对对的
# veth-pair 就是一对的虚拟设备接口，他们都是成对出现的，一段连着协议，一段彼此相连
# 正因为有这个特性，veth-pair 充当一个桥梁，连接各种虚拟网络设备
# OpenStack，Docker容器之间的连接，OVS的连接都是使用veth-pair技术
```

3、我们来测试下tomcat01和tomcat02是否可以ping通！

```shell
[root@localhost /]# docker exec -it tomcat02 ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.556 ms
64 bytes from 172.17.0.2: icmp_seq=2 ttl=64 time=0.096 ms
64 bytes from 172.17.0.2: icmp_seq=3 ttl=64 time=0.111 ms
...

# 结论：容器与容器之间是可以相互ping通的！！！
```

**绘制一个网络模型图：**

![image-20200613231046553](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613231046553.png)

**结论：tomcat01 和 tomcat02 是公用一个路由器，即 docker0 !** 

所有的容器不指定网络的情况下，都是经 docker0 路由的，docker 会给我们的容器分配一个默认的可用ip



> 小结

Docker使用的是Linux的桥接技术，宿主机是一个Docker容器的网桥 docker0

![image-20200613232031835](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613232031835.png)

**注意：**Docker中所有网络接口都是虚拟的，虚拟的转发效率高！（内网传递文件）

只要容器一删除，对应的一对网桥就没有！

## --link

> 思考一个场景：我们编写了一个微服务，database url = ip ，项目不重启，数据库ip换掉了，我们希望可以处理这个问题，可以通过名字来访问容器？

```shell
# tomcat02 想通过直接ping 容器名（即"tomcat01"）来ping通，而不是ip，发现失败了！
[root@localhost /]# docker exec -it tomcat02 ping tomcat01
ping: tomcat01: Name or service not known

# 如何解决这个问题呢？
# 通过--link 就可以解决这个网络联通问题了！！！      发现新建的tomcat03可以ping通tomcat02
[root@localhost /]# docker run -d -P --name tomcat03 --link tomcat02 tomcat
87a0e5f5e6da34a7f043ff6210b57f92f40b24d0d4558462e7746b2e19902721
[root@localhost /]# docker exec -it tomcat03 ping tomcat02
PING tomcat02 (172.17.0.3) 56(84) bytes of data.
64 bytes from tomcat02 (172.17.0.3): icmp_seq=1 ttl=64 time=0.132 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=2 ttl=64 time=0.116 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=3 ttl=64 time=0.116 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=4 ttl=64 time=0.116 ms

# 反向能ping通吗？       发现tomcat02不能oing通tomcat03
[root@localhost /]# docker exec -it tomcat02 ping tomcat03
ping: tomcat03: Name or service not known
```

探究：inspect  ！！！

![image-20200614002609300](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200614002609300.png)

![image-20200614002832045](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200614002832045.png)

其实这个tomcat03就是在本地配置了到tomcat02的映射：

```shell
# 查看hosts 配置，在这里发现原理！  
[root@localhost /]# docker exec -it tomcat03 cat /etc/hosts
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
172.17.0.3	tomcat02 95303c12f6d9    # 就像windows中的 host 文件一样，做了地址绑定
172.17.0.4	87a0e5f5e6da
```

本质探究：--link  就是我们在hosts 配置中增加了一个 172.17.0.3	tomcat02   95303c12f6d9 （三条信息都是tomcat02 的）

我们现在玩Docker已经不建议使用 --link 了！！！

**自定义网络，不使用docker0！**

docker0问题：不支持容器名连接访问！

## 自定义网络

> 查看所有的docker网络

‘![image-20200614004445923](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200614004445923.png)

**网络模式**

bridge  ：桥接 （docker默认，自己创建也使用bridge模式！）

none ：不配置网络

host  ：和宿主机共享网络

container  ：容器网络连通，容器直接互联！（用的少！局限很大！）

**测试**

```shell
# 我们之前直接启动的命令 (默认是使用--net bridge，可省)，这个bridge就是我们的docker0 
docker run -d -P --name tomcat01 tomcat   
docker run -d -P --name tomcat01 --net bridge tomcat
# 上面两句等价

# docker0（即bridge）默认不支持域名访问 ！ --link可以打通连接，即支持域名访问！

# 我们可以自定义一个网络！
# --driver bridge    		网络模式定义为 ：桥接
# --subnet 192.168.0.0/16	定义子网 ，范围为：192.168.0.2 ~ 192.168.255.255
# --gateway 192.168.0.1		子网网关设为： 192.168.0.1 
[root@localhost /]# docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
7ee3adf259c8c3d86fce6fd2c2c9f85df94e6e57c2dce5449e69a5b024efc28c
[root@localhost /]# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
461bf576946c        bridge              bridge              local
c501704cf28e        host                host                local
7ee3adf259c8        mynet               bridge              local  	#自定义的网络
9354fbcc160f        none                null                local

```

**自己的网络就创建好了：**

![image-20200614011229854](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200614011229854.png)



```shell
[root@localhost /]# docker run -d -P --name tomcat-net-01 --net mynet tomcat
b168a37d31fcdc2ff172fd969e4de6de731adf53a2960eeae3dd9c24e14fac67
[root@localhost /]# docker run -d -P --name tomcat-net-02 --net mynet tomcat
c07d634e17152ca27e318c6fcf6c02e937e6d5e7a1631676a39166049a44c03c
[root@localhost /]# docker network inspect mynet
[
    {
        "Name": "mynet",
        "Id": "7ee3adf259c8c3d86fce6fd2c2c9f85df94e6e57c2dce5449e69a5b024efc28c",
        "Created": "2020-06-14T01:03:53.767960765+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.0.0/16",
                    "Gateway": "192.168.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "b168a37d31fcdc2ff172fd969e4de6de731adf53a2960eeae3dd9c24e14fac67": {
                "Name": "tomcat-net-01",
                "EndpointID": "f0af1c33fc5d47031650d07d5bc769e0333da0989f73f4503140151d0e13f789",
                "MacAddress": "02:42:c0:a8:00:02",
                "IPv4Address": "192.168.0.2/16",
                "IPv6Address": ""
            },
            "c07d634e17152ca27e318c6fcf6c02e937e6d5e7a1631676a39166049a44c03c": {
                "Name": "tomcat-net-02",
                "EndpointID": "ba114b9bd5f3b75983097aa82f71678653619733efc1835db857b3862e744fbc",
                "MacAddress": "02:42:c0:a8:00:03",
                "IPv4Address": "192.168.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]


# 再次测试 ping 连接
[root@localhost /]# docker exec -it tomcat-net-01 ping 192.168.0.3
PING 192.168.0.3 (192.168.0.3) 56(84) bytes of data.
64 bytes from 192.168.0.3: icmp_seq=1 ttl=64 time=0.199 ms
64 bytes from 192.168.0.3: icmp_seq=2 ttl=64 time=0.121 ms
^C
--- 192.168.0.3 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 2ms
rtt min/avg/max/mdev = 0.121/0.160/0.199/0.039 ms

# 现在不使用 --link,也可以ping 名字了！！！！！！
[root@localhost /]# docker exec -it tomcat-net-01 ping tomcat-net-02
PING tomcat-net-02 (192.168.0.3) 56(84) bytes of data.
64 bytes from tomcat-net-02.mynet (192.168.0.3): icmp_seq=1 ttl=64 time=0.145 ms
64 bytes from tomcat-net-02.mynet (192.168.0.3): icmp_seq=2 ttl=64 time=0.117 ms
^C
--- tomcat-net-02 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 3ms
rtt min/avg/max/mdev = 0.117/0.131/0.145/0.014 ms

```

我们在使用自定义的网络时，docker都已经帮我们维护好了对应关系，推荐我们平时这样使用网络！！！



好处：

redis——不同的集群使用不同的网络，保证了集群的安全和健康

mysql——不同的集群使用不同的网络，保证了集群的安全和健康

![image-20200614015209053](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200614015209053.png)

## 网络连通

![image-20200614013625192](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200614013625192.png)

![image-20200614013801842](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200614013801842.png)

```shell
# 测试打通 tomcat01 — mynet
[root@localhost /]# docker network connect mynet tomcat01

# 连通之后就是将 tomcat01 放到了 mynet 网络下！ （见下图）
# 这就产生了 一个容器有两个ip地址 ! 参考阿里云的公有ip和私有ip
[root@localhost /]# docker network inspect mynet
```

![image-20200614014544797](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200614014544797.png)

```shell
# tomcat01 连通ok
[root@localhost /]# docker exec -it tomcat01 ping tomcat-net-01
PING tomcat-net-01 (192.168.0.2) 56(84) bytes of data.
64 bytes from tomcat-net-01.mynet (192.168.0.2): icmp_seq=1 ttl=64 time=0.124 ms
64 bytes from tomcat-net-01.mynet (192.168.0.2): icmp_seq=2 ttl=64 time=0.162 ms
64 bytes from tomcat-net-01.mynet (192.168.0.2): icmp_seq=3 ttl=64 time=0.107 ms
^C
--- tomcat-net-01 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 3ms
rtt min/avg/max/mdev = 0.107/0.131/0.162/0.023 ms

# tomcat02 是依旧打不通的
[root@localhost /]# docker exec -it tomcat02 ping tomcat-net-01
ping: tomcat-net-01: Name or service not known
```

**结论：**假设要跨网络操作别人，就需要使用docker network connect  连通。。。

## 实战：部署Redis集群

![image-20200614124559172](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200614124559172.png)

启动6个redis容器，上面三个是主，下面三个是备！

使用shell脚本启动！

```shell
# 创建redis集群网络
docker network create redis --subnet 172.38.0.0/16

# 通过脚本创建六个redis配置
[root@localhost /]# for port in $(seq 1 6);\
> do \
> mkdir -p /mydata/redis/node-${port}/conf
> touch /mydata/redis/node-${port}/conf/redis.conf
> cat <<EOF>>/mydata/redis/node-${port}/conf/redis.conf
> port 6379
> bind 0.0.0.0
> cluster-enabled yes
> cluster-config-file nodes.conf
> cluster-node-timeout 5000
> cluster-announce-ip 172.38.0.1${port}
> cluster-announce-port 6379
> cluster-announce-bus-port 16379
> appendonly yes
> EOF
> done

# 查看创建的六个redis
[root@localhost /]# cd /mydata/
[root@localhost mydata]# \ls
redis
[root@localhost mydata]# cd redis/
[root@localhost redis]# ls
node-1  node-2  node-3  node-4  node-5  node-6

# 查看redis-1的配置信息
[root@localhost redis]# cd node-1
[root@localhost node-1]# ls
conf
[root@localhost node-1]# cd conf/
[root@localhost conf]# ls
redis.conf
[root@localhost conf]# cat redis.conf 
port 6379
bind 0.0.0.0
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
cluster-announce-ip 172.38.0.11
cluster-announce-port 6379
cluster-announce-bus-port 16379
appendonly yes
```



```shell
docker run -p 6371:6379 -p 16371:16379 --name redis-1 \
-v /mydata/redis/node-1/data:/data \
-v /mydata/redis/node-1/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.11 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf

docker run -p 6372:6379 -p 16372:16379 --name redis-2 \
-v /mydata/redis/node-2/data:/data \
-v /mydata/redis/node-2/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.12 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf

docker run -p 6373:6379 -p 16373:16379 --name redis-3 \
-v /mydata/redis/node-3/data:/data \
-v /mydata/redis/node-3/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.13 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf

docker run -p 6374:6379 -p 16374:16379 --name redis-4 \
-v /mydata/redis/node-4/data:/data \
-v /mydata/redis/node-4/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.14 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf

docker run -p 6375:6379 -p 16375:16379 --name redis-5 \
-v /mydata/redis/node-5/data:/data \
-v /mydata/redis/node-5/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.15 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf

docker run -p 6376:6379 -p 16376:16379 --name redis-6 \
-v /mydata/redis/node-6/data:/data \
-v /mydata/redis/node-6/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.16 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf
```

![image-20200614133829277](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200614133829277.png)



```shell
[root@localhost conf]# docker exec -it redis-1 /bin/sh
/data # ls
appendonly.aof  nodes.conf
/data # redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379 172.38.0.13:6379 172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379 --cluster-replicas 1
>>> Performing hash slots allocation on 6 nodes...
Master[0] -> Slots 0 - 5460
Master[1] -> Slots 5461 - 10922
Master[2] -> Slots 10923 - 16383
Adding replica 172.38.0.15:6379 to 172.38.0.11:6379
Adding replica 172.38.0.16:6379 to 172.38.0.12:6379
Adding replica 172.38.0.14:6379 to 172.38.0.13:6379
M: c5551e2a30c220fc9de9df2e34692f20f3382b32 172.38.0.11:6379
   slots:[0-5460] (5461 slots) master
M: d12ebd8c9e12dbbe22e7b9b18f0f143bdc14e94b 172.38.0.12:6379
   slots:[5461-10922] (5462 slots) master
M: 825146ce6ab80fbb46ec43fcfec1c6e2dac55157 172.38.0.13:6379
   slots:[10923-16383] (5461 slots) master
S: 9f810c0e15ac99af68e114a0ee4e32c4c7067e2b 172.38.0.14:6379
   replicates 825146ce6ab80fbb46ec43fcfec1c6e2dac55157
S: e370225bf57d6ef6d54ad8e3d5d745a52b382d1a 172.38.0.15:6379
   replicates c5551e2a30c220fc9de9df2e34692f20f3382b32
S: 79428c1d018dd29cf191678658008cbe5100b714 172.38.0.16:6379
   replicates d12ebd8c9e12dbbe22e7b9b18f0f143bdc14e94b
Can I set the above configuration? (type 'yes' to accept): yes
>>> Nodes configuration updated
>>> Assign a different config epoch to each node
>>> Sending CLUSTER MEET messages to join the cluster
Waiting for the cluster to join
....
>>> Performing Cluster Check (using node 172.38.0.11:6379)
M: c5551e2a30c220fc9de9df2e34692f20f3382b32 172.38.0.11:6379
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
S: 79428c1d018dd29cf191678658008cbe5100b714 172.38.0.16:6379
   slots: (0 slots) slave
   replicates d12ebd8c9e12dbbe22e7b9b18f0f143bdc14e94b
M: d12ebd8c9e12dbbe22e7b9b18f0f143bdc14e94b 172.38.0.12:6379
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
S: e370225bf57d6ef6d54ad8e3d5d745a52b382d1a 172.38.0.15:6379
   slots: (0 slots) slave
   replicates c5551e2a30c220fc9de9df2e34692f20f3382b32
S: 9f810c0e15ac99af68e114a0ee4e32c4c7067e2b 172.38.0.14:6379
   slots: (0 slots) slave
   replicates 825146ce6ab80fbb46ec43fcfec1c6e2dac55157
M: 825146ce6ab80fbb46ec43fcfec1c6e2dac55157 172.38.0.13:6379
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.

```

docker搭建redis集群完成！

![image-20200614141549867](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200614141549867.png)

我们使用docker之后，所有的技术都会慢慢变得简单起来！

---



# Springboot微服务打包Docker镜像

1、构建springboot项目，打包应用

![image-20200614155721369](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200614155721369.png)

2、编写Dockerfile，连同项目的jar包一并上传指定目录下

![image-20200614153734161](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200614153734161.png)

![image-20200614154114656](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200614154114656.png)

3、构建镜像

![image-20200614154355597](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200614154355597.png)

4、创建项目容器，发布运行！！！

![image-20200614155034087](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200614155034087.png)

![image-20200614155340519](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200614155340519.png)

以后我们使用了Docker之后，给别人交付就是一个镜像即可！

