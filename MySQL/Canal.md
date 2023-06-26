# Canal解决MySQL与Redis之间数据同步

由于很简单都是实践类操作就不仔细写文档了，这是地址：https://blog.csdn.net/u014494148/article/details/126433498

**工作原理：**

`就是伪装成Slave从Binlog中复制SQL语句或者数据`

![image-20230626154556887](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202306261546973.png)

主从复制步骤：

1. 将Master的binary-log日志文件打开，mysql会把所有的DDL,DML,TCL写入BinaryLog日志文件中
2. Master会生成一个 log dump 线程，用来给从库的 i/o线程传binlog
3. 从库的i/o线程去请求主库的binlog，并将得到的binlog日志写到中继日志（relaylog）中
4. 从库的sql线程，会读取relaylog文件中的日志，并解析成具体操作，通过主从的操作一致，而达到最终数据一致
   

> 我们就可以通过Canal去自动同步数据库的binlog数据日志文件，然后再把数据同步到Redis，从而达到Mysql和Redis自动同步的功能

canal 作为 MySQL binlog 增量获取和解析工具，可将数据通过TCP协议将数据同步到canal-client也就是我们的应用中，因此我们可以使用下面这种方案来同步数据

![img](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202306261551293.png)

1. 首选需要开启Mysql的bin-log
2. 然后需要安装canal-server伪装成slave同步mysql中的数据
3. 编写canal-client客户端监听canal-server，把数据从canal-server中同步过来
4. 然后把拿到的数据写入Redis即可
   