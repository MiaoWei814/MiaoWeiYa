

Linux 一切皆文件：文件就 读、写、 （权限）


# 1. 环境搭建

Linux 的安装，安装步骤比较繁琐（操作系统本身也是一个软件），现在其实云服务器挺普遍的，价格也便宜，如果直接不想搭建，也可以直接买一台学习用用！

## 1.1安装CentOS 

在本地安装，这个不太建议，如果没有经济来源的话，可以考虑在本地搭建

Linux是一个操作系统，你也可以把自己电脑安装成双系统！

虚拟机（VMware下载（收费的，注册码！）： 360 一键安装（推荐，小白分分钟安装成功！）！）

![image-20221118105447635](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211181054844.png)

1.官网下载：

![image-20221118105521707](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211181055795.png)

安装完成后打开软件有如下界面：

![image-20221118105529553](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211181055634.png)

```bash
在虚拟机上安装CentOS
```

1 、可以通过镜像进行安装！ 下载地址：http://mirrors.aliyun.com/centos/7/isos/x86_64/，下载完成后安装即可！安装操作系统和安装软件是一样的，注意：Linux磁盘分区的时候需要注意分区名即可！
**/boot /home!**

2 、可以使用我已经制作好的镜像！（狂神制作好的一个自己的镜像！）使用百度云链接下载即可！

3 、安装 VMware 虚拟机软件，然后打开我们的镜像即可使用！

打开后的样子：

![image-20221118105600709](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211181056785.png)

**VMware的使用方式：**

点击屏幕进入虚拟机、Ctrl + Alt 将聚焦退出虚拟机！

开机后的页面：

```
链接：https://pan.baidu.com/s/1e0YEzN3BW0DDXFtMscvvVA
提取码：716m
复制这段内容后打开百度网盘手机App，操作更方便哦
```

![image-20221118105620948](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211181056094.png)

默认的用户是：kuangshen 密码是 123456

root用户：root-

大家下载完毕后，可以安装系统，然后根据我们的账号密码，来自己测试登录！

那我们的本地Linux环境就准备好了！

## 1.2 云服务器


> 购买云服务器（有经济来源的话，可以购买阿里云服务器，因为这才是最近接公司中原生环境的！）

云服务器就是一个远程电脑，服务器一般不会关机！

虚拟机安装后占用空间，也会有些卡顿，我们作为程序员其实可以选择购买一台自己的服务器，这样的话更加接近真实线上工作；

1. 阿里云购买服务器：https://www.aliyun.com/minisite/goods?userCode=0phtycgr

2. 购买完毕后，获取服务器的ip地址，重置服务器密码，就可以远程登录了

   -  获取公网IP地址！
   -  修改自己的登录密码

   - ![image-20221118110245377](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211181102451.png)

   - 设置安全组： （在阿里云这个很重要，自己需要开放什么端口来这里配置就好）

     ![image-20221118110408400](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211181104494.png)

     ![image-20221118110417200](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211181104283.png)

3 、下载 xShell 远程连接工具 22 ，进行远程连接使用！建议使用 360 一键下载！还需要下载一个 xFtp 文件上传 21 ！

4 、使用Xshell连接远程服务器！~

![image-20221118110454362](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211181104547.png)

这里就是我们的Linux操作系统了！以后的操作都在这里操作，项目也在这里进行发布！

![image-20221118110519423](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211181105505.png)

> Ctrl + 鼠标滚轮，放大和缩小字体！

上传文件使用xftp即可！

![image-20221118110554831](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211181105917.png)


Tomcat 需要 Java环境！我们后会按照JDK，到时候再测试吧，先学基本的Linux命令为主！

远程环境安装！

**注意事项：**

如果要打开端口，需要在阿里云的安全组面板中开启对应的出入规则，不然的话会被阿里拦截！

## 1.3宝塔面板

傻瓜式管理服务器

安装教程：https://www.bt.cn/bbs/thread-19376-1-1.html

1 、开启对应的端口

2 、一键安装

3 、安装完毕后会得到远程面板的地址，账号，密码，就可以登录了

4 、登录之后就可以可视化的安装环境和部署网站！

## 1.4 关于域名

如果自己的网站想要上线，就一定要购买一个域名然后进行备案；

备案的话需要一些认证和时间，备完完毕后，就可以解析到自己的网站了，这个时候就可以使用域名来进行服务器的访问！



# 2.走近Linux系统

1. 开机登录
开机会启动许多程序。它们在Windows叫做"服务"（service），在Linux就叫做"守护进程"（daemon）。
2. 开机成功后，
    它会显示一个文本登录界面，这个界面就是我们经常看到的登录界面，在这个登录界面中会提示用户输入用户名，而用户输入的用户将作为参数传给login程序来验证用户的身份，密码是不显示的，输完回车即可！

一般来说，用户的登录方式有三种：

- 命令行登录
- ssh登录
- 图形界面登录

**最高权限账户为 root，可以操作一切！**

3. 关机
在linux领域内大多用在服务器上，很少遇到关机的操作。毕竟服务器上跑一个服务是永无止境的，除非特殊情况下，不得已才会关机。

## 2.1 关机命令

> 关机指令为：`shutdown` ；
>

```basic
sync # 将数据由内存同步到硬盘中。
```
```basic
shutdown # 关机指令，你可以man shutdown 来看一下帮助文档。例如你可以运行如下命令关机：
```
```basic
shutdown –h 10 # 这个命令告诉大家，计算机将在 10 分钟后关机
```
```basic
shutdown –h now # 立马关机
```
```basic
shutdown –h 20 :25 # 系统会在今天20:25关机
```
```basic
shutdown –h + 10 # 十分钟后关机
```
```basic
shutdown –r now # 系统立马重启
```
```basic
shutdown –r + 10 # 系统十分钟后重启
```
```basic
reboot # 就是重启，等同于 shutdown –r now
```
```basic
halt # 关闭系统，等同于shutdown –h now 和 poweroff
```

最后总结一下，不管是重启系统还是关闭系统，首先要运行 **sync** 命令，把内存中的数据写到磁盘中。

## 2.2 系统目录结构

在Linux中一切皆文件

`根目录 / ，所有的文件都挂载在这个节点下`

登录系统后，在当前命令窗口下输入命令：

```bash
ls / # 查看根目录下所有的文件
```

你会看到如下图所示：

![image-20221118112030930](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211181120985.png)

树状目录结构：

![image-20221118112048152](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211181120217.png)

以下是对这些目录的解释：


1. /bin ： bin是Binary的缩写, 这个目录存放着最经常使用的命令。
2. /boot： 这里存放的是启动Linux时使用的一些核心文件，包括一些连接文件以及镜像文件。（不要动）
3. /dev ： dev是Device(设备)的缩写, 存放的是Linux的外部设备，在Linux中访问设备的方式和访问文件的方式是相同的。
4. **/etc： 这个目录用来存放所有的系统管理所需要的配置文件和子目录。**
5. **/home ：用户的主目录，在Linux中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的。**
6. /lib ： 这个目录里存放着系统最基本的动态连接共享库，其作用类似于Windows里的DLL文件。（不要动）
7. /lost+found ： 这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。（存放突然关机的一些文件）
8. /media ：linux系统会自动识别一些设备，例如U盘、光驱等等，当识别后，linux会把识别的设备挂载到这个目录下。
9. /mnt ：系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在/mnt/上，然后进入该目录就可以查看光驱里的内容了。（我们后面会把一些本地文件挂载在这个目录下）
10. **/opt ：这是给主机额外安装软件所摆放的目录。比如你安装一个ORACLE数据库则就可以放到这个目录下。默认是空的。**
11. /proc ： 这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。（不用管）
12. **/root ：该目录为系统管理员，也称作超级权限者的用户主目录。--类似于Windows中的C盘**
13. /sbin ：s就是Super User的意思，这里存放的是系统管理员使用的系统管理程序。
14. /srv ：该目录存放一些服务启动之后需要提取的数据。
15. /sys ：这是linux2.6内核的一个很大的变化。该目录下安装了2.6内核中新出现的一个文件系统
sysfs 。
15. **/tmp ：这个目录是用来存放一些临时文件的。用完即丢的文件，可以放在这个目录下，安装包！**


16. **/usr：这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于windows下的program files目录。--程序安装目录**
17. /usr/bin： 系统用户使用的应用程序。
18. /usr/sbin： 超级用户使用的比较高级的管理程序和系统守护程序。Super
19. /usr/src： 内核源代码默认的放置目录。
20. **/var ：这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。**
21. /run ：是一个临时文件系统，存储系统启动以来的信息。当系统重启时，这个目录下的文件应该被删掉或清除。
22. **/www：存放服务器网站相关的资源，环境，网站的项目**



# 3. 常用的基本命令

你可以使用 _man [_ 命令 _]_ 来查看各个命令的使用文档，如 ：man cp。

tab: 补全文件名或者命令

curl: 类似于在地址栏访问网络地址然后下载打印在控制台

## 3.1 cd 命令

格式：`cd [相对路径、绝对路径、特殊符号]`

- 不加参数时，默认切换到用户主目录
- 接绝对路径或相对路径，切换到对应目录
  - `..`返回上级目录；
  - `..`/`..`返回上两级目录；
  - `./` 代表当前目录
  - `../`返回上一级目录
- 接特殊符号，进入到对应表示目录
  - `~`进入用户主目录
  - `-`返回进入此目录之前所在的目录；
  - `!$`把上个命令的参数作为cd参数使用。

```basic
cd ： 进入用户主目录 
```

```basic
cd /xx/xx/ ： 开头加/斜杠表示从绝对路径根目录下开始 不加则从当前相对路径下开始找
cd ../1d/  ： 先返回上级目录然后去找1d这个文件夹
```

```basic
cd .. ： 返回上一级目录
```

```basic
cd !$	：这个把上一次执行的命令作为cd参数来执行
```

cd 目录名（绝对路径都是以 / 开头，相对路径，对于当前目录该如何寻找 ../../）

注意：如果在文件下存在文件和目录相同的名称，那么可以在命令结尾加可以加`/`标识符，有` /` 直接表明他是目录，没有末尾的` / `，那么 系统会 需要检测一下确定是目录还是文件



## 3.2 ls 查看文件

在Linux中` ls` 可能是最常常被使用的!

`-a`参数：all ，查看全部的文件，包括隐藏文件

`-l `参数 列出所有的文件，包含文件的属性和权限，没有隐藏文件

所有Linux可以组合使用！


```basic
cd 命令 切换目录
ls -al # 查看所有文件包括隐藏文件，以详细信息方式列出所有文件信息
ls -ll # 查看可视的文件-可替代命令 ll
ls -li # 带索引号查看文件详细信息
ls 目录名 # 快速查看这个目录下的文件
```
这里只是列出常用的，具体详细的`ls`的可查看文档：

https://blog.csdn.net/qq_45988641/article/details/116659868

![image-20220929172956596](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211181325040.png)

![image-20220929173047279](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211181325500.png)

## 3.3 文件夹的操作命令

> pwd 显示当前操作所在的目录

```basic
pwd #显示当前用户所在的目录！
```

> mkdir 创建目录

```basic
mkdir xx #创建一个目录
mkdir -p test2/tet3 # 创建递归多级目录-结构上是 目录名/下级目录名
```
> rmdir 删除目录

```basic
rmdir xx #删除目录
rmdir -p xx/xx # 递归删除多级目录。注意文件夹要存在不然会报错
注意：仅能删除空的目录，如果这个目录下存在文件那么就需要先删除文件再删除目录
```

> cp (复制文件或者目录)

```basic
cp xx xxx # 复制 原来的地方 新的地方！-文件
cp -r xx xxx # 复制 原来的地方 新的地方 -文件夹目录
注意：如果复制新的地方如果重复则就会提示覆盖？即输入y或者n就行
```
示例：

```
[root@miaowei miaowei]# ls
test1  test2  test3.txt  test.txt
[root@miaowei miaowei]# cd test1
[root@miaowei test1]# cp ../test.txt ../test1	# 复制文件到指定地方
[root@miaowei test1]# ls
test.txt
[root@miaowei test1]# cp ../test.txt ../test1
cp: overwrite ‘../test1/test.txt’? y			# 重复复制，提示是否覆盖提示
[root@miaowei test1]# ls
test.txt
[root@miaowei test1]# cd ..
[root@miaowei miaowei]# ls test2
test
[root@miaowei miaowei]# cp -r test2 test1		# 复制文件夹到指定地方
[root@miaowei miaowei]# cd test1
[root@miaowei test1]# ls
test2  test.txt
```

> rm （移除文件或者目录！）


```basic
rm -rf / # 系统中的所有文件就被删除了，这个操作是非常危险的-递归强制删除根目录下所有的文件和目录 rf后面可以跟多个文件夹或文件也会一并删除
```
- `-f `忽略不存在的文件，不会出现警告，强制删除！

- `-r` 递归删除目录！

- `-i` 互动，删除询问是否删除

> mv  移动文件或者目录！重命名文件

```basic
mv file1 file2: # 如果file2不存在，修改file1名字为file2，如果file2已存在，把file1的内容覆盖file2，删除file1
mv file1 file2 dir1: # 把file1和file2移动到dir1里
mv dir1 dir2: # 如果dir2不存在，相当于把dir1改名。如果dir2存在，把dir1放到dir2中，相当于下一层目录。
mv dir1/* dir2/: # 把dir1中的文件移动到dir2中（同名文件覆盖）
```
- `-f` 强制
- `-i`提示是否确定操作

- `-u` 只替换已经更新过的文件

## 3.4 查找文件

> find: 查找文件

```basic
find / -name "java"
```

![image-20221123093807068](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211230938324.png)

> whereis: 查找命令

```basic
whereis java
```

![image-20221123093946542](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211230939645.png)





# 4. 文件的基本属性

Linux系统是一种典型的多用户系统，不同的用户处于不同的地位，拥有不同的权限。

为了保护系统的安全性，Linux系统对不同的用户访问同一文件（包括目录文件）的权限做了不同的规定。

## 4.1 属性介绍

Linux系统按`文件所属者`、`文件所属者同组用户`和`其他用户`来规定了不同的文件访问权限。

在Linux中我们可以使用` ll `或者` ls –l `命令来**显示一个文件的属性以及文件所属的用户和组**，
![image-20221118150415847](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211181504921.png)

实例中boot文件的第一个属性用"d"表示。"d"在Linux中代表该文件是一个目录文件。

在Linux中第一个字符代表这个文件是目录、文件或链接文件等等：

```basic
当为[ d ]则是目录
当为[ - ]则是文件；
若是[ l ]则表示为链接文档 ( link file )；后面有->表示指向的位置
若是[ b ]则表示为装置文件里面的可供储存的接口设备 ( 可随机存取装置 )；
若是[ c ]则表示为装置文件里面的串行端口设备，例如键盘、鼠标 ( 一次性读取装置 )。
```
接下来的字符中，以三个为一组，且均为『`rwx`』 的三个参数的组合。

其中，`[ r ]代表可读(read)、[ w ]代表可写(write)、[ x ]代表可执行(execute)。`

要注意的是，这三个权限的位置不会改变，如果`没有权限，就会出现减号[ - ]而已。`

每个文件的属性由左边第一部分的 10 个字符来确定（如下图）：

![image-20221118150713212](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211181507294.png)



从左至右用0-9这些数字来表示。
`第 0 位确定文件类型，第1-3位确定属主（该文件的所属者）拥有该文件的权限。第4-6位确定属组（所属者的同组用户）拥有该文件的权限，第7-9位确定其他用户拥有该文件的权限。`
其中：
第 1 、 4 、 7 位表示读权限，如果用"r"字符表示，则有读权限，如果用"-"字符表示，则没有读权限；

第 2 、 5 、 8 位表示写权限，如果用"w"字符表示，则有写权限，如果用"-"字符表示没有写权限；

第 3 、 6 、 9 位表示可执行权限，如果用"x"字符表示，则有执行权限，如果用"-"字符表示，则没有执行权限。



对于文件来说，它都有一个特定的所有属者，也就是对该文件具有所属权的用户。

同时，**在Linux系统中，用户是按组分类的，一个用户属于一个或多个组**。

- 文件所属者以外的用户又可以分为文件所属者的同组用户和其他用户。

在以上实例中，boot 文件是一个目录文件，属主和属组都为 root。

![img](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211181553642.png)

```bash
文件的属性信息(1-8)：
1. 文件数据的 inode 信息：index node -- 索引节点
    作用：快速从磁盘 block 中找到数据信息
2. 文件的数据类型：文件 目录 链接文件 设备文件 ……
    作用：指明文件的数据类型
3. 权限位信息： r(read)读  w(write)写  x(execute)执行
    作用：控制让不同用户对文件可以有不同的权限
4. 文件目录的硬链接数：类似于超市的多个门
    作用：可以多个路径查看数据信息
    理解：从前面索引角度去看，它就有具体数量的入口来连接它
5. 文件所属用户信息(属主)：
    作用：文件的创建者或者拥有者
6. 文件所属组信息(属组)：
    作用：文件或数据的所属用户组
7. 文件的大小信息
8. 文件的时间信息(修改时间)
9. 文件名称
```



## 4.2 修改文件属性命令

**1 、chgrp：更改文件属组(change group)**

```basic
chgrp [-R]  属组名 文件名
```

-R：递归更改文件属组，就是在更改某个目录文件的属组时，如果加上-R的参数，那么该目录下的所有文件的属组都会更改。

**2 、chown：更改文件属主，也可以同时更改文件属组(change owner)**

```basic
chown [–R] 属主名 文件名
chown [-R] 属主名：属组名 文件名
```

**3 、chmod：更改文件 9 个属性(change mode)**

Linux文件属性有两种设置方法，一种是数字（常用的是数字），一种是符号。

工作中很多时候你没有权限操作此文件！那么这个命令就是用于来`修改文件的权限`的！

```basic
chmod [-R] xyz 文件或目录
```

Linux文件的基本权限就有九个，分别是`owner/group/others`三种身份各有自己的`read/write/execute`权限。

文件的权限字符为：『-rwxrwxrwx』， 这九个权限是三个三个一组的！其中，我们可以使用数字来代表各个权限，各权限的分数对照表如下：

```basic
r:4   w:2     x:1  # 权限字符后的固定数字

可读可写不可执行  rw-  6
可读可写不课执行  rwx  7
chomd 777 文件赋予所有用户可读可执行！
```

每种身份(owner/group/others)各自的三个权限(r/w/x)分数是需要累加的，例如当权限为：[-rwxrwx---]
分数则是：

```
owner = rwx = 4+2+1 = 7
group = rwx = 4+2+1 = 7
others= --- = 0+0+0 = 0
```
```bash
chmod 770 filename
```

![image-20221118154648627](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211181546690.png)



## 4.3 文件内容查看

我们会经常使用到文件查看内容！

Linux系统中使用以下命令来查看文件的内容：

```basic
cat # 由第一行开始显示文件内容，用来读文章，或者读取配置文件啊，都使用cat名
```

```basic
tac # 从最后一行开始显示，可以看出 tac 是 cat 的倒着写！
```

```basic
nl # 显示的时候，顺道输出行号！ 看代码的时候，希望显示行号！ 常用
```
```basic
more # 一页一页的显示文件内容，带余下内容的（空格代表翻页，enter 代表向下看一行， :f 行号）
```

```basic
less # 与 more 类似，但是比 more 更好的是，他可以往前翻页！ （空格下翻页，pageDown，pageUp键代表翻动页面！退出 q 命令，查找字符串 /要查询的字符向下查询，向上查询使用？要查询的字符串，n 继续搜寻下一个，N 上寻找！）
```

```basic
head 只看头几行 通过 -n 参数来控制显示几行！比如：head -n 20 fileName
```


```basic
tail 只看尾巴几行 -n 参数 要查看几行！
```


## 4.4 网络配置目录

>  cd /etc/sysconfig/network-scripts            在Centos才是这样的

![image-20221118162845451](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211181628528.png)

ifconfig 命令查看网络配置！

![image-20221118162902542](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211181629609.png)

如果想ping某个网站是否通畅存在：

![image-20221120130405258](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201304384.png)

## 4.5 软硬链接-创建文件

Linux的链接分为两种：硬链接、软链接！

**硬链接：** A---B，假设B是A的硬链接，那么他们两个的`索引下标地址指向了同一个文件`！允许`一个文件拥有多个路径链接`，用户可以通过这种机制建立硬链接到一些重要文件上，防止误删！

**软链接：** 类似Window下的快捷方式，删除的源文件，快捷方式也访问不了！



硬链接：

```
	硬连接指通过索引节点来进行连接。在 Linux 的文件系统中，保存在磁盘分区中的文件不管是什么类型都给它分配一个编号，称为索引节点号(Inode Index)。在 Linux 中，多个文件名指向同一索引节点是存在的。比如：A 是 B 的硬链接（A 和 B 都是文件名），则 A 的目录项中的 inode 节点号与 B 的目录项中的 inode 节点号相同，即一个 inode 节点对应两个不同的文件名，两个文件名指向同一个文件，A 和 B 对文件系统来说是完全平等的。删除其中任何一个都不会影响另外一个的访问。
	硬连接的作用是允许一个文件拥有多个有效路径名，这样用户就可以建立硬连接到重要文件，以防止“误删”的功能。其原因如上所述，因为对应该目录的索引节点有一个以上的连接。只删除一个连接并不影响索引节点本身和其它的连接，只有当最后一个连接被删除后，文件的数据块及目录的连接才会被释放。也就是说，文件真正删除的条件是与之相关的所有硬连接文件均被删
```

软链接：

```
	软链接文件有类似于 Windows 的快捷方式。它实际上是一个特殊的文件。在符号连接中，文件实际上是一个文本文件，其中包含的有另一文件的位置信息。比如：A 是 B 的软链接（A 和 B 都是文件名），A 的目录项中的 inode 节点号与 B 的目录项中的 inode 节点号不相同，A 和 B 指向的是两个不同的 inode，继而指向两块不同的数据块。但是 A 的数据块中存放的只是 B 的路径名（可以根据这个找到 B 的目录项）。A 和 B 之间是“主从”关系，如果 B 被删除了，A 仍然存在（因为两个是不同的文件），但指向的是一个无效的链接。
```

命令：

```basic
ln file1 file2 # 创建连接 ln 命令！file2为file1的硬链接
```

```basic
touch fileName #命令创建文件！
```
```basic
echo "文件内容" >> file #输入字符串,也可以输入到文件中！
```


```bash
[root@miaowei home]# touch f1		# 创建一个f1文件
[root@miaowei home]# ls
f1  miaowei  redis  www
[root@miaowei home]# ln f1 f2		# 创建一个硬链接 f2 
[root@miaowei home]# ln -s f1 f3	# 创建一个软链接（符号连接） f3
[root@miaowei home]# ll
total 12
-rw-r--r-- 2 root  root     0 Nov 18 17:13 f1
-rw-r--r-- 2 root  root     0 Nov 18 17:13 f2
lrwxrwxrwx 1 root  root     2 Nov 18 17:13 f3 -> f1
drwxr-xr-x 4 root  root  4096 Nov 18 16:01 miaowei
drwx------ 2 redis redis 4096 Oct  6 17:30 redis
drwxrwxrwx 2 www   www   4096 Sep 28 16:08 www
[root@miaowei home]# ls -li
total 12
926186 -rw-r--r-- 2 root  root     0 Nov 18 17:13 f1		# 可以发现f1的索引下标地址为926186
926186 -rw-r--r-- 2 root  root     0 Nov 18 17:13 f2		# 跟f1建立硬链接使f1跟f2两者共享同一个地址，类似于直接复制了一份文件
926188 lrwxrwxrwx 1 root  root     2 Nov 18 17:13 f3 -> f1  # 建立软链接，意思是创建了一个文件而文件内是f1的地址路径，两者索引下标不同
919784 drwxr-xr-x 4 root  root  4096 Nov 18 16:01 miaowei
926165 drwx------ 2 redis redis 4096 Oct  6 17:30 redis
924466 drwxrwxrwx 2 www   www   4096 Sep 28 16:08 www

[root@miaowei home]# echo "i like miaowei" >>f1 	 # 给f1文件中写入一些字符串！
[root@miaowei home]# ls
f1  f2  f3  miaowei  redis  www
[root@miaowei home]# cat f1							# 查看f1文件
i like miaowei
[root@miaowei home]# cat f2							# 查看f2文件
i like miaowei
[root@miaowei home]# cat f3							# 查看f3文件
i like miaowei
```

删除f1之后，查看f2 和 f3 的区别

```basic
[root@miaowei home]# ls
f1  f2  f3  miaowei  redis  www
[root@miaowei home]# rm f1				 #删除f1文件
rm: remove regular file ‘f1’? y
[root@miaowei home]# ls
f2  f3  miaowei  redis  www
[root@miaowei home]# cat f2			# 此时查看f2文件会发现其内容还存在，是因为删除f1文件只是假文件被删除了但是其索引地址还没删除，真正文件还存在
i like miaowei
[root@miaowei home]# cat f3			# f3文件就查看失败，因为虽然是独立的文件，但其指向了不存在的f1文件所以这里查看失败！造成链接失效！
cat: f3: No such file or directory
[root@miaowei home]# 
```



## 4.6 文件打包和解压

> 打包

```basic
tar -czvf tom.tar.gz tom  #把 tom 打包成tom.tar.gz
```

> 解包

```basic
tar -xzvf tom.tar.gz -C /usr/local #把tom.tar.gz解压到/usr/local
```

参数：

```
-c	创建一个新的tar文件           
-t	参看压缩文件内容
-v	显示运行过程信息	
-j	调用bzip2压缩命令执行压缩
-f	指定文件名称	      
-C 指定需要解压到的目录
-z	调用gzip压缩命令执行压缩   	
-x	解开tar文件
```



# 5. Vim 编辑器

## 5.1 什么是Vim编辑器

vim 通过一些插件可以实现和IDE一样的功能！

Vim是从 vi 发展出来的一个文本编辑器。代码补完、编译及错误跳转等方便编程的功能特别丰富，在程序员中被广泛使用。

`尤其是Linux中，必须要会使用Vim（查看内容，编辑内容，保存内容！）`

简单的来说， vi 是老式的字处理器，不过功能已经很齐全了，但是还是有可以进步的地方。

vim 则可以说是程序开发者的一项很好用的工具。

所有的 Unix Like 系统都会内建 vi 文书编辑器，其他的文书编辑器则不一定会存在。

连 vim 的官方网站 (http://www.vim.org) 自己也说 vim 是一个程序开发工具而不是文字处理软件。

vim 键盘图：

![image-20221119152736289](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211191527601.png)



## 5.2 三种模式

进入Vim：

![image-20221120134659774](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201346859.png)

```basic
vim fileName # 文件如果存在则进入编辑模式，如果不存在则是新增创建模式
```



基本上 vi/vim 共分为三种模式，分别是 **命令模式（Command mode）** ， **输入模式（Insert mode）** 和**底线命令模式（Last line mode）** 。这三种模式的作用分别是：

> 命令模式：

用户刚刚启动 vi/vim，便进入了命令模式。

![image-20221120135024368](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201350613.png)

此状态下敲击键盘动作会被Vim识别为命令，而非输入字符。比如我们此时按下`i`，并不会输入一个字符，i被当作了一个命令。

以下是常用的几个命令：

```basic
i 切换到输入模式，以输入字符。
x 删除当前光标所在处的字符。
: 切换到底线命令模式，以在最底一行输入命令。 如果是编辑模式，需要先退出编辑模式！ESC
```
若想要编辑文本：启动Vim，进入了命令模式，按下i，切换到输入模式。

命令模式只有一些最基本的命令，因此仍要依靠底线命令模式输入更多命令。

> 输入模式：
>

在命令模式下按下i就进入了输入模式。

![image-20221120135156247](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201351354.png)

在输入模式中，可以使用以下按键：

```basic
字符按键以及Shift组合 ，输入字符
ENTER ，回车键，换行
BACK SPACE ，退格键，删除光标前一个字符
DEL ，删除键，删除光标后一个字符
方向键 ，在文本中移动光标
HOME / END ，移动光标到行首/行尾
Page Up / Page Down ，上/下翻页
Insert ，切换光标为输入/替换模式，光标将变成竖线/下划线
ESC ，退出输入模式，切换到命令模式
```


> 底线命令模式

在命令模式下按下`:（英文冒号）`就进入了底线命令模式。光标就移动到了最底下，就可以在这里输入一些底线命令了！

![image-20221120135508343](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201355431.png)

底线命令模式可以输入单个或多个字符的命令，可用的命令非常多。

在底线命令模式中，基本的命令有（已经省略了冒号）：`wq`

```basic
q 退出程序
w 保存文件
```
![image-20221120135522620](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201355705.png)

**按ESC键可随时退出底线命令模式。**

![image-20221120135550186](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201355276.png)

## 5.3 基本演示使用说明

简单的说，我们可以将这三个模式想成底下的图标来表示：

![image-20221120135615436](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201356556.png)

1. `vim 文件名`进入到vim编辑器里面
2. 按`i`开始进入编辑模式，开始编写内容
3. 编写完成后退出编辑模式，`esc` 退出之后进入底线命令模式
4. 在底线命令模式输入`:wq`就保存退出！

编辑一个文本：`vi a.txt` -> `按 i` 进入insert模式 -> 编辑内容 -> `esc 退出编辑` -> `:wq` -> 保存退出/ `:q!` 强制退出



## 5.4 按键说明

### 1.一般按键

> 第一部分：一般模式可用的光标移动、复制粘贴、搜索替换等

| 移动光标的方法         |                                                              |
| ---------------------- | ------------------------------------------------------------ |
| **h 或 向左箭头键(←)** | 光标向左移动一个字符                                         |
| **j 或 向下箭头键(↓)** | 光标向下移动一个字符                                         |
| **k 或 向上箭头键(↑)** | 光标向上移动一个字符                                         |
| **l 或 向右箭头键(→)** | 光标向右移动一个字符                                         |
| [Ctrl] + [f]           | 屏幕『向下』移动一页，相当于 [Page Down]按键 (常用)          |
| [Ctrl] + [b]           | 屏幕『向上』移动一页，相当于 [Page Up] 按键 (常用)           |
| [Ctrl] + [d]           | 屏幕『向下』移动半页                                         |
| [Ctrl] + [u]           | 屏幕『向上』移动半页                                         |
| +                      | 光标移动到非空格符的下一行                                   |
| -                      | 光标移动到非空格符的上一行，配置文件中空格较多！             |
| **数字 < space>**      | 那个 n 表示『数字』，例如 20 。快捷切换光标， 数字 + 空格    |
| 0 或功能键[Home]       | 这是数字『 0 』：移动到这一行的最前面字符处 (常用)           |
| $ 或功能键[End]        | 移动到这一行的最后面字符处(常用)                             |
| H                      | 光标移动到这个屏幕的最上方那一行的第一个字符                 |
| M                      | 光标移动到这个屏幕的中央那一行的第一个字符                   |
| L                      | 光标移动到这个屏幕的最下方那一行的第一个字符                 |
| G                      | 移动到这个档案的最后一行(常用)                               |
| nG                     | n 为数字。移动到这个档案的第 n 行。例如 20G 则会移动到这个档案的第 20<br/>行(可配合 :set nu) |
| gg                     | 移动到这个档案的第一行，相当于 1G 啊！(常用)                 |
| **数字< Enter>**       | n 为数字。光标向下移动 n 行(常用)                            |



| 搜索替换  |                                                              |
| --------- | ------------------------------------------------------------ |
| **/word** | 向光标之下寻找一个名称为 word 的字符串。例如要在档案内搜寻 vbird 这个字符串，就输入 /vbird 即可！(常用) |
| ?word     | 向光标之上寻找一个字符串名称为 word 的字符串。               |
| **n**     | 这个 n 是英文按键。代表重复前一个搜寻的动作。举例来说， 如果刚刚我们执行 /vbird去向下搜寻 vbird 这个字符串，则按下 n 后，会向下继续搜寻下一个名称为 vbird 的字符串。如果是执行 ?vbird 的话，那么按下 n 则会向上继续搜寻名称为 vbird 的字符串！ |
| **N**     | 这个 N 是英文按键。与 n 刚好相反，为『反向』进行前一个搜寻动作。例如 /vbird后，按下 N 则表示『向上』搜寻 vbird 。 |



| 删除、复制与粘贴 |                                                              |
| ---------------- | ------------------------------------------------------------ |
| x, X             | 在一行字当中，x 为向后删除一个字符 (相当于 [del] 按键)， X 为向前删除一个字符(相当于 [backspace] 亦即是退格键) (常用) |
| nx               | n 为数字，连续向后删除 n 个字符。举例来说，我要连续删除 10 个字符， 『10x』。 |
| dd               | 删除游标所在的那一整行(常用)                                 |
| ndd              | n 为数字。删除光标所在的向下 n 行，例如 20dd 则是删除 20 行 (常用) |
| d1G              | 删除光标所在到第一行的所有数据                               |
| dG               | 删除光标所在到最后一行的所有数据                             |
| d$               | 删除游标所在处，到该行的最后一个字符                         |
| d0               | 那个是数字的 0 ，删除游标所在处，到该行的最前面一个字符      |
| yy               | 复制游标所在的那一行(常用)                                   |
| nyy              | n 为数字。复制光标所在的向下 n 行，例如 20yy 则是复制 20 行(常用) |
| y1G              | 复制游标所在行到第一行的所有数据                             |
| yG               | 复制游标所在行到最后一行的所有数据                           |
| y0               | 复制光标所在的那个字符到该行行首的所有数据                   |
| y$               | 复制光标所在的那个字符到该行行尾的所有数据                   |
| p, P             | p 为将已复制的数据在光标下一行贴上，P 则为贴在游标上一行！举例来说，我目前光标在第 20 行，且已经复制了 10 行数据。则按下 p 后， 那 10 行数据会贴在原本的 20行之后，亦即由 21 行开始贴。但如果是按下 P 呢？那么原本的第 20 行会被推到变成30 行。(常用) |
| J                | 将光标所在行与下一行的数据结合成同一行                       |
| c                | 重复删除多个数据，例如向下删除 10 行，[ 10cj ]               |
| **u**            | 复原前一个动作。(常用)                                       |
| [Ctrl]+r         | 重做上一个动作。(常用)                                       |



### 2.输入按键

> 第二部分：一般模式切换到编辑模式的可用的按钮说明

| 进入输入或取代的编辑模式 |                                                              |
| ------------------------ | ------------------------------------------------------------ |
| **i, I**                 | 进入输入模式(Insert mode)：i 为『从目前光标所在处输入』， I 为『在目前所在行的第一个非空格符处开始输入』。(常用) |
| a, A                     | 进入输入模式(Insert mode)：a 为『从目前光标所在的下一个字符处开始输入』，A 为『从光标所在行的最后一个字符处开始输入』。(常用) |
| o, O                     | 进入输入模式(Insert mode)：这是英文字母 o 的大小写。o 为『在目前光标所在的下一行处输入新的一行』；O 为在目前光标所在处的上一行输入新的一行！(常用) |
| r, R                     | 进入取代模式(Replace mode)：r 只会取代光标所在的那一个字符一次；R会一直取代光标所在的文字，直到按下 ESC 为止；(常用) |
| **[Esc]**                | 退出编辑模式，回到一般模式中(常用)                           |



### 3.指令按键

> 第三部分：一般模式切换到指令行模式的可用的按钮说明

| 指令行的储存、离开等指令                 |                                                              |
| ---------------------------------------- | ------------------------------------------------------------ |
| :w                                       | 将编辑的数据写入硬盘档案中(常用)                             |
| :w!                                      | 若文件属性为『只读』时，强制写入该档案。不过，到底能不能写入， 还是跟你对该档案的档案权限有关啊！ |
| :q                                       | 离开 vi (常用)                                               |
| :q!                                      | 若曾修改过档案，又不想储存，使用 ! 为强制离开不储存档案。    |
| **:wq**                                  | 储存后离开，若为 :wq! 则为强制储存后离开 (常用)              |
| ZZ                                       | 这是大写的 Z 喔！若档案没有更动，则不储存离开，若档案已经被更动过，则储存后离开！ |
| :w [filename]                            | 将编辑的数据储存成另一个档案（类似另存新档）                 |
| :r [filename]                            | 在编辑的数据中，读入另一个档案的数据。亦即将『filename』 这个档案内容加到游标所在行后面 |
| :n1,n2 w [filename]                      | 将 n1 到 n2 的内容储存成 filename 这个档案                   |
| :! command                               | 暂时离开 vi 到指令行模式下执行 command 的显示结果！例如『:! ls /home』即可在 vi 当中看 /home 底下以 ls 输出的档案信息！ |
| **:set nu** 设置行号，代码中经常会使用！ | 显示行号，设定之后，会在每一行的前缀显示该行的行号           |
| :set nonu                                | 与 set nu 相反，为取消行号！                                 |

注意：注意一下啊，那个惊叹号 (!) 在vi 当中，常常具有『强制』的意思～





# 6. 账号管理

你一般在公司中，用的应该都不是 root 账户！

Linux系统是一个`多用户多任务的分时操作系统`，任何一个要使用系统资源的用户，都必须首先向系统管理员申请一个账号，然后以这个账号的身份进入系统。

用户的账号一方面可以帮助系统管理员对使用系统的用户进行跟踪，并控制他们对系统资源的访问；另一方面也可以帮助用户组织文件，并为用户提供安全性保护。

每个用户账号都拥有一个唯一的用户名和各自的口令。

用户在登录时键入正确的用户名和口令后，就能够进入系统和自己的主目录。

实现用户账号的管理，要完成的工作主要有如下几个方面：

1. 用户账号的添加、删除与修改。
2. 用户口令的管理。
3. 用户组的管理。

## 6.1 用户账号的管理

用户账号的管理工作主要涉及到用户账号的添加、修改和删除。

添加用户账号就是在系统中创建一个新账号，然后为新账号分配用户号、用户组、主目录和登录Shell等资源。

属主，属组





> 添加用户

```basic
useradd -选项 用户名
```
选项: 

`-m`： 自动创建这个用户的主目录 /home/miaowei
`-G` : 给用户分配组！
`-c comment `指定一段注释性描述
`-d 目录` 指定用户主目录，如果此目录不存在，则同时使用-m选项，可以创建主目录
`-g 用户组` 指定用户所属的用户组。-
`-s` Shel文件 指定用户的登录Shell.
`-u` 用户号指定用户的用户号，如果同时有-0选项，则可以重复使用其他用户的标识用户名

用户名：

```
指定新账号的登录名。
```

添加一个用户：

![image-20221120151057197](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201510278.png)

理解一下本质：Linux中一切皆文件，这里的添加用户说白了就是往某一个文件中写入用户的信息了！
`/etc/passwd`查看这个配置文件：

![image-20221120151128559](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201511619.png)





> 删除用户 userdel

```basic
userdel -r miaowei #删除用户的时候将他的目录页一并删掉！
# 常用的选项是 -r，它的作用是把用户的主目录一起删除
```

![image-20221120151322129](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201513213.png)







> 修改用户

修改用户账号就是根据实际情况更改用户的有关属性，如用户号、主目录、用户组、登录Shell等。

修改已有用户的信息使用usermod命令，其格式如下：

```basic
usermod 选项 用户名
```

选项包括-c, -d, -m, -g, -G, -s, -u以及-o等，这些选项的意义与useradd命令中的选项一样，可以为用户指定新的资源值。

示例：

![image-20221120153116551](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201531613.png)

此时将miao这个用户的主目录迁移到233这个目录，可是很明显不存在，再看配置文件：

![image-20221120153206874](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201532932.png)

得出两点：

1. 在迁移主目录时若不存在则表面上迁移失败
2. 最终都要看配置文件，配置文件才是最准确的！

所以每次修改完毕之后查看配置文件即可！





> 切换用户！

root用户

![image-20221120153739893](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201537957.png)

1.切换用户的命令为：`su username` 【username是你的用户名哦】

2.从普通用户切换到root用户，还可以使用命令：`sudo su`

![image-20221120153802914](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201538996.png)

3.在终端输入exit或logout或使用快捷方式ctrl+d，可以退回到原来用户，其实ctrl+d也是执行的exit命令

![image-20221120153836906](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201538967.png)

4.在切换用户时，如果想在切换用户之后使用新用户的工作环境，可以在su和username之间加-，例
如：【su - root】

$表示普通用户

#表示超级用户，也就是root用户

有的小伙伴在阿里云买完服务器后，主机名是一个随机字符串！

![image-20221120154221059](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201542118.png)





> 用户的密码设置问题！

我们一般通过root创建用户的时候！要配置密码！

Linux上输入密码是不会显示的，你正常输入就可以了，并不是系统的问题！

在公司中，你们一般拿不到公司服务器的 root 权限，都是一些分配的账号！

如果是超级用户的话：

```basic
passwd 用户名：
new password：新密码
re password：重新再输入一次密码
```

如果是普通用户：

```bash
passwd
(current) UNIX password: 
new password： # 密码不能太过于简单！
re password：
```





> 锁定账户！

root，比如张三辞职了！冻结这个账号，一旦冻结，这个人就登录不上系统了！

```basic
passwd -l qinjiang # 锁定之后这个用户就不能登录了！
passwd -d qinjiang # 没有密码也不能登录！类似于将这个密码清空的意思
```

![image-20221120154454667](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201544744.png)

在公司中，你一般触及不到 root 用户！作为一个开发一般你拿不到!

这以上的基本命令，大家必须要掌握！但是自己玩的时候可以使用来学习！Linux是一个多用户的系统！



## 6.2 用户组管理

属主、属组

每个用户都有一个用户组，系统可以对一个用户组中的所有用户进行集中管理（开发、测试、运维、root）。不同Linux 系统对用户组的规定有所不同，如Linux下的用户属于与它同名的用户组，这个用户组在创建用户时同时创建。

用户组的管理涉及用户组的添加、删除和修改。组的增加、删除和修改实际上就是对`/etc/group`文件的更新。

> 创建一个用户组 groupadd

![image-20221120155244824](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201552886.png)

创建完用户组后可以得到一个组的id，这个id是可以指定的！ `-g 520 `， 若果不指定就是自增 1

> 删除用户组 groupdel

![](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201553277.png)

> 修改用户组的权限信息和名字 groupmod -g -n

![image-20221120155331034](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201553103.png)

> 用户如果要切换用户组怎么办呢？

```basic
# 登录当前用户 qinjiang
$ newgrp root
```



> 拓展：文件的查看！（了解即可）

/etc/passwd

```
用户名:口令(登录密码，我们不可见):用户标识号:组标识号:注释性描述:主目录:登录Shell
```

![image-20221120162836834](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201628887.png)

这个文件中的每一行都代表这一个用户，我们可以从这里看出这个用户的主目录在那里，可以看到属于哪一个组！一 一对应

登录口令：把真正的加密后的用户口令字存放到`/etc/shadow`文件中，保证我们密码的安全性！

用户组的所有信息都存放在`/etc/group`文件中。





# 7. 磁盘管理

```basic
df （列出文件系统整体的磁盘使用量） du（检查磁盘空间使用量！）
```
df！

![image-20221120155659667](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201556744.png)

du！

只显示当前目录下面的子目录的目录大小和当前目录的总的大小，最下面的186044为当前目录的总大小

![image-20221120155715578](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201557647.png)

![image-20221120155728563](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201557671.png)

![image-20221120155748745](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201557804.png)

> Mac 或者想使用Linux 挂载我们的一些本地磁盘或者文件！

挂载：mount

![image-20221120155827402](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201558955.png)


卸载：umount -f [挂载位置] 强制卸载

除了这个之外，以后我们安装了JDK ，其实可以使用java中的一些命令来查看信息！



# 8. 进程管理

Linux中一切皆文件

**（文件：读写执行（查看，创建，删除，移动，复制，编辑），权限（用户、用户组）。系统：（磁**
**盘，进程））**

对于我们开发人员来说，其实Linux更多偏向于使用即可！



> 基本概念！

1 、在Linux中，每一个程序都是有自己的一个进程，每一个进程都有一个id号！

2 、每一个进程呢，都会有一个父进程！

3 、进程可以有两种存在方式：前台！后台运行！

4 、一般的话服务都是后台运行的，基本的程序都是前台运行的！



> 命令

**ps** 查看当前系统中正在执行的各种进程的信息！

ps -xx ：

`-a` 显示当前终端运行的所有的进程信息（当前的进程一个）
`-u` 以用户的信息显示进程
`-x` 显示后台运行进程的参数！

```basic
# ps -aux 查看所有的进程信息
ps -aux|grep mysql	# 查看mysql的进程
ps -aux|grep redis	# 查看redis的进程

# | 在Linux这个叫做管道符 A|B	第一个命令A的输出结果带到B命令中
# grep 查找文件中符合条件的字符串！
```

对于我们来说，这里目前只需要记住一个命令即可 `ps -xx|grep 进程名字！` 过滤进程信息！



**ps -ef：可以查看到父进程的信息**

```basic
ps -ef|grep mysql # 看父进程我们一般可以通过目录树结构来查看！

# 进程树！更直观更来得一清二楚
pstree -pu
-p 显示父id
-u 显示用户组
```
![image-20221120160256913](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211201602988.png)

结束进程：杀掉进程，等价于window结束任务！

`kill -9 进程的id`

但是啊，我们平时写的一个Java代码死循环了，可以选择结束进程！杀进程

表示强制结束该进程！



将Java程序打包发的时候讲解！ `nohup` ，代表后台执行程序

# 9. 环境安装

> 安装软件一般有三种方式：

```
rpm（Jdk：在线发布一个SringBoot项目！）
解压缩（tomcat，启动并通过外网访问，发布网站）
yum在线安装（docker：直接安装运行跑起来docker就可以！）！
```
## 9.1 防火墙

Linux的防火墙端口是开启的，如果是阿里云，需要保证阿里云的安全组策略是开放的！

```basic
# 查看firewall服务状态
systemctl status firewalld

# 开启、重启、关闭、firewalld.service服务
# 开启
service firewalld start
# 重启
service firewalld restart
# 关闭
service firewalld stop

# 查看防火墙规则
firewall-cmd --list-all    # 查看全部信息
firewall-cmd --list-ports  # 只看端口信息

# 开启端口
开端口命令：firewall-cmd --zone=public --add-port=80/tcp --permanent
重启防火墙：systemctl restart firewalld.service

命令含义：
--zone #作用域
--add-port=80/tcp  #添加端口，格式为：端口/通讯协议
--permanent   #永久生效，没有此参数重启后失效
```



## 9.2 RPM源码安装

这种方式拿到的是软件的源码包，需要自行 make 进行编译，然后 install 安装。同时make命令需要有gcc的环境。

先安装环境,以Redis为例：https://my.oschina.net/liuyuantao/blog/915785



特点：这种安装方式，软件包会自动配置jdk的环境变量，不用手动配置。也是最方便快捷的一种方式

命令：

- `rpm -qa 软件名称` ：查询软件是否被安装
- `rpm -ivh 软件包路径` 需要安装的包文件 ： rpm –ivh xxx.rpm
- `rpm -e --nodeps 需要卸载的软件包` ： 卸载软件（–nodeps 忽略依赖关系并继续操作）
- `rpm -qa | grep 查询名称` ：利用管道模糊查询软件安装情况：

例如：**rpm -qa | grep java** 检测系统自带的jdk安装包

结构：

```
rpm [选项] [参数]

    -a：查询所有软件包
    -e：删除指定的软件包
    -f<文件>：查询拥有指定文件的套件； 
    -h或-–hash：显示进度信息 ，以#显示进度
    -i：显示包的详细信息
    -i<软件包>或-–install<软件包>：安装指定的软件包 
    -l：显示包的文件列表
    -p：查询指定的RPM包 
    -q：使用询问模式
    -U<软件包>或-–upgrade<软件包>：升级指定的程序包
    -v：显示指令执行详细过程
    -vv：详细显示指令执行过程，便于排错
```



### 1. JDK安装

我们开发java程序必须要的环境！

1 、下载JDK rpm。去oralce 官网下载即可！

rpm下载地址http://www.oracle.com/technetwork/java/javase/downloads/index.htmlq

2 、安装java环境

```basic
#检测当前系统是否存在java环境！ java -version
#如果有的话就需要卸载
# rpm -qa|grep jdk # 检测JDK版本信息
# rpm -e --nodeps jdk_几-----卸载

# 卸载完毕后即可安装jdk
# rpm -ivk rpm包
# 配置环境变量！
```

如果存在可以提前卸载：

![image-20221121092628176](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211210926242.png)

安 装：

![image-20221121093203963](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211210932025.png)

配置环境变量： `/etc/profile` 在文件的最后面增加java的配置和 window安装环境变量一样！

![image-20221122154952387](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211221549405.png)

```bash
JAVA_HOME=/usr/java/jdk1.8.0_221-amd64			# 找到jdk的安装目录-java的根路径
CLASSPATH=%JAVA_HOME%/lib:%JAVA_HOME%/jre/lib	# 配置环境变量-类路径文件
PATH=$PATH:$JAVA_HOME/bin:$JAVA_HOME/jre/bin	# $取变量-环境变量的文件
export PATH CLASSPATH JAVA_HOME					# 导出让系统识别
# 注意：:在Linux中标示分隔路径，最好是直接赋值粘贴到这个文件的最下面，然后退出后输入下面的命令让文件生效
```

让这个配置文件生效！ `source /etc/profile` 让新增的环境变量生效！

我们来发布一个项目试试！

注意点：

1. 先去阿里云安全组和防火墙都要一并开启：https://blog.csdn.net/lfdfhl/article/details/126880975
2. 先将jar包传到服务器home目录下的用户目录下
3. 前台运行直接`java -jar Beoka.jar`就可以运行了，如果不想运行直接Ctrl+C就直接退出了

### 2. Tomcat 安装

ssm war 就需要放到tomcat 中运行！

1 、下载tomcat。官网下载即可 tomcat9 apache-tomcat-9.0.22.tar.gz

2 、解压这个文件

```bash
tar -zxvf apache-tomcat-9.0.22.tar.gz
```

3 、启动tomcat测试！ ./xxx.sh 脚本即可运行

```basic
# 执行：startup.sh -->启动tomcat
# 执行：shutdown.sh -->关闭tomcat
./startup.sh
./shotdown.sh
```

![image-20221122171603490](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202211221716961.png)



## 9.3 Yum在线安装

介绍：

​	这种方式是在线安装，自动下载需要安装的软件，同时自动安装软件所依赖的其他软件，如果yum仓库中没有要安装的软件需要更换或者添加 “yum仓库" 。

特点：

​	将所有软件包放到官方服务器上，当进行yum在线安装时，可以自动解决依赖性问题；

缺点：

​	安装过程中，rpm包依赖性太强；

```java
yum命令：
	yum list --查询所有可用软件包列表
	yum search 关键字 --搜索服务器上所有和关键字相关的包
	yum -y install 包名 (-y 自动回答yes)
	yum安装只写包名即可！ eg:yum -y install gcc --c语言编译器
	升级：yum -y update 包名 : -y自动回答yes
	卸载：yum -y remove 包名
```

例子：

比如要安装Git，只需要执行 `yum install git`



### 1. Docker（yum安装）

联网的情况下 yum install -y yum源

官网安装参考手册：https://docs.docker.com/install/linux/docker-ce/centos/

我们现在是在Linux下执行，一定要联网 ，yum 在线安装！

1 、检测CentOS 7

```basic
[root@192 Desktop]# cat /etc/redhat-release
CentOS Linux release 7.2.1511 (Core)
```

2、yum安装gcc相关（需要确保 虚拟机可以上外网 ）

```basic
yum -y install 包名   # yum install 安装命令 -y 所有的提示都为 y
yum -y install gcc
yum -y install gcc-c++
```

3、卸载旧版本

```basic
yum -y remove docker docker-common docker-selinux docker-engine
# 官网版本
yum remove docker \
          docker-client \
          docker-client-latest \
          docker-common \
          docker-latest \
          docker-latest-logrotate \
          docker-logrotate \
          docker-engine
```

4、安装需要的软件包

```
yum install -y yum-utils device-mapper-persistent-data lvm2
```

5、设置stable镜像仓库

```basic
# 错误
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
## 报错
[Errno 14] curl#35 - TCP connection reset by peer
[Errno 12] curl#35 - Timeout

# 正确推荐使用国内的
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

6、更新yum软件包索引

```
yum makecache fast
```

7、安装Docker CE

```
yum -y install docker-ce docker-ce-cli containerd.io
```

8、启动docker

```
systemctl start docker
```

9、测试

```
docker version

docker run hello-world

docker images
```



## 9.5 宝塔面板（懒人式安装）

具体的教程 https://www.bilibili.com/video/BV177411K7bH

# 10.启动程序

我们在开发的时候经常要启动程序然后关闭程序：

## 10.1 命令方式

以命令方式运行jar包方式：

https://blog.csdn.net/qq_42169450/article/details/122688940



> 检查是否成功

使用 jobs 命令：

```
[root@localhost test]# jobs
[4]+  Running                 nohup python -u test.py > test_out.out 2>&1 &
```

使用 `ps aux` 命令，查看程序的进程号：

```
[root@localhost test]# ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root     22246  0.0  0.3 125572  6304 pts/0    S    17:29   0:00 python -u test.py
```

> 关闭挂起进程

查看正在运行中的PID：

```java
ps -ef|grep java
```

使用 `kill -9 进程号`，关闭指定进程号的程序

```
[root@localhost test]# kill -9 60000
```

## 10.2 可执行文件

在Linux系统中执行一个可执行文件，只需写正确文件路径，即可执行文件，不需要写命令。

> ./文件名

注意：以上操作的前提条件：文件是可执行文件，且当前执行的角色是有执行权限的用户（文件有x权限）
