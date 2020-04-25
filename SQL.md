#### DDL(数据定义语言)

CREATE：创建数据库和表对象

DROP：删除数据库和表对象

ALTER：修改数据库和表对象

#### DML(数据操纵语言)

SELECT：查询表中的数据

INSERT：向表中插入新数据

UPDATE：更新表中的数据

DELETE：删除表中的数据

#### DCL(数据控制语言)

COMMIT：确认对数据库中的数据进行的变更

ROLLBACK：取消对数据库中的数据进行的变更

GRANT：赋予用户操作权限

REVOKE：取消用户的操作权限

#### 数据库的创建

CREATE DATABASE product；

#### 表的创建

CREATE TABLE product;

#### 表的删除

DROP TABLE product;

#### 数据的删除

DELETE FROM product

#### 表的更新

ALTER TABLE product ADD COLUMN <列的定义>;

ALTER TABLE product DROP COLUMN <列名>;

#### 数据的更新

UPDATE product SET <列名> = <表达式>

#### 向表中插入数据

INSERT INTO product (列1，列2，列3）VALUES (值1，值2，值3);

#### 从其他表中复制数据

INSERT INTO productcopy (列1，列2， 列3)

SELECT (列1，列2，列3) FROM product

#### 事务

- 需要在同一个处理单元中执行的一系列更新处理的集合

  - BEGIN TRANSACTION;

  - COMMIT;/ROLLBACK;

- 每条SQL语句就是一个事务

- 直到用户执行COMMIT或者ROLLBACK为止算作一个事务

- ACID特性：

  - 原子性：在事务结束时，其中所包含的更新处理要么全部执行，要么完全不执行
  - 一致性：事务中包含的处理要满足数据库提前设置的约束
  - 隔离性：保证不同事务之间互不干扰的特性
  - 持久性：事务结束后，DBMS能够保证该时间点的数据状态会被保存的特性

#### 视图

- 创建视图
  - CREATE VIEW 视图名称 (<视图列名1>，<视图列名2>)
  - AS
  - <SELECT语句>
- 删除视图
  - DROP VIEW productview



#### 基础知识

- SQL语句中使用字符串或者日期常数时，必须使用单引号('')将其括起来，而中文别名使用双引号("")

- 删除重复行：DISTINCT

- 单行注释：--  
- 多行注释：/* */
- 判断是否为NULL：IS NULL  、IS NOT NULL
- 聚合函数：
  - COUNT、SUM、AVG、MAX、MIN
  - SUM/AVG只能对数值类型的列使用
  - MAX/MIN可以适用于任何数据类型的列
- 使用GROUP BY语句时，SELECT子句中只能存在三种元素：常数、聚合函数、GROUP BY子句中指定的列名
- GROUP BY子句结果的显示是无序的
- HAVING子句中能够使用的3要素：常数、聚合函数、GROUP BY子句中指定的列名
- 在ORDER BY子句后使用关键字ASC进行升序排序，使用DESC关键字进行降序排序，默认使用升序排序
- LIKE
  - 前方一致 LIKE ‘ddd%’
  - 中间一致 LIKE '%ddd%'
  - 后方一致 LIKE ‘%ddd’
- BETWEEN
  - 包含临界值



#### CHAR和VARCHAR的区别

- CHAR是固定长度的类型，CHAR(8)假如输出’abc‘，不足用半角空格补足
- VARCHAR是可变长度类型，VARCHAR(8)输入‘abc’，那么就是‘abc’



#### DROP、DELETE与TRUNCATE的区别

| DROP             | TRUNCATE    |            DELETE             |
| ---------------- | ----------- | :---------------------------: |
| DDL              | DDL         |              DML              |
| 不可回滚         | 不可回滚    |            可回滚             |
| 不可带where      | 不可带where |           可带where           |
| 表内容和结构删除 | 表内容删除  |          表内容删除           |
| 删除速度快       | 删除速度快  | 删除速度慢，根据where逐行删除 |



#### 索引

CREATE INDEX index_name 

ON table_name (column_name)

是一种快速查询表中内容的机制，类似于新华字典的目录，运用在表中某个些字段上，但存储时，独立于表之外

- 什么时候**要**创建索引
  1. 表经常进行SELECT操作
  2. 表很大（记录超多），记录内容分布范围很广
  3. 列名经常在WHERE子句或连接条件中出现
- 什么时候**不要**创建索引
  1. 表经常进行INSERT/UPDATE/DELETE操作
  2. 表很小（记录超小）
  3. 列名不经常作为连接条件或出现在WHERE子句中
- 索引优缺点
  - 索引加快数据库的检索速度
  - 索引降低了插入、删除、修改等维护任务的速度（大部分数据更新需要同时更新索引）
  - 唯一索引可以确保每一行数据的唯一性，通过使用索引，可以在查询的过程中使用优化隐藏器，提高系统的性能
  - 索引需要占用物理和数据空间



#### 联合索引

​		在mysql中建立联合索引时会遵循最左前缀匹配的原则，即最左有限，在检索数据时从联合索引的最左边开始匹配，对列col1、col2、col3建一个联合索引，实际上建立了（col1）、（col1，col2）、（col1，col2，col3）三个索引

- 减少开销

  ​		每多一个索引，都会增加写操作的开销和磁盘空间的开销。对于大量数据的表，使用联合索引会大大的减少开销

- 覆盖索引

  ​		覆盖索引是一种索引策略：当某一查询中包含的所需字段皆包含于一个索引中，此实索引将大大提高查询性能。

- 效率高

  ​		索引列越多，通过索引筛选出的数据越少。



#### 视图

CREATE VIEW view_name AS
SELECT column_name(s)
FROM table_name
WHERE condition

1. 视图是一种虚表
2. 试图建立在已有表的基础上，视图赖以建立的这些表成为基表
3. **向视图提供数据内容的语句为SELECT语句，可以将试图理解为存储起来的SELECT语句**
4. 视图向用户提供基表数据的另一种表现形式
5. 视图没有存储真正的数据，真正的数据还是存储在基表中
6. 程序员虽然操作的是视图，但最终视图还是会转成操作基表
7. 一个基表可以有0个或者多个视图

好处：

1. 视图无需保存数据，因此可以节省存储设备的容量
2. 可以将频繁使用的SELECT语句保存成视图，这样就不用每次都重新书写了，视图中的数据会随着原表的变化自动更新



#### 子查询



#### 多表联查



#### ACID

- 原子性 Atomicity

  - 原子性是指事务是一个不可再分割的工作单位，事务中的操作要么都发生，要么都不发生
  - 保障事务的原子性是数据库管理系统的责任，为此许多数据源采用日志机制

- 一致性 Consistency

  - 一致性是指在事务开始之前和事务结束以后，数据库的完整性约束没有被破坏。这是说数据库事务不能破坏关系数据的完整性以及业务逻辑上的一致性
  - 在一个事务执行之前和之后，数据会符合你设置的约束（唯一约束，外键约束，Check约束等）和触发器设置。这一点是由SQL SERVER进行保证的

- 隔离性 Isolation

  - 多个事务并发访问时，事物之间是隔离的，一个事务不应该影响其他事务运行效果
  - 当多个事务并发时，SQL SERVER利用加锁和阻塞来保证事务之间不同等级的隔离性。一般情况下，完全的隔离性是不现实的，完全的隔离性要求数据库同一时间只执行一条事务，这样会严重影响性能。
  - 事务之间的相互影响分为几种：
    - 脏读：一个事务读取了另一个事务未提交的数据，而这个数据有可能回滚的
    - 不可重复读：在数据库访问中，一个事务范围内两个相同的查询却返回了不同数据。这是由于查询时系统中其他事务修改的提交而引起的
    - 幻读：是指当事务不是独立执行时发生的一种现象，例如第一个事务对一个表中的数据进行了修改，这种修改涉及到表中的全部数据行。同时，第二个事务也修改这个表中的数据，这种修改是向表中插入一行新数据。那么，以后就会发生操作第一个事务的用户发现表中还有没有修改的数据行，就好象发生了幻觉一样。
    - 丢失更新：两个事务同时读取同一条记录，A先修改记录，B也修改记录（B是不知道A修改过），B提交数据后B的修改结果覆盖了A的修改结果
  - 隔离级别
    - 未提交读 Read Uncommited 什么都避免不了
    - 已提交读 Read Commited 可避免脏读
    - 可重复读 Repeatable Read 可避免脏读、不可重复读
    - 可串行读 Serializable 可避免脏读、不可重复读、幻读

- 持久性 Durability

  - 在事务完成以后，该事务对数据库所做的更改便持久的保存在数据库之中，并不会被回滚。
  - SQL SERVER通过write-ahead transaction log来保证持久性。write-ahead transaction log的意思是，事务中对数据库的改变在写入到数据库之前，首先写入到事务日志中。而事务日志是按照顺序排号的（LSN）。当数据库崩溃或者服务器断点时，重启动SQL SERVER，SQLSERVER首先会检查日志顺序号，将本应对数据库做更改而未做的部分持久化到数据库，从而保证了持久性。

- 总结

  ​		事务的（ACID）特性是由关系数据库管理系统（RDBMS，数据库系统）来实现的。数据库管理系统采用日志来保证事务的原子性、一致性和持久性。日志记录了事务对数据库所做的更新，如果某个事务在执行过程中发生错误，就可以根据日志，撤销事务对数据库已做的更新，使数据库退回到执行事务前的初始状态。
  　　数据库管理系统采用锁机制来实现事务的隔离性。当多个事务同时更新数据库中相同的数据时，只允许持有锁的事务能更新该数据，其他事务必须等待，直到前一个事务释放了锁，其他事务才有机会更新该数据。



#### 悲观锁和乐观锁

- 悲观锁

  ​		在关系数据库管理系统里，悲观并发控制（又名“悲观锁”，Pessimistic Concurrency Control，缩写“PCC”）是一种并发控制的方法。它可以阻止一个事务以影响其他用户的方式来修改数据。**如果一个事务执行的操作都某行数据应用了锁，那只有当这个事务把锁释放，其他事务才能够执行与该锁冲突的操作**。悲观并发控制主要用于数据争用激烈的环境，以及发生并发冲突时使用锁保护数据的成本要低于回滚事务的成本的环境中。

  ​		悲观锁，正如其名，它指的是对数据被外界（包括本系统当前的其他事务，以及来自外部系统的事务处理）修改持保守态度(悲观)，因此，在整个数据处理过程中，将数据处于锁定状态。 悲观锁的实现，往往**依靠数据库提供的锁机制**（也只有数据库层提供的锁机制才能真正保证数据访问的排他性，否则，即使在本系统中实现了加锁机制，也无法保证外部系统不会修改数据）。

  ​		悲观并发控制实际上是“先取锁再访问”的保守策略，为数据处理的安全提供了保证。但是**在效率方面，处理加锁的机制会让数据库产生额外的开销，还有增加产生死锁的机会**；另外，在只读型事务处理中由于不会产生冲突，也没必要使用锁，这样做只能增加系统负载；还有会降低了并行性，一个事务如果锁定了某行数据，其他事务就必须等待该事务处理完才可以处理那行数。

- 乐观锁

  ​		在关系数据库管理系统里，乐观并发控制（又名“乐观锁”，Optimistic Concurrency Control，缩写“OCC”）是一种并发控制的方法。**它假设多用户并发的事务在处理时不会彼此互相影响，各事务能够在不产生锁的情况下处理各自影响的那部分数据。在提交数据更新之前，每个事务会先检查在该事务读取数据后，有没有其他事务又修改了该数据。如果其他事务有更新的话，正在提交的事务会进行回滚**。

  ​		乐观锁（ Optimistic Locking ） 相对悲观锁而言，乐观锁假设认为数据一般情况下不会造成冲突，所以在数据进行提交更新的时候，才会正式对数据的冲突与否进行检测，如果发现冲突了，则让返回用户错误的信息，让用户决定如何去做。

  ​		相对于悲观锁，在对数据库进行处理的时候，乐观锁并不会使用数据库提供的锁机制。一般的实现乐观锁的方式就是记录数据版本。数据版本,为数据增加的一个版本标识。当读取数据时，将版本标识的值一同读出，数据每更新一次，同时对版本标识进行更新。当我们提交更新的时候，判断数据库表对应记录的当前版本信息与第一次取出来的版本标识进行比对，如果数据库表当前版本号与第一次取出来的版本标识值相等，则予以更新，否则认为是过期数据。

  ​		**实现数据版本有两种方式，第一种是使用版本号，第二种是使用时间戳。**

  ​		乐观并发控制相信事务之间的数据竞争(data race)的概率是比较小的，因此尽可能直接做下去，直到提交的时候才去锁定，所以不会产生任何锁和死锁。但如果直接简单这么做，还是有可能会遇到不可预期的结果，例如两个事务都读取了数据库的某一行，经过修改以后写回数据库，这时就遇到了问题。



#### 范式

应用数据库范式可以带来许多好处，主要有三点：

1. 减少数据冗余
2. 消除异常（插入异常，更新异常，删除异常）
3. 让数据库内的数据更好的组织，让磁盘空间得到更有效利用

- 第一范式

  每一个属性都不可再分。不符合第一范式则不能称为关系数据库

- 第二范式

  表中的属性必须完全依赖于全部主键，而不是部分主键。所以只有一个主键得表如果符合第一范式，那一定是第二范式

- 第三范式

  消除数据库中关键字之间的依赖关系

- BC范式（BCNF）

  ​		bc范式是在第三范式的基础上的一种特殊情况，既每个表中只有一个候选键（在一个数据库中每行的值都不相同，则可称为候选键）

- 第四范式

  ​		第四范式是消除表中的多值依赖，也就是说可以减少维护数据一致性的工作。

  

#### SQL优化

- 数据库的解析器按照从右到左的顺序处理表名，写在最后的表将被最先处理
  - FROM子句中选择记录条数最少的表或者被引用最多的表放在最后
  - WHERE子句中可以过滤掉最大数量记录的条件必须卸载WHERE子句的之右
- SELECT子句中避免使用*号
- 使用TRUNCATE代替DELETE
- 使用表或者列的别名
- 善用索引查询
- 多使用commit，会释放回滚点



#### 存储引擎

- Innodb引擎

  提供了对数据库ACID事务的支持，并且还提供了行级锁和外键的约束，它的设计的目标就是处理大数据容量的数据库系统。默认是REPEATABLE级别，可以防止脏读和非重复读。索引数据结构：B+数

- MyIASM引擎

  不提供事务的支持，也不支持行级锁和外键，但支持全文索引。索引数据结构：B+数

- MEMORY引擎

  所有的数据都保存在内存中，数据的处理速度快，但是安全性不高，更适合临时表这种应用场景。索引数据结构：哈希
