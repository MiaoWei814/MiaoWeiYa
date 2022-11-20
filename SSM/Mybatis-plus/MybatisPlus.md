# 笔记

## 1. 概念

简介：Mybatis-Plus是一个Mybatis的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

特性：

- **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
- **损耗小**：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
- **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
- **支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
- **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
- **支持 ActiveRecord 模式**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
- **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
- **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
- **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
- **分页插件支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
- **内置性能分析插件**：可输出 SQL 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
- **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作

官网地址：https://baomidou.com/pages/24112f/#%E7%89%B9%E6%80%A7

## 2.快速开始

> 步骤

1. 新建数据库“mybatis_plus_miao”，导入官网的SQL表及其数据：

   ```sql
   DROP TABLE IF EXISTS user;
   
   CREATE TABLE user
   (
       id BIGINT(20) NOT NULL COMMENT '主键ID',
       name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
       age INT(11) NULL DEFAULT NULL COMMENT '年龄',
       email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
       PRIMARY KEY (id)
   );
   ```

   ```mysql
   DELETE FROM user;
   
   INSERT INTO user (id, name, age, email) VALUES
   (1, 'Jone', 18, 'test1@baomidou.com'),
   (2, 'Jack', 20, 'test2@baomidou.com'),
   (3, 'Tom', 28, 'test3@baomidou.com'),
   (4, 'Sandy', 21, 'test4@baomidou.com'),
   (5, 'Billie', 24, 'test5@baomidou.com');
   ```

2. 新建一个工程：

   1. 导入依赖：

      ```xml
      <!--数据库连接驱动-->
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
      </dependency>
      <!--mybatis-plus-->
      <dependency>
          <groupId>com.baomidou</groupId>
          <artifactId>mybatis-plus-boot-starter</artifactId>
          <version>3.0.5</version>
      </dependency>
      //这是重要的依赖，其他都是基本的依赖就不粘贴了！
      ```

      注意：在导入mybatis-plus要避免同时和mybatis同时导入，这会引起版本的差异等其他问题！

   2. 连接数据库：

      ```yml
      # DataSource Config
      spring:
        datasource:
          driver-class-name: com.mysql.cj.jdbc.Driver
          # useSSL:使用安全配置 serverTimezone=GMT%2B8 时区配置
          url: jdbc:mysql://localhost:3306/mybatis_plus_miao?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT%2B8
          username: root
          password: root
      ```

   3. 新建mapper：

      ```java
      //在对应的mapper上面继承基本的类BaseMapper
      @Repository  //代表持久层
      public interface UserMapper extends BaseMapper<User> {
          //这里面就有基本的CRUD的代码了
      }
      ```

      这里我们点击BaseMapper就可以发现已经提供了基本的CRUD的代码，并且我们只需要传递一个泛型就行

      ![image-20220213141639387](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/imgs/202202131416503.png)

   4. 实体类和在主启动类上添加@MapperScan("mapper的路径")省略

   5. 测试：

      ```java
      @SpringBootTest
      class SpringBootMybatisPlusDemoApplicationTests {
      
          @Autowired
          private UserMapper userMapper;
          @Test
          void contextLoads() {
              //这里条件参数是Wrapper，表示这是条件构造器
              //这里表示没有条件，就直接查询全部用户
              List<User> users = userMapper.selectList(null);
              users.forEach(System.out::println);
          }
      }
      ```

   6. 结果：

      ![image-20220213141917957](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/imgs/202202131419996.png)

## 3.配置日志

在我们的开发过程中，我们需要知道这段SQL在数据库中它是如何执行的，所以需要查看日志并打开它！

```yaml
# 配置日志
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl  # 这是控制台打印日志
```

这是其他日志：

![image-20220213142652310](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/imgs/202202131426363.png)

如果要导入例如：Log4J2或者Slf4J那么就需要导入对应的依赖即可！

这是结果：

![image-20220213142759280](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/imgs/202202131427358.png)



但是要注意：项目上线的话就要关闭这个打印日志的功能，不然会浪费时间的！



## 4.CURD扩展

### 4.1 插入及相关主键生成策略

我们先测试一下插入，看看会发生什么样的变化

```java
@Test
    void testInsert(){
        User user = new User();
        user.setAge(3);
        user.setName("miao");
        user.setEmail("不告诉你");
		int row=userMapper.insert(user); //受影响的行数
        System.out.println(user);
    }
```

看结果：

![image-20220213144713173](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/imgs/202202131447225.png)

> 咦？为什么这个id这么长，难道不应该自增+1么？吓得我赶紧去看了下数据库表果然没有设置自增，那么点击自增然后重新运行看看呢？

![image-20220213144830906](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/imgs/202202131448945.png)

可以发现两者自增也是不规律啊！这个其实是这样子的：

> 数据库插入的id的默认值为：全局的唯一id

-----

找到User表的id，添加注解@TableId，可以看到type的默认是`IdType.ID_WORKER`为全局的唯一id

这是相关分布式系统唯一id生成：https://www.cnblogs.com/haoxinyue/p/5208136.html

**雪花算法**

snowflake是Twitter开源的分布式ID生成算法，结果是一个long型的ID。其核心思想是：使用41bit作为毫秒数，10bit作为机器的ID（5个bit是数据中心，5个bit的机器ID），12bit作为毫秒内的流水号（意味着每个节点在每毫秒可以产生 4096 个 ID），最后还有一个符号位，永远是0。可以保证几乎全球唯一！

> AUTO自增策略

1. 实体类字段上`@TableId(type = IdType.AUTO)`

2. 数据库字段一定是要自增，否则就会发生异常！

3. 启动重试插入测试：

   ![image-20220213150523789](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/imgs/202202131505840.png)

> INPUT手动输入

1. 实体类字段上`@TableId(type = IdType.INPUT)`

2. 意思就是这个id需要我们手动输入

3. 测试：

   ```java
   @Test
   void testInsert(){
       User user = new User();
       user.setAge(3);
       user.setName("miao");
       user.setEmail("不告诉你");
       user.setId(111L);
       int insert = userMapper.insert(user);
       System.out.println(insert);
       System.out.println(user);
   }
   ```

4. 结果：

   ![image-20220213151533706](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/imgs/202202131515746.png)

​	注意：如果你这里也没有给id，那么数据库如果有自增那么它就会接着继续自增！

> 其他

```java
public enum IdType {
    AUTO(0),  //数据库id自增
    NONE(1),  //未设置主键
    INPUT(2), //手动输入
    ID_WORKER(3), //默认的全局唯一id
    UUID(4),      //全局唯一的id uuid
    ID_WORKER_STR(5); //ID_WORKER 字符串表示法
}
```

###  4.2 更新及自动填充

更新操作：

```java
@Test
void testUpdate(){
    User user = new User();
    user.setId(111L);
    user.setName("这是我新名字");
    userMapper.updateById(user);
}
```

![image-20220213153318945](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/imgs/202202131533984.png)

然后追加一下我再更新一个字段：

`user.setEmail("wwww")`

![image-20220213153736013](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/imgs/202202131537049.png)

可以发现：这个自动拼接SQL是非常的方便了，按照我们以前的Mybatis来讲就得写if判断，可以说节省了很多的时间了！

> 自动填充

创建时间、修改时间！这些个操作一遍都是自动化完成的，我们不希望手动更新！

阿里巴巴Java开发手册：![image-20220319180117921](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/imgs/202203191801971.png)

方式一：数据库级别

1. 新增数据库字段`create_time`，类型为`dateTime`，默认为`CURRENT_TIMESTAMP`
2. 新增后看效果

![image-20220319180748315](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/imgs/202203191807387.png)

**理解:**可以发现它的默认值就是获取当前时间，可以这是动的数据库如果我的主观意想不想新增这个字段那么就会是默认值，所以这个是不建议的！

方式二：代码级别

1. 在对应Java字段上添加注解`TableField`,如:`@TableField(fill = FieldFill.INSERT)`; 意思就是说表明了该注解就表示在新增或者更新的时候就会执行对应的填充字段!

其中fill表示填充的意思，类型含有：

```java
/**
 * <p>
 * 字段填充策略枚举类
 * </p>
 *
 * @author hubin
 * @since 2017-06-27
 */
public enum FieldFill {
    /**
     * 默认不处理
     */
    DEFAULT,
    /**
     * 插入填充字段
     */
    INSERT,
    /**
     * 更新填充字段
     */
    UPDATE,
    /**
     * 插入和更新填充字段
     */
    INSERT_UPDATE
}
```

2. 写处理器:

```java
@Slf4j
@Component
public class MyMetaObjectHandler implements MetaObjectHandler {

    @Override
    public void insertFill(MetaObject metaObject) {
        log.info("start insert fill ....");
        //第一个字段:字段名
        //第二个字段:设置的字段值
        //第三个字段:要给哪个数据处理
        if (metaObject.getValue("createTime") == null) {
            this.setFieldValByName("createTime", new Date(), metaObject);
        }
    }

    @Override
    public void updateFill(MetaObject metaObject) {
        log.info("start update fill ....");
        this.setFieldValByName("updateTime", new Date(), metaObject);
    }
}
```

3. 然后新增:

```java
@Test
    void testInsert(){
        User user = new User();
        user.setAge(3);
        user.setName("miao");
        user.setEmail("不告诉你");
        user.setId(111L);
        int insert = userMapper.insert(user);
        System.out.println(insert);
        System.out.println(user);
    }
```

数据库:

![image-20220319183833844](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/imgs/202203191838906.png)



**理解：**这里给我感觉就是加了那个注解之后然后此时就会经过处理器，然后给对应的字段赋值，这里更像一种拦截器一样，不过这种自动更新我觉得还很麻烦！然后在处理器中需要做一个判断，如果该值为null那么就用自动化时间赋值，否则就不做改变直接存数据库！

更新操作：https://blog.csdn.net/weixin_45817985/article/details/126764318

### 4.3 乐观锁

> 乐观锁：顾名思义十分乐观，它总是认为不会出现问题！无论干什么都不会上锁，如果出现了问题，再次更新值测试!
>
> 悲观锁：顾名思义十分悲观，它总是认为会出现问题，无论干什么都会上锁，再次操作！

这是官网解释：https://baomidou.com/pages/0d93c0/#optimisticlockerinnerinterceptor

乐观锁实现方式：

- 取出记录时，获取当前 version
- 更新时，带上这个 version
- 执行更新时， set version = newVersion where version = oldVersion
- 如果 version 不对，就更新失败

**理解：**数据库级别的乐观锁就是带一个version版本号，然后每次修改的时候就会将查询的version带进来作为where条件，如果这条记录没有被别人改过那么就正常修改，如果一般被修改那么version就被改变，所以此时更新失败！

```sql
乐观锁:1.先查询,获得版本号 version=1
---A线程
update user set name="miaowei",version=version+1 where id=2 and version=1

--B线程 线程抢先完成,这个时候version=2,就会导致A修改失败!
update user set name="miaowei",version=version+1 where id=2 and version=1
```

> 测试:

1. 数据库增加`version`字段，并默认为1
2. JavaBean增加一个字段`version`并添加注解`@Version`

注:这里的注解一定是mybatis-plus的注解，不然没效果

3. 增加一个拦截器

```java
@Configuration //配置类
@EnableTransactionManagement  //开启自动事务管理器
@MapperScan("cn.miao.mapper")
public class MybatisPlusConfig {
    /**
     * 注册乐观锁插件-旧版
     */
    @Bean 
    public OptimisticLockerInterceptor optimisticLockerInterceptor() {
        return new OptimisticLockerInterceptor();
    }
    //如果版本是3.0及以下的那么可以用旧版,如果以上比如3.5版本的,那么就得用这个:
    
    /**
     *  注册乐观锁插件-新版
     */
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();
        mybatisPlusInterceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        return mybatisPlusInterceptor;
    }
}
```

4. 测试乐观锁-成功

```java
//测试乐观锁-成功
@Test
public void test1() throws Exception{
    //先查询乐观锁version
    User user = userMapper.selectById(1L);
    //修改
    user.setName("缪威");
    //更新
    userMapper.updateById(user);
}
```

![image-20220320103607855](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/imgs/202203201036907.png)

可以发现加了`@version`后就会在where条件中有version这一条件!

5. 测试乐观锁-并发修改失败

这里模拟了线程并发时的操作

```java
//测试乐观锁-失败
@Test
public void test2() throws Exception{
    //线程1
    User user = userMapper.selectById(1L);
    user.setName("缪威1");

    //线程2
    User user1 = userMapper.selectById(1L);
    user1.setName("缪威2");
    int updateById1 = userMapper.updateById(user1);
    System.out.println("updateById1 = " + updateById1);

    //更新-如果没有乐观锁就会覆盖插队线程的值
    int updateById = userMapper.updateById(user);
    System.out.println("updateById = " + updateById);

}
```

控制台:

![image-20220320110328056](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/imgs/202203201103131.png)

### 4.4 查询操作

```java
//测试查询
@Test
public void test3() throws Exception{
   //通过id查询
    User user = userMapper.selectById(1);
    System.out.println(user);

    //批量id查询
    List<User> userList = userMapper.selectBatchIds(Arrays.asList(1, 2, 3));
    userList.forEach(System.out::println);

    //条件查询map
    List<User> users = userMapper.selectByMap(MapUtil.of("name", "Tom"));//会自动拼接到SQL的where条件中
    users.forEach(System.out::println);
}
```

### 4.5 分页查询

分页在网站使用的可是十分之多!

1. 原始的limit进行分页
2. PageHelper第三方插件
3. Mabatis-Plus也内置了分页插件

> 如何使用!

maven:

```xml-dtd
<!--mybatis-plus-->
<dependency>
    <groupId>com.baomidou</groupId>
    	<artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.0.5</version>
</dependency>
```

1. 配置分页拦截器组件

```java
@Bean
public PaginationInterceptor mybatisPlusInterceptor() {
    return new PaginationInterceptor();
    
    @Bean
    public ConfigurationCustomizer configurationCustomizer() {
        //清除缓存
        return configuration -> configuration.setUseDeprecatedExecutor(false);
    }
}
```

2. 直接使用Page对象即可!

```java
 //测试分页
Page<User> page = new Page<>(1, 5); //开始分页
IPage<User> userIPage = userMapper.selectPage(page, null); 

userIPage.getRecords().forEach(System.out::println);
System.out.println(userIPage.getTotal()); //分页总数
```

注意:这里虽然使用的是mybatis-plus自带的查询分页，但很多时候我们是自定义SQL语句，所以我们也可以效仿这个

如：

```java
test:
 //测试分页
Page<User> page = new Page<>(1, 5);
//手动分页
List<User> xx=userMapper.select(3, page);

System.out.println(xx);
System.out.println(page.getTotal());

//mapper:
public interface UserMapper extends BaseMapper<User> {
    @Select("select * from user where age=#{id}")
    List<User> select(@Param("id") Integer id, @Param("param")Page<User> page);//这里尝试过加不加注解都不影响
}
```

![image-20220320131632370](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/imgs/202203201316436.png)

事实证明是可以的!

高版本:

maven：

```xml-dtd
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.5.1</version>
</dependency>
```

拦截器:

```java
@Configuration //配置类
@EnableTransactionManagement  //开启自动事务管理器
@MapperScan("cn.miao.mapper")
public class MybatisPlusConfig {
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        //分页插件
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        return interceptor;
    }
}
```

运行测试：

代码还是跟之前一样--不变

![image-20220320134217263](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/imgs/202203201342336.png)

还是可以分页，嘻嘻嘻！

> 链接：https://baomidou.com/pages/97710a/#%E8%87%AA%E5%AE%9A%E4%B9%89%E7%9A%84-mapper-method-%E4%BD%BF%E7%94%A8%E5%88%86%E9%A1%B5

### 4.6 删除操作

![image-20220320140540208](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/imgs/202203201405274.png)

解释：

1. `delete`: 第一个使用Wrapper条件构造器来删除
2. `deleteById`:根据id来删除
3. `deleteByMap`: 根据map的作为列
4. `deleteBatchIds`:  根据id批量删除

> 这个删除delete是直接删除数据的,是物理删除的那种！

### 4.7 逻辑删除

> 这里逻辑删除的话那么可以模拟删除数据的情况，无论查询还是更新都是将这个逻辑删除的状态加上作为条件

1. 数据库加上字段`deleted`

   1. 实体类上加上对应字段`deleted`,并添加注解`@TableLogic`

   2. 在yaml配置文件进行配置:

      ```yaml
      mybatis-plus:
      	global-config:
          	db-config:
                logic-delete-value: 1 # 删除状态为1 
                logic-not-delete-value: 0 # 正常有效的状态为0
      ```

   3. 添加组件(可选)

      ```java
      //这个看版本,如果是3.0的那么就需要这个组件,如果是3.5的版本那么就不需要添加组件了
      
      //逻辑删除组件
      @Bean
      public ISqlInjector sqlInjector(){
          return new LogicSqlInjector();
      }
      ```

   4. 测试:

      4.1 删除

      ```java
      @Test
      public void test5() throws Exception{
          userMapper.deleteById(2);
      }
      ```

      ![image-20220320145748537](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/imgs/202203201457593.png)

​			可以发现，这里删除的SQL语句已经从delete修改为update更新了, 而且这里指定删除的状态的跟我们在yaml里是一样的!

​	   4.2 查询

```java
@Test
public void test5() throws Exception{
    userMapper.selectById(2);
}
```

![image-20220320150055129](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/imgs/202203201500185.png)

可以发现，在查询的时候已经自动给我们带上相关状态进行查询了！

## 5.条件构造器

我们在开发过程中大多数都是以查询为主，这里根据不同的条件来查询SQL语句；

接下来就会根据案例一步一步演示：

1. 测试一 `isNotNull`、`ge`

```java
//查询name不为空的用户,并且邮箱不为空的用户,年龄大于等于12
QueryWrapper<User> wrapper = new QueryWrapper<>();
wrapper.isNotNull("name").isNotNull("email").ge("age", 12);
userMapper.selectList(wrapper).forEach(System.out::println);
```

2. 测试二 `eq`

```java
//查询年龄=12
QueryWrapper<User> wrapper = new QueryWrapper<>();
wrapper.eq("age",12);
User user=userMapper.selectOne(wrapper)
```

3. 测试三 `between`

```java
//查询年龄在20-30区间的数量
QueryWrapper<User> wrapper = new QueryWrapper<>();
wrapper.between("age", 20, 30);
Long aLong = userMapper.selectCount(wrapper);
```

4. 测试四 `like`、`likeRight`

```java
//模糊查询
QueryWrapper<User> wrapper = new QueryWrapper<>();
wrapper.notLike("name", "e").likeRight("email", "t");
List<Map<String, Object>> selectMaps = userMapper.selectMaps(wrapper);
System.out.println(selectMaps);
```

5. 测试五 `inSql`

```java
QueryWrapper<User> wrapper = new QueryWrapper<>();
//id在子查询中查出来
wrapper.inSql("id", "select id from user where id<3");
List<Object> objects = userMapper.selectObjs(wrapper);
System.out.println(objects);
```

6. 测试六 `orderByDesc`

```java
QueryWrapper<User> wrapper = new QueryWrapper<>();
//id在子查询中查出来
wrapper.orderByDesc("id");
List<User> objects = userMapper.selectList(wrapper);
```

## 6.代码生成器

```java
package cn.miao;

import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.annotation.FieldFill;
import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.core.config.GlobalConfig;

import java.util.ArrayList;

// 代码自动生成器
public class KuangCode {
    public static void main(String[] args) {
// 需要构建一个 代码自动生成器 对象
        AutoGenerator mpg = new AutoGenerator();
// 配置策略
// 1、全局配置
        GlobalConfig gc = new GlobalConfig();
        //获取当前项目的地址
        String projectPath = System.getProperty("user.dir");
        //输出目录
        gc.setOutputDir(projectPath + "/src/main/java");
        //设置作者
        gc.setAuthor("缪威");
        //是否打开资源管理器
        gc.setOpen(false); 
        // 是否覆盖
        gc.setFileOverride(false); 
        // 去Service的I前缀
        gc.setServiceName("%sService"); 
        //id的策略
        gc.setIdType(IdType.ID_WORKER);
        gc.setDateType(DateType.ONLY_DATE);
        //是否配置swagger注解
        gc.setSwagger2(true);
        mpg.setGlobalConfig(gc);
//2、设置数据源
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://localhost:3306/kuang_community?
                useSSL = false & useUnicode = true & characterEncoding = utf - 8 & serverTimezone = GMT % 2B8");
                dsc.setDriverName("com.mysql.cj.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("123456");
        dsc.setDbType(DbType.MYSQL);
        mpg.setDataSource(dsc);
//3、包的配置
        PackageConfig pc = new PackageConfig();
        pc.setModuleName("blog");
        pc.setParent("com.kuang");
        pc.setEntity("entity");
        pc.setMapper("mapper");
        pc.setService("service");
        pc.setController("controller");
        mpg.setPackageInfo(pc);
//4、策略配置
        StrategyConfig strategy = new StrategyConfig();

        strategy.setInclude("blog_tags", "course", "links", "sys_settings", "user_record", "
                user_say"); // 设置要映射的表名
		//下划线转驼峰
        strategy.setNaming(NamingStrategy.underline_to_camel);
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);
        // 自动lombok；
        strategy.setEntityLombokModel(true);
        //设置逻辑删除                    
        strategy.setLogicDeleteFieldName("deleted");
// 自动填充配置
        TableFill gmtCreate = new TableFill("gmt_create", FieldFill.INSERT); //设置字段填充
        TableFill gmtModified = new TableFill("gmt_modified",
                FieldFill.INSERT_UPDATE);
        ArrayList<TableFill> tableFills = new ArrayList<>();
        tableFills.add(gmtCreate);
        tableFills.add(gmtModified);
        strategy.setTableFillList(tableFills);
// 乐观锁
        strategy.setVersionFieldName("version");
        strategy.setRestControllerStyle(true);
        strategy.setControllerMappingHyphenStyle(true); //
        localhost:
        8080 / hello_id_2
        mpg.setStrategy(strategy);
        mpg.execute(); //执行
    }
}
```

以上只适合3.0之前的版本，如果是最新的版本如3.5那么就直接看官网的新的代码生成器，那么就可以；不过像这种都是百度一抓一大把，所以一次配置终生使用，知道有这回事如何使用知道怎么用就行了！！！

## 7.一些记录

### 7.1 日志打印

```java
# 方式一：这种会打印比较详细，会将sqlSession级别的信息都会打印且是全局
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl

# 方式二：这种会打印比较清晰，只打印请求参数和SQL片段
logging:
  level:
    com.baomidou.example.mapper: debug  # 指定 mapper 文件所在的包，如果有多级mapper包可以这样写：com.xx.xx.*.dao: debug
        
关闭日志打印：
mybatis-plus:
  configuration:
	log-impl: org.apache.ibatis.logging.nologging.NoLoggingImpl
```

### 7.2 更新操作

根据ID更新 updateById：

```java
 @Test
    void updateTest(){
        User user = new User();
        user.setId(19); //查询id的条件
        user.setRole("updateTest"); //更新字段的值
        System.out.println(userDao.updateById(user));
        //返回int类型，这里修改的是一条，返回1就是成功了，如果修改多条，就返回多少个数量，
        //如果返回的是0表示没有搜索到这条数据，修改不成功。
    }
}
```

根据条件更新update:

```java
方式1:使用QueryWrapper 操作:
 @Test
    void updateTest(){
        User user = new User();
        user.setRole("updatewhere"); //更新的字段
        // Query对象，用于设置条件，传User实体
        QueryWrapper<User> wrapper = new QueryWrapper<>(); 
        wrapper.eq("role","asdf"); // 设置查询条件
        userDao.update(user, wrapper);
    }

方式2:使用UpdateWrapper操作(推荐)
@Test
    void updateTest2(){
        //UpdateWrapper更新操作
        UpdateWrapper<User> warp = new UpdateWrapper<>();
        //通过set设置需要修改的内容，eq设置条件
        warp.set("pwd","warpper").set("token","warptoken").eq("user","zyh4");
        System.out.println(userDao.update(null,warp));
	}
```



