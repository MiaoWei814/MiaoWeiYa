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





如何提交一个自己的镜像？

## commit镜像

```shell
docker commit 提交容器成为一个新的副本

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

![image-20200611172701729](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211301734327.png)

```shell
如果你想要保存当前容器的状态，就可以通过commit来提交，获得一个镜像，就好比我们我们使用虚拟机的快照。
```

到了这里就算是入门Docker了！

