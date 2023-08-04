索引的实现：hash索引和b+树索引

Mysql索引：

单列索引：对表中得某一列建立索引，可以有多个单列索引

普通索引：没有要求，可以是空，可以是重复只是为了查询速率更快

唯一索引：

主键索引：

组合索引：最左原则

全文索引：最好是先建表，在写数据，再加索引，针对于模糊查询，where match 列名 against 参数，innodb的搜索引擎最小搜索长度4

空间索引：用的较少，必须not null，用的不是btree

# **创建索引：**

普通索引：

Creat index 索引名 on 表名(字段名)

Alert table 表名 add Index 索引名(字段名)

唯一索引：

Creat unique index 索引名 on 表名(字段名)

Alert table 表明 add unique 索引名(字段名)

# **查看索引**

Show index from 表

 

删除索引

Drop index 索引名 on 表名

 

Alert table 表名 drop index 索引名

 

# **二叉树**

**特点：分为左子树，右子树和根节点，左子树的值要比根节点小，右子树要比根节点大**

**平衡二叉树=>****左子树和右子树深度只差不超过1**

 

**索引的优点**

**·大大加快数据的查询速度**

**·使用分组和排序进行数据查询时，可以显著减少查询时分组和排序的时间**

**·创建唯一索引，能够保证数据库表中每一行数据的唯一性**

**·在实现数据的参考完整性方面，可以加速表和表之间的链接**

**索引的缺点**

**·创建索引和维护索引需要消耗时间，并且随着数据的增加，时间也会增加**

**·索引需要占据次磁盘空间**

**·对数据表中的数据进行增加修改删除时，索引也要动态维护，降低了维护速度**

 

**创建索引的原则**

**·更新频繁的列不应设置索引**

**·数据量小的表不要使用索引（一共两页不需要目录）**

**·重复数据多的字段不应设为索引**

**·首先应该考虑对where****和order by****、groupby****涉及的列建立索引**

**什么情况下索引失效**

*****

**前缀%**

**在条件上使用运算**

**Is null** **或is not null**

**使用！=**

**字符串不加‘’**

**In**

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg)

 

# **MVCC基本概念**

当前读

读取得记录时记录得最想念版本，读取时还要保证其他并发事务不能修改当前记录，会对读取得记录加锁，比如：select … lock in share mode，select ..for update,update,insert,delete(排他锁)

快照读

简单的select（不加锁）就是快照读，读取的就是记录数据得可见版本，有可能是历史数据，不加锁，是非阻塞读

Read committed:每次select,都生成一个快照读

Repeatable Read:开启事务后第一个select语句才是快照读得地方，第一次select会产生一个快照，后面的select都会查看这个快照

Serializable:快照读会退化成当前读

 

MVCC得实现依赖于数据库得三个隐式字段，undo log，readview

DB_TRX_ID    最近修改事务ID，记录插入这条数据或者最后修改该记录得事务id

DB_ROLL_PTR  回滚指针，指向这条记录的上一个版本，用于配合undo log，指向上一个版本

DB_ROW_ID    隐藏主键，如果表结构没有指定主键，将会生成该隐藏字段

 

 

Readview是快照读sql执行时MVCC提取数据得一句，记录并维护系统当前活跃的事务id

ReadView中包含四个核心字段

M_ids 当前活跃的事务id集合

Min_trx_id 最小活跃事务id

Max_trx_id 预分配事务id

Creator_trx_id 生成readview的事务id

 

 

# Binlog格式

Statement        基于sql语句的日志记录，记录的是sql语句，对数据进行修改的sql都会记录在日志文件

Row                   基于行的日志记录，记录的是每一行的数据变更（默认）

Mixed               混合了statement和row两种格式，默认采用statement，在某些特殊情况下自动切换row

 

主从复制原理

![img](file:///C:/Users/hp/AppData/Local/Temp/msohtmlclip1/01/clip_image004.jpg)

\1.      master主库提交事务时，会把数据变更记录在二进制文件binlog中

\2.      从库读取主库的二进制binlog，写入从库中继日志RelayLog

\3.      Slave重做中继日志中的事件



# sql执行过程

解析、优化、执行、返回

## 如何判断这条语句是否用到索引

explain或desc查询这个sql的执行计划

## key_len越长索引效率越高？key_len越长说明命中索引的个数越多？

不一定
当走索引所需要的执行成本比扫描全表成本还要高时，就不会走索引了
比如如果一张表里都姓张那么张%模糊查询不会走，但是张111111%会走，因为匹配条件更细了，

## buffer pool中有三个集合

lrulist，freelist，flushlist

### freelist

当查询一条数据时会将数据放到缓存页中，一页是16kb，那么freelist就是记录哪些缓存页是空白的，当mysql启动时，没有缓存
那么freelist中都是缓存页，如果需要插入缓存，那么缓存页从freelist中剔除，将该缓存页的控制块加入，用来指向缓存页

### flushlist

当修改一条数据时，因为数据和磁盘中的数据不一致，所以需要刷新到磁盘中，但是不能每修改一条数据就做一次IO处理，IO非常消耗性能
所以会将变动的缓存页的控制块加载到flushlist中，如果刷新磁盘之后，那么会将该控制块从flushlist剔除

### lrulist

当访问某一页时，这一页数据不在bufferpool中，那么会从磁盘中将这一页数据的控制块加载lrulist的头部，如果已经存在，那么会将控制块移到头部
当lrulist满了时，会从链表的尾部进行淘汰

全表扫描优化：如果全表扫描扫到老年区的时间间隔大于某个特定时间，默认1000ms时，那么不会将这个也的控制块挪到年轻区

young区优化：如果查询的页在年轻区的1/4之后，才会将该数据页挪到list的头部  