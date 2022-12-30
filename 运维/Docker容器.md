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

> 业务场景

- Web 应用的自动化打包和发布。
- 自动化测试和持续集成、发布。
- 在服务型环境中部署和调整数据库或其他的后台应用。
- 从头编译或者扩展现有的 OpenShift 或 Cloud Foundry 平台来搭建自己的 PaaS 环境。



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

1. yum安装gcc相关环境（需要确保 虚拟机可以上外网）

   ```shell
   yum -y install gcc
   yum -y install gcc-c++
   ```

2. 卸载旧版本-直接复制即可

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

3. 安装需要的软件包

   ```shell
   #2.需要的安装包（安装 yum-utils 包(它提供了 yum-config-manager 实用程序)并设置存储库。）
   yum install -y yum-utils
   ```

4. 设置镜像的仓库

   ```shell
   yum-config-manager \
       --add-repo \
       https://download.docker.com/linux/centos/docker-ce.repo
   #上述方法默认是从国外的，不推荐
   
   #推荐使用国内的阿里云镜像-比较快
   yum-config-manager \
       --add-repo \
       https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
   ```

5. 更新yum软件包索引

   ```shell
   yum makecache fast
   ```

6. 安装Docker CE

   ```shell
   #安装docker docker-ce 社区版 而ee是企业版
   yum install docker-ce docker-ce-cli containerd.io # 这里我们使用社区版即可
   ```

7. 启动Docker

   ```shell
   systemctl start docker
   ```

8. 测试命令

   ```shell
   #使用docker version 查看是否安装成功
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
# 停止docker
systemctl stop docker

#1.卸载依赖
yum -y remove docker-ce docker-ce-cli containerd.io

#2. 删除资源
rm -rf  /var/lib/docker
# /var/lib/docker 	是docker的默认工作路径！
```

## 2.3 阿里云镜像加速

**1、登录阿里云找到容器服务——>镜像加速器**

![image-20221220111554431](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201115509.png)

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

## 2.5 虚拟化技术和容器化技术

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

注意：

1. 常见的坑，docker 容器使用后台运行，就必须要有一个前台进程，docker发现没有应用（对外提供的应用）会自动停止nginx,容器启动后，发现自己没有提供服务，就会立刻停止，就是没有程序了，所以应该是docker run -it 镜像名，然后不停止退出：Ctrl+p+q，当然还有一种办法能正常后台启动且不进去，那就是<mark>docker run -dit nginx</mark>
2. 如果启动的时候镜像不加tag版本号那么自动去找最新版！

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

> **docker network**用于管理网络，子命令创建，列出，检查，删除，连接和断开网络

https://www.yiibai.com/docker/network.html







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

## 7.2 容器与本地同步

### 1. 直接使用命令挂载

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

#### 实战：安装MySQL

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

####  具名和匿名挂载

```shell
# 匿名挂载，就跟Java的匿名类一样没有名字
-v 容器内路径
docker run -d -P --name nginx01 -v /etc/nginx nginx


# 查看所有卷的情况
[root@localhost data]# docker volume ls
DRIVER              VOLUME NAME
local               2dd0379216c9ee4441ed56f8ce53461c19abe78b8cfd024ac5fbe07c3b8f09ba

# 这里发现，这种就是匿名挂载，我们在 -v 后只写了容器内的路径，没有写容器外的路径！

# 具名挂载，注意不要在具名前加/斜杠，不然这就是绝对路径！
[root@localhost home]# docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx nginx
5ba5708389bf71b2156fdbcedc50a62b16ac27adb2a3dfac42c52e9da5ace79f
[root@localhost home]# docker volume ls
DRIVER              VOLUME NAME
local               juming-nginx

# 通过 -v 卷名：容器内路径
# 查看一下这个卷  # 先找到卷所在路径 docker volume inspect 卷名，如下图：
```

![image-20200611235522418](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212020920426.png)

所有的docker容器内的卷，没有指定目录的情况下都是在**/var/lib/docker/volumes/xxxx/_data**下！
我们通过具名挂载可以方便的找到我们的一个卷，大多数情况使用 **具名挂载**

```shell
# 如何确定是具名挂载还是匿名挂载，还是指定路径挂载！
-v	容器内路径		       # 匿名挂载，随机生成一串字符串作为名字
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

> **docker volume**管理Docker卷

![image-20221202093049109](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212020930276.png)

### 2. Dockerfile体验

Dockerfile 就是用来构建 docker镜像的构建文件！

Dockerfile就是一堆命令脚本！ 先体验一下！

通过这个脚本可以生成镜像，镜像是一层一层的，脚本是一个个的命令，每个命令都是最终镜像的一层！

```shell
# 创建一个dockerfile文件，名字可以随机，建议 dockerfile

[root@localhost docker-test-volume]# vim dockerfile
# 文件中的内容：指令(大写) 参数

# 当前镜像使用xx作为基础
FROM centos 

# 挂载卷
VOLUME ["volume01","volume02"]

# 生成完后发一段消息
CMD echo"----end----"

# 构建完后进入默认走这命令
CMD /bin/bash

# 这里的每个命令，就是镜像的一层！
```

build-构建

-f：文件地址

-t：目标文件名

![image-20200612003052844](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212051536177.png)

注意：我们这里的 dockerfile  是我们编写的文件名哦！

![image-20200612003717223](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212051537921.png)

这两个卷和外部一定有两个同步的目录！

![image-20200612003946028](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212051537596.png)

查看一下卷挂载在主机上的路径

**docker inspect 容器id**

![image-20200612004608027](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212051721988.png)

测试一下刚才的文件是否同步出去了！

这种方式我们未来使用十分的多，因为我们通常会构建自己的镜像！

假设构建镜像的时候没有挂在卷，要手动镜像挂载即可： (参考上文**具名和匿名挂载**)

```shell
-v 卷名:容器内路径 
```

## 7.3 容器与容器同步

本意：实现容器与容器之间数据的**共享**！

![image-20221205155218748](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212051552821.png)

核心命令：`--volumes-from`

1. **docker build -f dockerfile -t miaowei/centos .**构建一个dockerFile的镜像

2. **docker run -it --name docker01 miaowei/centos**  启动容器。注意：镜像没加tag版本就是去找最新,且命令docker01为父容器，接下来的02和03都是子容器

   ![image-20221205160849911](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212051608997.png)

3. **docker run -it --name docker02 --volumes-from docker01 miaowei/centos** 
   以继承的方式来达到父级容器的数据卷同步共享的目的

4. 进入到父容器docker01后在数据卷Volume01里创建一个文件，然后在子容器docker02里查看是否同步了---可以发现同步成功

   ![image-20221205165400108](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212051654184.png)

5. 在docker02里创建一个文件，再去docker01父容器看是否同步--同步成功！

   ![image-20221205170054298](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212051700359.png)

   

测试：

```shell
# 测试1：删除docker01后，docker02和docker03是否还可以访问原来docker01下创建的的文件？
# 测试1的结果为：依旧可以访问！！！

# 测试2：删除docker01后，docker02和docker03之间是否可以相互同步文件？
# 测试2的结果为：docket02和docker03之间依旧可以完成同步！！！

# 测试3：如果docker01退出并且停止运行，然后进入docker02中删除掉docker01创建的文件，然后再进去	docker01看是否同步？
#测试3的结果为：删除后重新登录docker01，数据已被删除，表示正常完成同步！！！
```

> 结论：远观来看是数据共享同步，围观来看是数据的拷贝与备份！
>
> ​		容器之间的配置信息的传递，数据卷容器的生命周期一直持续到没有容器使用为止。
>
> 但是一旦你持久化到了本地，这个时候，本地的数据是不会删除的！







**案例：多个mysql实现数据共享（不同容器装一个本地MySQL保证数据同步）**

```shell
➜  ~ docker run -d -p 3306:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7
➜  ~ docker run -d -p 3307:3306 -e MYSQL_ROOT_PASSWORD=123456 --name mysql02 --volumes-from mysql01  mysql:5.7
# 这个时候，可以实现两个容器数据同步！
```



---



# 8. DockerFile

## 8.1 DockerFile介绍

`dockerfile`是用来构建docker镜像的文件！命令参数脚本！

**构建步骤：**

1、 编写一个dockerfile文件

2、 `docker build` 构建称为一个镜像

3、 `docker run`运行镜像

4、 `docker push`发布镜像（DockerHub 、阿里云仓库)

> 命令`docker build -f dockerfile文件路径 -t 镜像名:[tag] .`

注意：

1. 如果dockerfile文件的名字是Dockerfile那么就可以省略**-f dockerfile**，系统会自动去匹配路径
2. 生成的镜像名必须全部为小写



查看官方是怎么做的！

![image-20221206094157600](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212060941691.png)

![image-20221208134339255](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212081343325.png)

很多官方镜像都是基础包，很多功能没有，我们通常会自己搭建自己的镜像！

官方既然可以制作镜像，那我们也可以！

## 8.2 DockerFile构建过程

**基础知识：**

1、每个保留关键字(指令）都是必须是大写字母

2、执行从上到下顺序

3、# 表示注释

4、每一个指令都会创建提交一个新的镜像层，并提交！

```dockerfile
#比如这里就是四层镜像
FROM centos 
# 挂载卷
VOLUME ["volume01","volume02"]
# 生成完后发一段消息
CMD echo"----end----"
# 构建完后进入默认走这命令
CMD /bin/bash
```

![image-20200612234419262](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212081405300.png)

Dockerfile是面向开发的，我们以后要发布项目，做镜像，就需要编写dockerfile文件，这个文件十分简单！

Docker镜像逐渐成企业交付的标准，必须要掌握！

**DockerFile**：构建文件，定义了一切的步骤，源代码

**DockerImages**：通过DockerFile构建生成的镜像，最终发布和运行产品。

**Docker容器**：容器就是镜像运行起来提供服务。


## 8.3 DockerFile的指令

```shell
FROM			# 基础镜像，一切从这里开始构建
MAINTAINER		# 镜像是谁写的，姓名+邮箱
RUN				# 镜像构建的时候需要运行的命令
ADD				# 步骤：tomcat镜像，这个tomcat压缩包！ 添加内容
WORKDIR			# 镜像的工作目录
VOLUME			# 挂载的目录
EXPOSE          # 暴露端口配置，跟 -p 是一个道理
CMD				# 指定这个容器启动时要执行的命令,只有最后一个命令会生效，可被替代
ENTRYPOINT		# 指定这个容器启动的时候要执行的命令，可以追加命令
ONBUILD			# 当构建一个被继承DockerFile 这个时候就会运行ONBUILD的指令。触发指令
COPY			# 类似ADD,将我们文件拷贝到镜像中
ENV				# 构建的时候设置环境变量，跟 -e 是一个意思

# CMD 和 ENTRYPOINT 的区别说明：（后面也会介绍）
# 若CMD 和 ENTRYPOINT 后跟的都是 ls -a 这个命令，当docker run 一个容器时，添加了 -l 选项，则CMD里的ls -a 命令就会被替换成-l;而ENTRYPOINT中的 ls -a会追加-l变成 ls -a -l  
```

![image-20200613000838850](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212081429197.png)

## 8.4 实战测试

Docker Hub中99%镜像都是从这个基础镜像过来的( **FROM scratch** )，然后配置需要的软件和配置来构建。

> 创建一个自己的 centos

```shell
# 1、编写DockerFile文件，内容如下：
[root@localhost dockerfile]# cat mydockerfile-centos
# 来自于centos基础镜像
FROM centos
# 标注这个镜像的作者信息
MAINTAINER ztx<123456@qq.com> 
# 设置环境变量 
ENV MYPATH /usr/local
# 设置当前镜像的工作目录
WORKDIR $MYPATH
# 在线安装vim跟net-tools
RUN yum -y install vim
RUN yum -y install net-tools
# 指定对外端口
EXPOSE 80
# cmd执行并进入命令行
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

![image-20200613004551789](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212081451705.png)

**我们增加之后的镜像**

![image-20200613005056516](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212081451834.png)

注：net-tools 包含一系列程序，构成了 Linux 网络的基础。

我们可以列出本地镜像的一步一步变更历史：

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

## 8.5 实战：Tomcat镜像

1、准备tomcat和jdk 到当前目录，编写好README帮助文档 （touch README.txt）

![image-20221209112531882](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212091125951.png)

2、编写dockerfile文件，官方命名：`Dokerfile`，build会自动寻找这个文件，就不需要-f指定了！

```dockerfile
FROM centos:7 										# 基础镜像centos
MAINTAINER miaowei<2439135122@qq.com>					# 作者


COPY README.txt /usr/local/README.txt 				# 复制README文件
ADD jdk-8u351-linux-x64.tar.gz /usr/local/ 			# 添加jdk，ADD 命令会自动解压，根据自己下载的压缩包进行修改
ADD apache-tomcat-9.0.69.tar.gz /usr/local/ 		# 添加tomcat，ADD 命令会自动解压

RUN yum -y install vim								# 安装 vim 命令
ENV MYPATH /usr/local 								# 环境变量设置 工作目录
WORKDIR $MYPATH

ENV JAVA_HOME /usr/local/jdk1.8.0_351 				# 环境变量： JAVA_HOME环境变量
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.69 	# 环境变量： tomcat环境变量
ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.69
 
# 设置环境变量 分隔符是：
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin 	

EXPOSE 8080 										# 设置暴露的端口

# && 多个命令一起执行
# tail -F 打印输出且一直监听命令
CMD /usr/local/apache-tomcat-9.0.69/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.69/logs/catalina.out 		                    # 设置默认命令

#保存:wq
```

3、构建镜像

```shell
# 因为dockerfile命名使用默认命名 因此不用使用-f 指定文件
$ docker build -t mytomcat:0.1 .
```

4、启动镜像，创建容器

```shell
# -d:后台运行 -p:暴露端口 --name:别名 -v:绑定路径 
$ docker run -d -p 8080:8080 --name tomcat01 -v /home/miao/build/tomcat/test:/usr/local/apache-tomcat-9.0.62/webapps/test -v /home/miao/build/tomcat/tomcatlogs/:/usr/local/apache-tomcat-9.0.62/logs mytomcat:0.1
```

5、访问测试-成功

![image-20221209112909285](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212091129385.png)

6、发布项目（由于做了卷挂载，我们就可以直接在本地发布项目了）

走到挂载在本地的目录<mark> /home/miao/build/tomcat/test</mark>

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

![image-20221209113604311](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212091136435.png)

注意：这时进入<mark>/home/miao/build/tomcat/tomcatlogs</mark>目录下就可以看到日志信息了：

```shell
[root@localhost tomcatlogs]# cat catalina.out 
```

![image-20200613180355186](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212091031692.png)

我们以后开发的步骤：需要掌握Dockerfile的编写！我们之后的一切都是使用docker镜像来发布运行！

## 8.6 发布自己的镜像

###  DockerHub

1、地址 https://hub.docker.com/

2、确定这个账号可以登录

3、在我们服务器上提交自己的镜像

![image-20221209162909673](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212091629767.png)

4、登录完毕后就可以提交镜像了，就是一步 docker push

​	![image-20221209163843569](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212091638654.png)

​	解决：

```shell
# 第一种 build的时候添加你的dockerhub用户名，然后在push就可以放到自己的仓库了
# 作者名/镜像名:版本号
$ docker build -t miaowei8721/mytomcat:0.1 .

# 第二种：增加一个tag         docker tag  指定镜像的id   dockerhub的用户名/镜像重命名:[tag]
$ docker tag 容器id miaowei8721/mytomcat:1.0 
# 再次push
$ docker push hxl/mytomcat:1.0
```

**注意：镜像的重命名前一定要加当前的dockerhub的用户名，否则将会push失败！！！！**（如：把miaowei8721改成任意内容,  push一定失败！）

![image-20221209165037159](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212091650248.png)

发现

1. 提交时也是按照镜像的层级来进行提交的！
2. 自己平时发布的镜像尽量带上版本号
3. 推送的话会比较很慢，可以开VPN！
4. 由于推送到DockerHub上所以在tag更换标签时 前面为dockerhub用户名/自定义名字:版本号



### 阿里云

> 发布到阿里云镜像服务上

1、登录阿里云

2、找到容器镜像服务

3、创建命名空间

![image-20221209165447211](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212091654276.png)

4、创建容器镜像仓库

![image-20221209165625036](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212091656106.png)

​	选择本地仓库并点击创建

5、浏览阿里云，照着步骤来就行

![image-20221209171114269](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212091711369.png)

> 手动演示进行推送阿里云镜像仓库
>

第一步：登录阿里云镜像空间

```shell
docker login --username=喵小威 registry.cn-chengdu.aliyuncs.com
```

第二步：更改tag推送信息,这里版本号自定义由本身业务决定

```shell
docker tag [ImageId] registry.cn-chengdu.aliyuncs.com/miaowei_save/miaowei-repository:[镜像版本号]
```

第三步：将更换后的镜像进行推送，这里版本号是上一步的版本号

```shell
docker push registry.cn-chengdu.aliyuncs.com/miaowei_save/miaowei-repository:[镜像版本号]
```

![image-20221209172010545](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212091720628.png)

此时我们去看阿里云仓库：

![image-20221212090636346](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212120906442.png)

填写好要拉取的版本号就可以直接将阿里云远程仓库的镜像拉取到本地了：

```shell
docker pull registry.cn-chengdu.aliyuncs.com/miaowei_save/miaowei-repository:[镜像版本号]
```



注：阿里云仓库的话其实会发现一个仓库里推送的话只能根据版本号来进行拉取，所以在实际业务开发过程中一个仓库就代表一个项目！

## 8.7 小结

![image-20200613214846464](C:\Users\MiaoDaWei\Desktop\狂神docker笔记（超详细）\docekr进阶\docker容器数据卷.assets\image-20200613214846464.png)

​	用Dockerfile编写所有的源代码用`build`构建一个镜像，然后使用`tag`更换标签再`push`到远程仓库，需要时就`pull`拉取到本地！

​	镜像使用`run`运行为容器，容器本身可以`stop、start、restart`进行停止、启动、重启，当操作好以后就`commit`提交为新的层级镜像。

​	如果想把镜像提交打包为tar压缩包则可以使用`save`命令，然后想加载则使用`load`。---本质上跟DockerHub远程是一样的道理，这里只是备份tar解压和压缩，而远程则直接拉取和提交!



---



# 9. Docker网络(容器互联)

##  9.1 Docker的网卡-docker0

首先随便启动一个容器，然后在Linux的终端输入：`ip addr`

![image-20221212141416283](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212121414460.png)

出现了四个网卡：

1. lo：本机地址<mark>127.0.0.1</mark>
2. etho：云服务器内网地址<mark>172.20.203.94</mark>
3. docker0: docker地址<mark>172.17.0.1</mark>
4. `163: veth0c2a844@if162`:这个是某容器内部的网卡

------

进入到此容器内部后执行`ip addr`

> 如果在容器内部中出现bash:ip:commandnot found
>
> 解决办法：
>
> 1. **yum -y install initscripts**
> 2. https://blog.csdn.net/winnyrain/article/details/127313184

![image-20221212142809517](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212121428654.png)

发现：容器在启动的时候会分配一个网卡，而这个网卡是跟容器外某个网卡是意义一一对应的，并且在你发现docker的地址是**172.17.0.1**跟容器内的ip地址**172.17.0.2**是递增并且在Linux端去ping是可以ping通的容器内部的！

> 问题：bash: ping: command not found

执行命令：

```shell
#执行以下命令
apt-get update
apt install iputils-ping
apt install net-tools
```

![在这里插入图片描述](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212211732259.png)

> 原理

1. 我们每启动一个docker容器, docker就会给docker容器分配一个ip ,我们只要安装了docker ,就会有一个网卡docker0。

2. 该技术被称为桥接模式,使用的技术是evth-pair 技术。evth-pair 就是`成对`的虚拟设备接口(容器带来网卡都是一对的)，他们都是成对出现的正因为有这个特性，evth-pair充当一个桥粱，连接各种虚拟网络设备的。**一端连着协议，一端彼此相连**

3. docker两个容器之间的网络交互也是可以ping通的，使用的也是evth-pair技术
4. docker0（Linux那端）充当docker两个容器之间的路由器，也就是说**docker两个容器之间不是直接互联的，而是通过docker0链接**

网络模型画图：

![image-20221212161355034](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212121613112.png)

**结论：tomcat01 和 tomcat02 是公用一个路由器，即 docker0 !** 

所有的容器不指定网络的情况下，都是经 docker0 路由的，docker 会给我们的容器分配一个默认的可用ip

**也就是说dokcer0是docker容器们的网关，docker容器们共同构成一个子网**

> 小结

Docker使用的是Linux的桥接技术，宿主机是一个Docker容器的网桥 docker0

![image-20200613232031835](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212121618867.png)

**注意：**Docker中所有网络接口都是虚拟的，虚拟的转发效率高！（内网传递文件）

只要容器一删除，对应的一对网桥就没有！

## 9.2 容器互联的方法

### --link

**优点**：直接在tomcat02里面ping tomcat01 容器名，照样ping通

**缺点**：单向绑定，较为麻烦！

**效果**：该命令可以把两个容器连接为一组容器，该命令主要解决的问题是：在tomcat02容器中ping tomcat01 容器，不需要ping tomcat01 容器的ip，只需要ping tomcat01 容器名（就是tomcat01）即可。

> 业务场景：在一个微服务中我们可以通过ip进行连接，但是万一ip更换了重新分配，那么就连接失败了，那么在这里容器互联如果能用名字来连接访问容器呢？

**--link=[]: 添加链接到另一个容器；**

例：docker run -d -P --name tomcat03 --link tomcat02 tomcat

在tomcat03里去ping tomcat2

![image-20221212171410591](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212121714706.png)

那么反向ping试试呢？-发现tomcat02不能ping通tomcat03

![image-20221212171502960](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212121715039.png)

-----

1. **docker network ls **列出网络
2. **docker network inspect** 显示一个或多个网络的详细信息

![image-20200614002609300](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212131000164.png)

![image-20200614002832045](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212131016300.png)



**该命令原理其实就是在A容器中配置host，使B容器的ip映射到B容器名。**

```shell
root@ec1742fd1571:/usr/local/tomcat# cat /etc/hosts
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
172.18.0.2	tomcat01 dce458284287 # 就是这一行给配置了host  就像windows中的 host 文件一样，做了地址绑定
172.18.0.4	ec1742fd1571
```



### 自定义网络

> 查看所有的docker网络

![image-20221213104219083](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212131042182.png)

**网络模式**

bridge  ：桥接 （docker默认，自己创建也使用bridge模式！）

none ：不配置网络

host  ：和宿主机共享网络

container  ：容器网络连通，容器直接互联！（用的少！局限很大！）

**测试**

**使用 --net 命令选择网络**

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
# 192.168.0.0/16: 有了计算机网络的CIDR记法知识（和前文说的原理一样），可知该命令指定了前16位是网络号，后16位是主机号。能分配的主机个数是65535个；默认网关是192.168.0.1 ， 网络名就叫mynet
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

![image-20200614011229854](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212131118666.png)



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

![image-20200614015209053](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212131118117.png)

**那么两个子网之间是隔离的。这样可以保证集群的安全和健康！！！**
**可是如果有需求让两个子网联通，那么该怎么操作呢？引出了下一个话题——网络连通**

## 9.3 网络连通

![img](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212131131858.png)

**想实现上图的需求，需要先将 tomcat-01 与【mynet】 这个网关打通（见黄色字体），才能做到容器间打通**

`docker network connect` 命令用于将容器连接到网络。可以按名称或ID连接容器。 一旦连接，容器可以与同一网络中的其他容器通信 

<mark>实现的是不同网段的容器实现通信</mark>

选项：

- `--alias` 为容器添加网络范围的别名  
- `--ip` 指定IP地址 
- `--ip6` 指定IPv6地址 
- `--link` 添加链接到另一个容器
- `--link-local-ip` 添加容器的链接本地地址 

示例：

![image-20200614013625192](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212141545859.png)

![image-20200614013801842](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212141545086.png)

```shell
# 测试打通 tomcat01 — mynet
[root@localhost /]# docker network connect mynet tomcat01

# 连通之后就是将 tomcat01 放到了 mynet 网络下！ （见下图）
# 这就产生了 一个容器有两个ip地址 ! 参考阿里云的公有ip和私有ip
[root@localhost /]# docker network inspect mynet
```

你会发现在mynet的元数据中除了tomcat-net-01、02还加了一个刚刚放入的tomcat01容器网络！

![image-20200614014544797](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212141545736.png)

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

# 10. 实战



## 1. 实战：部署Redis集群

![image-20200614124559172](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212141601223.png)

启动6个redis容器，上面三个是主，下面三个是备！ 如果发现有一个主挂了那么备会立即顶上去

<mark>如果是集群那么这里就要建立redis集群的</mark>



步骤：

1. 创建redis的集群网络

   ```shell
   docker network create redis --subnet 172.38.0.0/16
   ```

2. 执行shell脚本

   ```shell
   #通过脚本一次创建6个redis配置
   for port in $(seq 1 6); \
   do \
   mkdir -p /mydata/redis/node-${port}/conf
   touch /mydata/redis/node-${port}/conf/redis.conf
   cat << EOF >/mydata/redis/node-${port}/conf/redis.conf
   port 6379
   bind 0.0.0.0
   cluster-enabled yes
   cluster-config-file nodes.conf
   cluster-node-timeout 5000
   cluster-announce-ip 172.38.0.1${port}
   cluster-announce-port 6379
   cluster-announce-bus-port 16379
   appendonly yes
   EOF
   
   # 通过脚本一次启动6个redis容器
   docker run -p 637${port}:6379 -p 1637${port}:16379 --name redis-${port} \
   -v /mydata/redis/node-${port}/data:/data \
   -v /mydata/redis/node-${port}/conf/redis.conf:/etc/redis/redis.conf \
   -d --net redis --ip 172.38.0.1${port} redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf
   
   done
   ```

​	![image-20221216093828406](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212160938589.png)

​	

![image-20221216093851501](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212160938590.png)

3. 查看正在运行的容器

   ![image-20221216094025952](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212160940102.png)

4. 创建集群

   ```shell
   # docker exec -it redis-1 /bin/sh
   # redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379 172.38.0.13:6379 172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379 --cluster-replicas 1
   ```

5. 开始测试

<img src="https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212161004371.png" alt="image-20200614141549867" style="zoom:200%;" />



---



## 2. Springboot微服务打包Docker镜像

1、构建springboot项目，打包应用

![image-20200614155721369](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212161029649.png)

2、编写Dockerfile，连同项目的jar包一并上传指定目录下

![image-20200614153734161](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212161030689.png)

![image-20200614154114656](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212161030487.png)

3、构建镜像

![image-20200614154355597](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212161030965.png)

4、创建项目容器，发布运行！！！

![image-20200614155034087](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212161030395.png)

![image-20200614155340519](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212161030146.png)

以后我们使用了Docker之后，给别人交付就是一个镜像即可！我们也可以把自己的镜像push到阿里云上



# 11. Docker Compose

## 概念

​	**Docker-Compose** 是用来管理容器的，类似用户容器管家，我们有N多台容器或者应用需要启动的时候，如果手动去操作，是非常耗费时间的，如果有了 **Docker-Compose** 只需要一个配置文件就可以帮我们搞定，但是 **Docker-Compose** 只能管理当前主机上的 Docker，不能去管理其他服务器上的服务。意思就是单机环境。docker-compose是基于docker的编排工具，使容器的操作能够批量的，可视的执行，是一个管理多个容器的工具，比如可以解决容器之间的依赖关系，当在宿主机启动较多的容器时候，如果都是手动操作会觉得比较麻烦而且容器出错，这个时候推荐使用 dockerd的单机编排工具 docker-compose。

​	docker-compose是基于docker的开源项目，托管于github上，由python实现，调用 docker服务的API负责实现对docker容器集群的快速编排，即通过一个单独的yaml文件，来定义一组相关的容器来为一个项目服务。

　　所以，docker-compose默认的管理对象是项目，通过子命令的方式对项目中的一组容器进行生命周期的管理。

> 理解: docker-compose就是一个单机批量操作多容器的一键部署解决方案。

**在后面我们可以docker-compose up 100个服务。**

相关术语：

1. 服务 (service)：一个应用容器，实际上可以运行多个相同镜像的实例
2. 项目 (project)：由一组关联的应用容器组成的一个完整业务单元

可见，一个项目可以由多个服务（容器）关联而成，Compose 面向项目进行管理

## 安装-卸载

```shell
# 官网提供 （没有下载成功-很慢）
curl -L "https://github.com/docker/compose/releases/download/2.14.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
 
# 国内地址，下载速度很快
curl -L https://get.daocloud.io/docker/compose/releases/download/2.14.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```

授权：

```shell
chmod +x /usr/local/bin/docker-compose
```

验证是否安装成功:

![image-20221219095623047](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212190956190.png)

卸载：

```shell
rm /usr/local/bin/docker-compose
```

**以上是通过二进制包方式安装的，所以这里删除二进制文件即可**

## 快速开始

这是官方快速开始的例子：https://docs.docker.com/compose/gettingstarted/

这里例子的话根据官网来的，一个计数器!

1. 创建目录

   ```shell
   mkdir composetest
   cd composetest
   ```

2. 目录下创建文件`app.py`,并复制其内容

   ```python
   import time
   
   import redis
   from flask import Flask
   
   app = Flask(__name__)
   cache = redis.Redis(host='redis', port=6379)
   
   def get_hit_count():
       retries = 5
       while True:
           try:
               return cache.incr('hits')
           except redis.exceptions.ConnectionError as exc:
               if retries == 0:
                   raise exc
               retries -= 1
               time.sleep(0.5)
   
   @app.route('/')
   def hello():
       count = get_hit_count()
       return 'Hello World! I have been seen {} times.\n'.format(count)
   ```

3. 创建一个导入的依赖包`requirements.txt`文件，并复制其内容：

   ```
   flask
   redis
   ```

4. 创建一个`Dockerfile`文件，并复制其内容：

   ```dockerfile
   # syntax=docker/dockerfile:1
   FROM python:3.7-alpine
   WORKDIR /code
   ENV FLASK_APP=app.py
   ENV FLASK_RUN_HOST=0.0.0.0
   RUN apk add --no-cache gcc musl-dev linux-headers
   COPY requirements.txt requirements.txt
   RUN pip install -r requirements.txt
   EXPOSE 5000
   COPY . .
   CMD ["flask", "run"]
   ```

5. 创建一个`docker-compose.yml`文件

   ```yaml
   version: "3"	
   services:	# 服务列表
     web:
       build: .	# 同价为dockerfile进行build进行构建，如果这里只写一个.那么就表示会自动去寻找构建Dockerfile文件，如果想要指定则下一行输入 dockerfile：文件地址名称
       ports:
         - "8000:5000"		# 暴露端口
     redis:
       image: "redis:alpine" 	#从官方镜像上进行拉取镜像
   ```

6. 运行命令`docker-compose up`,然后看效果：

   ![image-20221219110229491](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212191102558.png)

注意：如果Docker-compose.yml不在当前目录下，需要指定，则需要在`docker-compose up 文件地址`来指定

> 核心就四步骤：

1. 准备应用-项目应用
2. 编辑Dockerfile文件并准备打包镜像
3. Docker-compose yaml文件（定义整个服务，需要的环境 web、redis） 完整的上线服务！
4. 启动compose项目（docker-compose up）

流程：

1. 创建网络
2. 执行Docker-compose.yaml
3. 启动服务![image-20221219111905207](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212191119378.png)



## 默认规则

1. 在启动后查看镜像：

![image-20221219112410326](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212191124399.png)

发现：以前我们需要手动的一个一个pull拉取run运行，现在只需要在yaml里面配置即可!就可批量启动！

2. 查看启动后的容器：

   ![image-20221219113450957](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212191134014.png)

这里命名格式为：**文件夹名\_服务名_num**

解释：在集群中如果有多个相同服务，那么这里的num就表示副本数量

记住一点：我们的服务都不可能只有一个运行实例，我们的服务都是弹性的！

3. 网络规则

查看网络：

![image-20221219132716778](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212191327848.png)

这是刚刚会默认创建的网络，我们再看一下这个元数据：

![image-20221219132904932](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212191329049.png)

此时我们发现其中两个服务已自动添加到当前网络并配置ip地址，这样有个好处就是：

在我们访问其redis的主机时就可以不用以ip来ping而是以域名来ping，比如：

```python
在刚刚上面写的app.py里访问redis的主机
cache = redis.Redis(host='redis', port=6379)
好处：不管每次启动或者ip变化而去修改，以域名访问也可以
```

## 相关命令

**docker-compose**命令要在指定有docker-compose.yml下进行使用，因为这些命令也是要找到后才会执行，否则就会失败！

> 停止容器

1. docker-compose down
2. ctrl+c

![image-20221219142032576](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212191420659.png)

发现：这里停止了对应的容器后进行移除，并且移除了网络！

并且这是批量操作的省略了我们一个一个停止的操作！

**通过docker-compose编写yaml配置文件，就可以通过compose一键启动所有服务，停止！**

## yaml规则

https://docs.docker.com/compose/compose-file/#depends_on

编写规则其实只要记住核心就**三层**。

```yaml
# 3层
version: ' ' 	# 1. 版本
services:	# 2. 服务
	服务1：web
		# 服务配置
		images:	
        	build:
        	network:
	服务2：redis
	....
# 3. 其他配置 网络/卷、 全局规则
volumes:
networks:
configs:
```

个别注意的点：

> depends_on： 项目中出现启动顺序问题时可以由这个进行指定

案例：

```yaml
services:
  web:
    build: .
    depends_on:	# 启动web时先启动db、redis
      - db
      - redis
  redis:
    image: redis
  db:
    image: postgres
```

## 实战

### 博客

根据官方例子:https://github.com/docker/awesome-compose/tree/master/official-documentation-samples/wordpress/



1. 编写`docker-compose.yaml`文件：

   ```yaml
   #指定 docker-compose.yml 文件的版本
   version: '3.3'
   
   # 定义所有的 service 信息, services 下面的第一级别的 key 既是一个 service 的名称
   services:
      db:
        image: mysql:5.7
        volumes:
          - db_data:/var/lib/mysql
        # 定义容器重启策略
        restart: always
        # 设置环境变量， environment 的值可以覆盖 env_file 的值 
        environment:
          MYSQL_ROOT_PASSWORD: somewordpress
          MYSQL_DATABASE: wordpress
          MYSQL_USER: wordpress
          MYSQL_PASSWORD: wordpress
   
      wordpress:
        #docker-compose up 以依赖顺序启动服务，先启动db
        depends_on:
          - db
        image: wordpress:latest
        # 建立宿主机和容器之间的端口映射关系,容器的 80 端口和宿主机的 8000 端口建立映射关系
        ports:
          - "8000:80"
        restart: always
        environment:
          WORDPRESS_DB_HOST: db:3306
          WORDPRESS_DB_USER: wordpress
          WORDPRESS_DB_PASSWORD: wordpress
          WORDPRESS_DB_NAME: wordpress
   # 定义容器和宿主机的卷映射关系, 其和 networks 一样可以位于 services 键的二级key和 compose 顶级key, 如果需要跨服务间使用则在顶级key定义, 在 services 中引用
   volumes:
       db_data: {}
   ```

2. 如果需要文件，比如我们自己的本地应用等则需要编辑Dockerfile文件

3. 一键启动容器

   ```dockerfile
   dokcer-compose up -d
   ```

4. 如果需要停止

   ```sh
   该命令docker compose down删除容器和默认网络，但保留您的 WordPress 数据库。
   
   该命令docker compose down --volumes删除容器、默认网络和 WordPress 数据库。
   ```

   ![img](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212191658169.png)





### 自己编写服务上线

1. 新建一个springBoot服务，此处我springBoot版本`2.5.14`,java版本`11`,版本过高报了些奇奇怪怪的异常

2. 新建controller

   ```java
   @RestController
   public class TestController {
   
       @Autowired
       private StringRedisTemplate redisTemplate;
   
       @GetMapping("/hello")
       public String test(){
           Long increment = redisTemplate.opsForValue().increment("a");
           return "Hello miaowei，you is good " + increment;
       }
   }
   ```

3. 编写application.properties

   ```properties
   # 这里连接redis的主机直接就是域名，这里名字是docker-compose.yml里的redis的名字
   spring.redis.host=redis    
   ```

4. 新建Dockerfile

   ```dockerfile
   FROM openjdk:11	# 传统java:11 在官方仓库里已经没有了
   
   COPY *.jar /app.jar	# 将外层里的jar包复制进该构建的镜像里面去
   
   CMD ["--server.port=8080"]
   
   EXPOSE 8080
   
   ENTRYPOINT ["java","-jar","/app.jar"]	# 后面在run启动该容器时执行该jar包
   ```

5. 新建docker-compose.yml

   ```yaml
   version: '3.3'
   services:
     miaoapp:	# 服务名，后面run容器的中会出现这个名称
       build: .	# 后面有个点，表示在当前目录下寻找Dockerfile文件并进行构建
       image: miaoweiapp	# 标注这个镜像的名字
       depends_on:
         - redis	# 表示优先启动redis这个服务
       ports:		# 对外展示的端口
         - "8080:8080"
     redis:	# 这里名字自定，在jar包中连接redis，主机名跟这个保持一致就可连接
       image: "redis:alpine"	# 镜像
   ```

6. 复制到Linux中同一目录下（将写好的代码进行打包）：

   ![image-20221220101923716](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201019784.png)

7. 执行命令`docker-compose up -d`

   ![image-20221220102221317](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201022378.png)

8. 界面访问：

   ![image-20221220102251526](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201022585.png)

9. 查看运行的容器：

   ![image-20221220102512952](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201025014.png)

10. 结束停止访问`docker-compose down`

> 记住：所有的docker-compose的命令都是建立在当前目录下有docker-compose.yml文件才能执行，否则都是找不到！

## 小结

- Docker Compose是一个开源项目，用来统一编排多个应用的
- 多个应用=服务->这些应用都会在同一个网络下->可通过域名直接访问
- 默认的服务名为：文件夹名称\_镜像名_副本数量（运行数量）
- Dockerfile 用于构建镜像，docker-compose.yaml 用于编排项目，同时组装多个应用
- docker-compose.yaml 文件结构总共三层
  - docker compose版本
  - 服务
  - 其他配置
- 如何使用:
  1. 准备好项目并编写 Dockerfile（可省略，如果直接使用仓库镜像的话）
  2. 编写docker-compose.yaml 编排项目
  3. 在当前目录（docker-compose.yaml文件所在目录）运行`docker-compose up`; 停止`docker-compose down`

# 12. Docker Swarm

## 概念

官网文档连接：https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/

解释：

​	Docker Engine 1.12 引入了 swarm 模式，使您能够创建一个或多个 Docker 引擎的集群，称为 swarm。**一个 swarm 由一个或多个节点组成**

架构：

![image-20221220153029223](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201530540.png)



> 工作模式

节点分为两种：manages和works。

![Swarm 模式集群](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201322061.png)

有图可知：

1. manager是管理节点，worker是工作节点
2. 操作都在manager节点上，且工作节点之间是可以互相管理的。manager可以操作worker，但worker不能操作manager。
3. Raft一致性算法是为了保证上面三个管理节点的可用性，无论谁挂了都可以保证一台节点的存活！整个成为一个高可用！



**注意**：Raft协议要求管理节点至少得3台以上，如果是双主，一个主节点挂了，另外一个不能继续使用！

```ejs
Error response from daemon: rpc error: code = Unknown desc = The swarm does not have a leader. It's possible that too few managers are online. Make sure more than half of the managers are online.
```

## 常用命令

```dockerfile
# 初始化集群
docker swarm init 	#针对机器只有一个IP的情况
docker swarm init --advertise-addr 172.16.1.13 # 针对机器有多个IP的情况，需要指定一个IP，一般都是指定内网IP
 
# 查看工作节点的 token
 docker swarm join-token worker 	
 
# 查看管理节点的 token
 docker swarm join-token manager
 
# 加入集群
 docker swarm join
```

节点相关：

```shell
# 查看集群所有节点
 docker node ls 
	
# 查看当前节点所有任务
 docker node ps
 
# 删除节点（-f强制删除）
docker node rm 节点名称| 节点ID
 
# 查看节点详情
docker node inspect 节点名称| 节点ID 
	
# 节点降级，由管理节点降级为工作节点
docker node demote 节点名称| 节点ID 	
 
# 节点升级，由工作节点升级为管理节点
docker node promote 节点名称| 节点ID 	
 
# 更新节点
docker node update 节点名称| 节点ID 	
```

服务相关：

service的相关命令：https://blog.csdn.net/cold___play/article/details/125341414

```shell
#创建服务
 docker service create
 
# 查看所有服务
 docker service ls 	
 
# 查看服务的详细信息
 docker service inspect 服务名称|服务ID
 	
# 查看服务日志
 docker service logs 服务名称|服务ID
 	
# 删除服务（-f强制删除）
 docker service rm 服务名称|服务ID
 	
# 设置服务数量
 docker service scale 服务名称|服务ID=n 	
 
# 更新服务
 docker service update 服务名称|服务ID 	
```

其他：

```shell
#管理节点，解散集群
docker swarm leave --force
#普通节点：
docker swarm leave

#在任意 Manager 节点中运行 docker info 可以查看当前集群的信息
docker info 
```





## 购买服务器

> 至少选择1核2G的，并且预算不足可以使用按量付费模式

1. 账户余额至少充值100，才能开始使用按量付费模式

2. 创建实例

   ![image-20221220112030308](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201120700.png)

3. 选择

   ```
   1. 选择按量付费
   2. 选择离自己较近的区域（可用区随便选择）
   3. 选择共享型（便宜）
   	3.1 选择1核2G的
   4. 实例数量（由自己选择）
   5. 镜像选择（Centos 版本7）
   ```

4. 下一步

   ```
   1. 带宽计费模式： 选择按使用流量
   2. 带宽峰值： 直接拉满，毕竟是按照按流量模式，这里无所谓的
   3. 安全组：选择自己之前创建的，这样后面的都在一个安全组内，互相ping不走外网不费流量，省钱
   ```

5. 下一步

   选择自定义密码，然后输入登录密码都是一样，方便记忆和登录

6. 确认订单

   ![image-20221220112912069](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201129160.png)

7. ![image-20221220112943046](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201129113.png)

8. 在本地FinalShell进行连接，然后执行相同命令发送到其他会话里

   ![image-20221220113509624](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201135731.png)

9. 都安装docker

   。。。。

10. 相同会话发送位置在这里：

    ![image-20221220114210993](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201142078.png)

## Swarm集群搭建

就两步：

1. 生成主节点init
2. 加入（管理者、worker)



​	在任意节点下通过 `docker swarm init` 命令创建一个新的 Swarm 集群并加入，且该节点会默认成为 Manager 节点。一般任意一台机器上运行该命令即可。通常第一个加入集群的管理节点将成为 `Leader`，后来加入的管理节点都是 `Reachable`。当前的 Leader 如果挂掉，所有的 Reachable 将重新选举一个新的 Leader。

> 创建集群

首先通过`ip addr`获取到linux内网地址： 172.26.221.63

然后创建一个集群并初始化：

![image-20221220141347353](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201413466.png)

> 加入集群

​	Docker 中内置的集群模式自带了公钥基础设施(PKI)系统，使得安全部署容器变得简单。集群中的节点使用传输层安全协议(TLS)对集群中其他节点的通信进行身份验证、授权和加密。



​	默认情况下，通过 `docker swarm init` 命令创建一个新的 Swarm 集群时，Manager 节点会生成新的根证书颁发机构（CA）和密钥对，用于保护与加入群集的其他节点之间的通信安全。



​	Manager 节点会生成两个令牌，供其他节点加入集群时使用：一个 Worker 令牌，一个 Manager 令牌。每个令牌都包括根 CA 证书的摘要和随机生成的密钥。当节点加入群集时，加入的节点使用摘要来验证来自远程管理节点的根 CA 证书。远程管理节点使用密钥来确保加入的节点是批准的节点。

----

Manager:

​	若要向该集群添加 Manager 节点，管理节点先运行 `docker swarm join-token manager` 命令查看管理节点的令牌信息。

![image-20221220142221316](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201422413.png)

表示：要想加入swarm集群，则执行以下这个命令来指示！

![image-20221220142511019](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201425099.png)

运行 `docker swarm join` 并携带令牌参数加入 Swarm 集群，该节点角色为 Manager。

-----

Worker:

​	通过创建集群时返回的结果可以得知，要向这个集群添加一个 Worker 节点，运行下图中的命令即可。或者管理节点先运行 `docker swarm join-token worker` 命令查看工作节点的令牌信息。

![image-20221220142744236](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201427334.png)

然后在其他节点上运行 `docker swarm join` 并携带令牌参数加入 Swarm 集群，该节点角色为 Worker

![image-20221220143245810](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201432890.png)

> 查看集群节点

在任意 Manager 节点中运行 `docker node ls` 可以查看当前集群节点信息。

![image-20221220143522959](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201435048.png)

​	`*` 代表当前节点，现在的环境为 2 个管理节点构成 1 主 1 从，以及 2 个工作节点。 节点 `MANAGER STATUS` 说明：表示节点是属于 Manager 还是 Worker，没有值则属于 Worker 节点。

- `Leader`：该节点是管理节点中的主节点，负责该集群的集群管理和编排决策；
- `Reachable`：该节点是管理节点中的从节点，如果 Leader 节点不可用，该节点有资格被选为新的 Leader；
- `Unavailable`：该管理节点已不能与其他管理节点通信。如果管理节点不可用，应该将新的管理节点加入群集，或者将工作节点升级为管理节点



节点 `AVAILABILITY` 说明：表示调度程序是否可以将任务分配给该节点。

- `Active`：调度程序可以将任务分配给该节点；
- `Pause`：调度程序不会将新任务分配给该节点，但现有任务仍可以运行；
- `Drain`：调度程序不会将新任务分配给该节点，并且会关闭该节点所有现有任务，并将它们调度在可用的节点上

## 节点的删除

> Manager

​	删除节点之前需要先将该节点的 `AVAILABILITY` 改为 `Drain`。其目的是为了将该节点的服务迁移到其他可用节点上，确保服务正常。最好检查一下容器迁移情况，确保这一步已经处理完成再继续往下。

```shell
docker node update --availability drain 节点名称|节点ID
```

然后，将该 Manager 节点进行降级处理，降级为 Worker 节点。

```shell
docker node demote 节点名称|节点ID
```

然后，在已经降级为 Worker 的节点中运行以下命令，离开集群

```shell
docker swarm leave
```

最后，在管理节点中对刚才离开的节点进行删除。

```shell
docker node rm 节点名称|节点ID
```

> Worker

​	删除节点之前需要先将该节点的 `AVAILABILITY` 改为 `Drain`。其目的是为了将该节点的服务迁移到其他可用节点上，确保服务正常。最好检查一下容器迁移情况，确保这一步已经处理完成再继续往下。

```shell
docker node update --availability drain 节点名称|节点ID
```

然后，在准备删除的 Worker 节点中运行以下命令，离开集群

```shell
docker swarm leave
```

最后，在管理节点中对刚才离开的节点进行删除。

```shell
docker node rm 节点名称|节点ID
```

## 弹性服务

​	以前是docker run来启动容器，后来通过docker-compose up 来启动一个项目，但这些都是单机版的！现在在企业中为了高可用变成了集群，那么就是swarm docker-service!

**服务具有动态扩缩容、滚动更新的功能**

### 创建服务

```sh
docker service create --replicas 1 --name mynginx -p 80:80 nginx
```
![image-20221220162031255](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201620355.png)

- `docker service create`：创建服务；
- `--replicas`：指定一个服务有几个实例运行；
- `--name`：服务名称。

浏览器访问:

![image-20221220162422982](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201624061.png)

验证：四台机器，每台无论是主还是从 都可以访问nginx；可以认为集群就是一个整体！也就是说该集群下任意节点的IP地址都能访问到该服务。

> 查看正在运行的服务

可以通过`docker service ls` 查看运行的服务。

![image-20221220162615174](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201626244.png)

注意：仅限于管理节点上操作，工作节点无法进行操作！



> 查看服务运行在哪些节点上

可以通过 `docker service ps 服务名称|服务ID` 查看服务运行在哪些节点上。

![image-20221220163333817](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201633897.png)

在对应的任务节点上运行 `docker ps` 可以查看该服务对应容器的相关信息。

注意：是在对应的任务节点上，这个在创建服务时是任意分配的！其他没有安装的节点上是查询不存在的！

### 弹性服务

​	将 service 部署到集群以后，可以通过命令弹性扩缩容 service 中的容器数量。在 service 中运行的容器被称为 task（任务）。 

1. 通过`docker service scale服务名称|服务ID=n`可以将service运行的任务扩缩容为n个。 
2. 通过`docker service update --replicas n 服务名称|服务ID`也可以达到扩缩容的效果。将mynginx service 运行的任务扩展为5个：

![image-20221220165924255](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201659335.png)

扩容了5台，然后我们查看这5台分配到不同的节点上进行部署！

![image-20221220165855784](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201658885.png)

---

想要运行指定数量的服务，docker swarm实现了动态化的**扩缩**！

### 删除服务

通过 `docker service rm 服务名称|服务ID` 即可删除服务。

```shell
[root@manager1 ~]# docker service rm mynginx
 
mynginx
 
[root@manager1 ~]# docker service ls
ID                NAME              MODE              REPLICAS          IMAGE             PORTS
```

### 容器的滚动更新及回滚

Redis 版本如何滚动升级至更高版本再回滚至上一次的操作。

> 首先创建5个Redis服务副本，版本为 5，详细命令如下：

```shell
docker service create --replicas 5 --name redis \
--update-delay 10s \
--update-parallelism 2 \
--update-failure-action continue \
--rollback-monitor 20s \
--rollback-parallelism 2 \
--rollback-max-failure-ratio 0.2 \
redis:5
```

![image-20221220170919584](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201709694.png)

解释：

​	创建 5 个副本，每次更新 2 个，更新间隔 10s，20% 任务失败继续执行，超出 20% 执行回滚，每次回滚 2 个

- --update-delay：定义滚动更新的时间间隔；
- --update-parallelism：定义并行更新的副本数量，默认为 1；
- --update-failure-action：定义容器启动失败之后所执行的动作；
- --rollback-monitor：定义回滚的监控时间；
- --rollback-parallelism：定义并行回滚的副本数量；
- --rollback-max-failure-ratio：任务失败回滚比率，超过该比率执行回滚操作，0.2 表示 20%。

> 开始滚动更新redis的版本：

```shell
docker service update --image redis:6 redis
```

![image-20221220171604534](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201716681.png)

> 回滚服务，只能回滚到上一次操作的状态，并不能连续回滚到指定操作

```shell
docker service update --rollback redis
```

![image-20221220171747791](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212201717961.png)



## 相关概念解释

> Swarm

集群的管理和编排，docker可初始化一个swarm集群，其他节点可加入（manager、worker）

> Node

就是一个docker节点，多个节点组成了一个网络集群（manager、worker）

> Service

服务、可管理节点或者工作节点运行

![image-20221221094119543](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212210941654.png)

swarm 接收服务并且调度任务给 worker 节点manager：

![image-20221221094419168](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212210944271.png)

manager：

```
1. 接收命令并创建service
2. 循环创建并调度 service
3. 分配 ip 地址给task
4. 分配 task 给节点
5. 命令 worker 运行 task
```

worker node：

```
1. 连接并确认被分配的 task
2. 执行 tasks
```









> Task

容器内的命令

![image-20221221093143749](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212210931849.png)

## 服务副本与全局服务

有两种service的部署模式，**复制与全局**

> 复制（默认）

在设置task任务的时候会根据当前已有的节点进行创建并部署相同副本的http服务，

> 全局

不能指定task的数量，每次增加一个节点，协调器会创建一个task，并有调度器分配到新节点上！

----

![image-20221221095944254](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212210959358.png)

命令：

```shell
--mode
docker service create --mode replicated 默认的  --mode global
场景：
日志收集：每一个节点有自己的日志收集器，过滤，再把所有日志最终传给日志中心。
服务监控
```

理解：

**服务副本**：默认的，可以设置创建副本的数量然后根据当前有多少节点进行分配，如果此时创建新的节点不会进行处理，除非又重新设置创建节点副本才可能会分配task任务。

**全局服务**：不能设置task数量，整个集群有多少个节点就分配多少个task任务，每创建添加一个节点就将任务添加分配！

## 小结

- Docker Swarm 是docker-compose的集群版，采用的是 Raft 协议

  1. 最多容忍 N-1/2个节点 挂掉

- 两种类型的节点

  1. manager：主节点
     - 可查看集群信息、扩缩容、运行服务等
  2. worker：工作节点
     - 仅可运行服务，不允许控制扩缩容等

- service=多个服务=多个node节点

  1. 服务=task+容器

- 服务分为全局服务和服务副本（默认）

  

  

# 13. Docker stack

概念：

​	Docker-Compose的缺点是不能在分布式多机器上使用。此时我们学习了Docker-Swarm但缺点是不能同时编排多个服务，那么此时Docker-Stack的优点就出来了：**可以在分布式多机器上同时编排多个任务**。

  

案例：https://www.jianshu.com/p/d250b91c7adf

相关命令：

![image-20221221100927714](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212211009800.png)

一个stack 是一组相互关联的 service，这组 service共享依赖，可被安排在一起运行和扩展：

```shell
# docker-compose 单机部署
docker-compose up -d
# docker stack 集群部署
# docker stack deploy -c [docker-compose.yml] [service name]
docker stack deploy -c docker-compose.yml mygoweb
docker stack deploy myapps --compose-file=/wordpress.yaml
# 查看所有stack
docker stack ls
```

# 14. Docker Secret

官网描述：Manage Docker secrets 管理 Docker 机密

1. 用户名密码
2. SSH Key
3. TLS认证
4. 任何不想让别人看到的数据

​	我们知道有的service是需要设置密码的，比如mysql服务是需要设置密码的。比如：docker-compose.yml中的service密码都是明文，这样就导致了不是很安全。

![image-20221221101227876](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212211012958.png)

我们知道manager节点保持状态的一致是通过Raft Database这个分布式存储的数据库，它本身就是将信息进行了secret，所以可以利用这个数据库将一些敏感信息，例如账号、密码等信息保存在这里，然后通过给service授权的方式允许它进行访问，这样达到避免密码明文显示的效果。



总之，secret的Swarm中secret的管理通过以下步骤完成：

1. secret存在于Swarm Manager节点的的Raft Database里

2. secret可以assign给一个service，然后这个service就可以看到这个secret
3. 在container内部secret看起来像文件，实际上就是内存



## 创建与使用

1. docker secret的帮助命令：

   ![image-20221221101802190](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212211018296.png)

2. 查看secret的创建命令：

   ![image-20221221102242749](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212211022850.png)

开始：

```shell
#首先先创建一个文件用于存放mysql密码 密码为：root
[root@localhost home]# vim mysql-password
[root@localhost home]# cat mysql-password
root
#创建secret 基于文件创建
#这些都是建立在有一个集群的基础上的。要搭建好docker swarm.
#mysql-pass是secret的名称，mysql-password是我们建立存储密码的文件，这样执行后就相当于将文件中的密码存储在Swarm中manager节点的Raft Database中了
[root@localhost home]# docker secret create mysql-pass
mysql-password
uwrfxavvhpm3opyx7hyvdxkhk
#为了安全起见，现在可以直接将这个文件删掉，因为Swarm中已经有这个密码了。
[root@localhost home]# rm -f mysql-password
#查看secret列表
[root@localhost home]# docker secret ls
ID             NAME     DRIVER  CREATED UPDATED
uwrfxavvhpm3opyx7hyvdxkhk  mysql-pass       About aminute ago  About a minute ago
#创建secret 基于命令创建
#docker secret create [OPTIONS] CONFIG file|-
[root@localhost home]# echo "root" | docker secret create mysql-pass2 -
#查看secret列表
[root@localhost home]# docker secret ls
ID             NAME     DRIVER  CREATED     UPDATED
uwrfxavvhpm3opyx7hyvdxkhk  mysql-pass        2 minutes ago  2 minutes ago
9mis56r456xj55tkvnn3npq42  mysql-pass2       7 seconds ago  7 seconds ago
#查看更多信息
# docker secret inspect mysql-pass2
[
 {
    "ID": "9mis56r456xj55tkvnn3npq42",
    "Version": {
      "Index": 101
   },
    "CreatedAt": "2022-04-01T02:42:51.986190083Z",
    "UpdatedAt": "2022-04-01T02:42:51.986190083Z",
    "Spec": {
      "Name": "mysql-pass2",
      "Labels": {}
   }
 }
]
#删除secret
[root@localhost home]# docker secret rm mysql-pass2
mysql-pass2
[root@localhost home]# docker secret ls
ID             NAME     DRIVER  CREATED UPDATED
uwrfxavvhpm3opyx7hyvdxkhk  mysql-pass       4 minutes ago  4 minutes ago
#Secret在单容器中的使用
#容器中查看secret,启动一个服务后，将其授权给特定的服务使其可以看到
#docker service create -secret mysql-pass(密码名字) 创建服务时可以给服务暴露出secret
#创建服务
[root@localhost home]# docker service create --name demo --secret mysql-pass busybox sh -c "while true; do sleep 3600; done"
l5tc9hplb9kdue0ogu7tzazc2
overall progress: 1 out of 1 tasks
1/1: running 
verify: Service converged
#查看这个服务运行在那个节点上
[root@localhost home]# docker service ls
ID       NAME    MODE     REPLICAS  IMAGE   PORTS
l5tc9hplb9kd  demo    replicated  1/1   busybox:latest j5yu04xqlfe1
[root@localhost home]# docker service ps demo
ID       NAME   IMAGE      NODE       DESIRED STATE  CURRENT STATE     ERROR PORTS
c1b8oz3ppbn6  demo.1  busybox:latest localhost.localdomain  Running     Running about a minute agomy-ngnix  replicated  3/3    nginx:latest *:8888->80/tcp
#可以看到这个服务运行在localhost.localdomain主机的节点上，去这个节点上进入到容器内部，查看secret：
[root@localhost home]# docker ps
CONTAINER ID  IMAGE      COMMAND    CREATED       STATUS       PORTS   NAMES
bd823abfcf67  busybox:latest  "sh -c 'while true; …"  About a minute ago  Up About a minute demo.1.c1b8oz3ppbn685shhvg83egik
[root@localhost home]# docker exec -it bd823abfcf67
/bin/sh
/ # cd /run/secrets/
/run/secrets # ls
mysql-pass
/run/secrets # cat mysql-pass
root
```

## 实战MySQL

```shell
#mysql服务
#关于mysql镜像官网有关于secret的描述：
#作为通过环境变量传递敏感信息的替代方法，_FILE可以将其附加到先前列出的环境变量中，从而使初始化脚本从容器中存在的文件中加载那些变量的值。
#特别是，这可用于从/run/secrets/<secret_name>文件中存储的DockerSecret加载密码。例如：
#$ docker run --name some-mysql -e MYSQL_ROOT_PASSWORD_FILE=/run/secrets/mysql-root -d ysql:tag
#目前，这仅支持MYSQL_ROOT_PASSWORD，MYSQL_ROOT_HOST，
MYSQL_DATABASE，MYSQL_USER，和MYSQL_PASSWORD。
#所以我们需要先创建一个文件secret用于存储数据库的敏感信息，因为之前已
经创建过，这里无需再创建：
#再次查看secret 命令多打打才不容易忘记
[root@localhost home]# docker secret ls
ID             NAME     DRIVER  CREATED PDATED
uwrfxavvhpm3opyx7hyvdxkhk  mysql-pass       4 inutes ago  4 minutes ago
#启动mysql服务：
[root@localhost home]# docker service create --name db-mysql --secret mysql-pass -e
MYSQL_ROOT_PASSWORD_FILE=/run/secrets/mysql-pass mysql:5.7
w1ptd75g4locdme9zct7opiy2
overall progress: 0 out of 1 tasks
1/1: preparing
^COperation continuing in background.
Use `docker service ps w1ptd75g4locdme9zct7opiy2` to check
progress.
[root@localhost home]# docker service ps
w1ptd75g4locdme9zct7opiy2
ID       NAME     IMAGE    NODE    DESIRED STATE  CURRENT STATE       ERROR  PORTS
fv4arc2gq6y8  db-mysql.1  mysql:5.7 
localhost.localdomain  Running     Preparing 3
minutes ago
#查看mysql服务在那个节点上：
[root@localhost ~]# docker ps
CONTAINER ID  IMAGE     COMMAND   CREATED     STATUS     PORTS    NAMES
Secret在Stack中的使用
Stack利用的就是docker-compose.yml文件来部署stack，那么如何在
docker-compose.yml中来定义secret呢？
55feeb983b81  mysql:5.7    "docker-entrypoint.s…"  2
minutes ago  Up 2 minutes   3306/tcp, 33060/tcp  db-
mysql.1.fv4arc2gq6y8g7lr1m4c7sefn
#在该节点中进入该服务的容器中查看secret：
[root@localhost ~]# docker exec -it 55feeb983b81 /bin/sh
# cd /run/secrets/
# ls
mysql-pass
# cat mysql-pass
root
#这样知道了密码就可以进入到mysql数据库中了。
# mysql -uroot -p
Enter password:
Welcome to the MySQL monitor. Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.36 MySQL Community Server (GPL)
Copyright (c) 2000, 2021, Oracle and/or its affiliates.
Oracle is a registered trademark of Oracle Corporation
and/or its
affiliates. Other names may be trademarks of their
respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the
current input statement.
mysql>
```

## Secret在Stack中的使用

Stack利用的就是docker-compose.yml文件来部署stack，那么如何在docker-compose.yml中来定义secret呢？

```yaml
mysql:
 image: mysql
 secrets:
   - my-pw
 environment:
  MYSQL_ROOT_PASSWORD_FILE: /run/secrets/mysql-pass #
制定secret
  MYSQL_DATABASE: wordpress
```

显然我们在运行这个docker-compose.yml文件之前必须先要进行对应的secret文件的创建。然后就可以通过docker stack deploy命令来部署这个stack了。



部分参考 https://www.cnblogs.com/shenjianping/p/12272847.html

# 15. Docker Config

在集群环境中配置文件的分发，可以通过将配置文件放入镜像中、设置环境变量、挂载volume、挂载目录的方式，当然也可以通过 docker config来**管理集群中的配置文件**，这样的方式也更加通用。

## 基本命令

```shell
#一切命令从help学起
[root@localhost home]# docker config --help
Usage:  docker config COMMAND

Manage Docker configs

Commands:
  create      Create a config from a file or STDIN
  inspect     Display detailed information on one or more configs
  ls          List configs
  rm          Remove one or more configs

Run 'docker config COMMAND --help' for more information on a command.

[root@localhost home]# docker config create --help
Usage:  docker config create [OPTIONS] CONFIG file|-

Create a config from a file or STDIN
#可以看出创建可以分为 文件创建，命令创建
Options:
  -l, --label list               Config labels
      --template-driver string   Template driver

  
#文件创建   
[root@localhost home]# vim default.conf
[root@localhost home]# cat default.conf
server {
 listen    88;
 server_name localhost;
 location / {
   root  /usr/share/nginx/html;
   index index.html index.htm;
 }
}
#创建config
[root@localhost home]# docker config create conf
default.conf
ouyt1mlylntrzyd4qw74f5qx6
#查看config
[root@localhost home]# docker config ls
ID             NAME   CREATED    UPDATED
ouyt1mlylntrzyd4qw74f5qx6  conf    19 seconds ago  19 seconds ago
#从标准输入创建
#Usage: docker config create [OPTIONS] CONFIG file|-
[root@localhost home]# echo "listen 80" | docker config
create conf2 -
[root@localhost home]# docker config ls
ID             NAME   CREATED    UPDATED
ouyt1mlylntrzyd4qw74f5qx6  conf    7 minutes ago   7 minutes ago
7eqao1yfzpjqfk4zhh35o7osj  conf2   34 seconds ago  34 seconds ago
#查看config详细信息
[root@localhost home]# docker config inspect conf
[
 {
    "ID": "ouyt1mlylntrzyd4qw74f5qx6",
    "Version": {
      "Index": 117
   },
    "CreatedAt": "2022-04-01T03:29:25.097432457Z",
    "UpdatedAt": "2022-04-01T03:29:25.097432457Z",
    "Spec": {
      "Name": "conf",
      "Labels": {},
      "Data":
"c2VydmVyIHsKICAgIGxpc3RlbiAgICAgICA4ODsKICAgIHNlcnZlcl9uY
W1lICBsb2NhbGhvc3Q7CgogICAgbG9jYXRpb24gLyB7CiAgICAgICAgcm9
vdCAgIC91c3Ivc2hhcmUvbmdpbngvaHRtbDsKICAgICAgICBpbmRleCAga
W5kZXguaHRtbCBpbmRleC5odG07CiAgICB9Cn0K"
   }
 }
]
#对conf进行base64解码
[root@localhost home]# docker config inspect -f '{{json.Spec.Data}}' conf | cut -d '"' -f2 | base64 -d
#出现结果
server {
 listen    88;
 server_name localhost;
 location / {
   root  /usr/share/nginx/html;
   index index.html index.htm;
 }
}
#删除secret
[root@localhost home]# docker config rm conf2
```

## 使用

```shell
#docker config
#使用nginx镜像创建容器
#在conf配置中，将nginx的监听端口改成了88 --config source=conf，
#然后再替换掉nginx中的默认80端口的配置文件target=/etc/nginx/conf.d/default.conf，
#创建service时，将容器内部端口88端口映射成主机上90端口，-p 90:88
#后台运行。-d
[root@localhost ~]# docker service create --name nginx-01--config source=conf,target=/etc/nginx/conf.d/default.conf -p 90:88 nginx:latest
yuu4hnqk2masmlr11v4tya83y
overall progress: 1 out of 1 tasks
1/1: running 
verify: Service converged
```

测试得出：

```html
[root@localhost ~]# curl http://127.0.0.1:90
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is
successfully installed and
working. Further configuration is required.</p>
<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>
<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

# 16. CI/CD (流水线)

![image-20221221104530630](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212211045902.png)

CI/CD是实现敏捷和Devops理念的一种方法。
具体而言，CI/CD 可让持续自动化和持续监控贯穿于应用的整个生命周期（从集成和测试阶段，到交付和部署）。这些关联的事务通常被统称为“CI/CD 管道”，由开发和运维团队以敏捷方式协同支持。

## CI-持续集成

> 概念：

持续集成指的是，频繁地（一天多次）将代码集成到主干。



​	CI是一种通过在应用开发阶段引入**自动化**来频繁向客户交付应用的方法。CI的核心概念是持续集成、持续交付和持续部署。作为一个面向开发和运营团队的解决方案，CI 主要针对在集成新代码时所引发的问题（亦称“集成地狱”）。
​	持续集成强调开发人员提交了新代码之后，**立刻自动的进行构建**、（单元）测试。根据测试结果，我们可以确定新代码和原有代码能否正确地集成在一起。

> 持续集成的目的：

​	持续集成的目的就是让产品可以快速迭代，同时还能保持高质量。它的核心措施是，代码集成到主干之前，必须通过自动化测试。只要有一个测试用例失败，就不能集成。
​	持续集成过程中很重视自动化测试验证结果，对可能出现的一些问题进行预警，以保障最终合并的代码没有问题。

> 持续集成的作用：

1. 代码库存越是积压，就越得不到生产检验，积压越多，代码间交叉感染的概率越大，下个发布（release）的复杂度和风险越高，持续集成可以保证团队开发人员提交代码的质量，减轻了软件发布时的压力；

2. 持续集成中的任何一个环节都是自动完成的，无需太多的人工干预，有利于减少重复过程以节省时间、费用和工作量；

3. 及早的发现代码中的问题，及早解决，代码越早推送（PUSH）出去，
   用户能越早用到，快就是商业价值；

> 特点
>

1. 它是一个自动化的周期性的集成测试过程，从检出代码、编译构建、运行测试、结果记录、测试统计等都是自动完成的，无需人工干预；
2. 需要有专门的集成服务器来执行集成构建；
3. 需要有代码托管工具支持；

## CD-持续交付

![image-20221221105159915](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212211052148.png)

> CD与DevOps关系

持续交付与DevOps的含义很相似，所以经常被混淆。但是它们是两个不同的概念：

**DevOps**：

​	范围更广，它以文化变迁为中心，特别是软件交付过程所设计的多个团队之间的合作。一种思想。

**CD**：

​	持续交付，是一种自动化交付的手短，关注点在于将不同的过程集中起来，并且更快，更频繁的执行这些过程。
