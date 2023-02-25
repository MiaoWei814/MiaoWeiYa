## 1. 不要使用select *去查询

```sql
select * from table;   // 要写清楚你需要的字段
```

存在的问题：

1. 在使用\*号查询的时候*数据库会解析更多的对象、字段、权限、属性*。

2. 在SQL语句比较复杂硬解析比较多的情况下呢会对数据库造成非常*沉重的这个计算负担*。

3. 这个\*号会带上所有的字段，那比如表中存在某些字段text、Blob这种类型，那么在数据传输这个size呢就会变得非常的大-*增加了IO负担*。

4. 在微服务中DB和应用程序是分开的，所以说通过*网络传输*这种数据库结果的时候*开销*是非常非常大的。

5. select \*它失去了*覆盖索引的这个可能性*。

   ```
   比如说现在有个表 t（a,b,c,d,e,f)。其中a是主键，b是我们人为给它添加了一个索引，那么此时在磁盘上世纪上是两颗B+树，也就是聚集索引和辅助索引-分别保存的是 a b c d e f。和咱们这个ab。
   简单来讲：
   	全部的数据呢我们叫data然后索引我们叫index，如果说查询条件中 where条件可以通过b列的索引过滤一部分数据，那么这个时候查询是优先走了index，如果说用户只需要返回a和b这两列数据的话，那么可以直接通过index查询并返回用户的这个数据，那么这个查询是非常快的！但是如果用户使用的是select *的话那么就要获取非index的数据，那么它的查询过程除了使用索引过滤掉一些数据那么还要增加一次B+树的一次过程，那么此时索引的效率是发挥不出来的！
   ```

   
## 2. 小数据集驱动大数据集，用in不要用exists

```mysql
select * from goods where
	exists(select 1 from restaurants where goods.id=restaurants.id and restaurants.type=1);
--------------------------------------------------------------------------------------------------------------------------------
select * from goods as 'g' where
	g.id in (select id from restaurants where type=1);
```

存在的问题：

​	比如说restaurants这张表在数据库有8000多条数据，但是这个goods呢非常多有100多万条。当现在使用这个exists的时候会去查询所有goods然后来过滤这个exists里面的这些子句啊是否有包含。所以说这是先查大表再去过滤-不满足小表里面的这些条件的。

​	如果使用in的话，查询就是相反的，首先会先去执行in里面的这个查询语句得到一个条件，得到一个数据集比如是id的这个集合，然后在查询goods的时候会去走这样的一个比较小范围的查询，那么这个时候就是*小数据集驱动大数据集的策略*！

**注**：使用in的时候要考虑in里面的字段会不会太多，如果很多你就需要分批查询，避免这个返回数据量过大造成程序的这个内存溢出。

**不成文规定：**一条SQL只会同时操作一张表然后得到数据带入去查询另外一张表！-走单表查询

## 3. 批量插入代替循环插入

```mysql
# 循环插入
# for (Order order : list){
# 	orderMapper.insert(order)
# }
# 批量插入
# orderMapper.insertBatch(list)
insert into table(name,id) values ('name1',1),('name2',1)
```

存在的问题：

​	循环插入的性能是比较低的，*会多次和数据库做一个交互*。而使用mybatis的批处理呢原子性并且只交互一次，性能较好！



## 4.用limit限制返回条数，凡是list、query接口必须分页，避免内存溢出。

```mysql
select id,name from table order by id desc limit 10;
```

存在的问题：必须得有分页，有size才可以避免内存溢出。比如说调用接口时最多不能返回2000条，最多不能返回1000条。这样子操作会让你的程序*有一定的稳定性*，不至于说某一天某一个条件触发了一个非常大的查询导致整个程序就挂了！

## 5. 同步数据使用updated_at检索出增量数据，定时任务run 1次/2min，避免全表扫描

```mysql
select id,name from table where updated_at >(now()-2min)
```

理由：

​	在同步数据时候是以updated_at来检索出增量的数据，然后定时任务再去跑的时候是可以以非常小的一个时间频率去跑这个增量数据的一个更新的，这样操作*避免了全表的一个扫描*，这样操作性能会得到较大的提升！

https://blog.csdn.net/chushoufengli/article/details/102662658

## 6. 分页查询优化

```mysql
select id,name from goods limit 1,30
------------------------------------------------------
# 随着pageNo不断增大，查询效率变低
# int lastMaxId=100; (前端传递)
select id,name from goods
where id >100 limit 1,30
```

存在的问题：

​	随着数据条数的增加往后的分页效率是比较低的，那么我们可以借助上次查询的order by最大的那个值或者最小值来作为一个条件的过滤。 分页数不变直接获取的是当前第一页的数据！



## 7. 缩小数据集的条件放前面

```mysql
select id, name from goods
where is_deleted=0 and wechat_id=1 and discount> 20
```

理由：

​	当前条件是否删除在前面，可是在全表中都具有这样的条件，按照优化来讲可以把 wechat_id放在前面因为wechat_id可以缩小范围比如可以缩小在30-40条，然后再用Is_deleted来过滤没有删除的。其实主要是表现为过滤的数据集是要小很多的，再不断的往后缩减的过程中那么这个效率也是非常明显的！