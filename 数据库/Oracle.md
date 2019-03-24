## 安装及配置
## TNS
## 运算符
## 序列
## 与MySQL的不同之处
## 在oracle中varchar和varchar2有什么区别
## Oracle  数据库的6个索引

## PLSQL编程(基本语法 流程控制 循环 异常处理)
## 游标
## 函数(数学 日常 转化函数)
## 存储过程
## 触发器

1. Oracle的存储过程和其返回值？
2. 在ORACLE大数据量下的分页解决方法。
3. 组成Oracle 的三个物理文件  数据  日志 参数
4. Oracle 中类次 like ％5400％ 检索比 like x5400％ or A5400％效率低下，为什么？
因为模糊查询的关键字如果第一个字符是通配符“-”或者是“%”，即要进行全表扫描，而后两者的检索可以利用索引进行范围查询
5. 数据库切换日志的时候，为什么一定要发生检查点？这个检查点有什么意义？
6.  表空间的管理方式有哪几种，各有什么优劣。
7. 本地索引与全局索引的差别与适用情况。
8. 一个表a varchar(2)，b number(1)，c char(2)，有100000条记录，创建B-TREE索引在字段Ａ上，那么表与索引谁大？为什么？
9. 执行计划是什么，查看执行计划一般有哪几中方式。
10. 简单描述一下nest loop 与 hash join 的差别
11. db file sequential read 与 db file scattered read 等待的差别，如果以上等待比较多，证明了什么问题？
12. library cache pin 与 library cache lock 是什么地方的等待事件，一般说明什么问题？
13. 在一个24x7的应用上，需要把一个访问量很大的1000万以上的数据级别的表的普通索引(a,b)修改成唯一约束(a,b,c)，你一般会选择怎么做，请说出具体的操作步骤与语句。
14. 如果一个linux上的oracle数据库系统突然变慢，你一般从哪里去查找原因。
15. 列举几种表连接方式
   等连接、非等连接、自连接、外连接（左、右、全）
16. 不借助第三方工具，怎样查看sql的执行计划
~~~
I) 使用Explain Plan,查询PLAN_TABLE;
   EXPLAIN   PLAN
      SET STATEMENT_ID='QUERY1'
      FOR
      SELECT *
      FROM a
      WHERE aa=1;
   SELECT    operation, options, object_name, object_type, ID, parent_id
       FROM plan_table
      WHERE STATEMENT_ID = 'QUERY1'
   ORDER BY ID;

II)SQLPLUS中的SET TRACE 即可看到Execution Plan Statistics
   SET AUTOTRACE ON;
~~~
17. 如何使用CBO,CBO与RULE的区别
18. 如何定位重要(消耗资源多)的SQL
19. 如何跟踪某个session的SQL
20. SQL调整最关注的是什么
   检查系统的I/O问题
   sar－d能检查整个系统的iostat（IO statistics）
21. 绑定变量是什么？绑定变量有什么优缺点？
绑定变量是指在SQL语句中使用变量，改变变量的值来改变SQL语句的执行结果。
优点：使用绑定变量，可以减少SQL语句的解析，能减少数据库引擎消耗在SQL语句解析上的资源。提高了编程效率和可靠性。减少访问数据库的次数, 就能实际上减少ORACLE的工作量。
缺点：经常需要使用动态SQL的写法，由于参数的不同，可能SQL的执行效率不同；
22. 如何稳定(固定)执行计划
可以在SQL语句中指定执行计划。使用HINTS;
23. 和排序相关的内存在8i和9i分别怎样调整，临时表空间的作用是什么
SORT_AREA_SIZE 在进行排序操作时，如果排序的内容太多，内存里不能全部放下，则需要进行外部排序，
此时需要利用临时表空间来存放排序的中间结果。
24. Pctused and pctfree 表示什么含义有什么作用
pctused与pctfree控制数据块是否出现在freelist中,
pctfree控制数据块中保留用于update的空间,当数据块中的free space小于pctfree设置的空间时,
该数据块从freelist中去掉,当块由于dml操作free space大于pct_used设置的空间时,该数据库块将
被添加在freelist链表中。
25. 简单描述tablespace / segment / extent / block之间的关系
tablespace    : 一个数据库划分为一个或多个逻辑单位，该逻辑单位成为表空间;每一个表空间可能包含一个或多个 Segment;
Segments      : Segment指在tablespace中为特定逻辑存储结构分配的空间。每一个段是由一个或多个extent组成。包括数据段、索引段、回滚段和临时段。
Extents       : 一个 extent 由一系列连续的 Oracle blocks组成.ORACLE为通过extent 来给segment分配空间。
Data Blocks   ：Oracle 数据库最小的I/O存储单位，一个data block对应一个或多个分配给data file的操作系统块。
26. 描述tablespace和datafile之间的关系
      一个表空间可包含一个或多个数据文件。
      表空间利用增加或扩展数据文件扩大表空间，表空间的大小为组成该表空间的数据文件大小的和。
      一个datafile只能属于一个表空间;
27. 本地管理表空间和字典管理表空间的特点，ASSM有什么特点
本地管理表空间：（9i默认）
空闲块列表存储在表空间的数据文件头。
特点：减少数据字典表的竞争，当分配和收缩空间时会产生回滚，不需要合并
字典管理的表空间：（8i默认）
空闲块列表存储在数据库中的字典表里.
特点：片由数据字典管理，可能造成字典表的争用。存储在表空间的每一个段都会有不同的存储字句，需要合并相邻的块;
28. 回滚段的作用是什么
   回滚段用于保存数据修改前的映象，这些信息用于生成读一致性数据库信息、在数据库恢复和Rollback时使用。一个事务只能使用一个回滚段。
29. 日志的作用是什么
日志文件（Log File）记录所有对数据库数据的修改，主要是保护数据库以防止故障,以及恢复数据时使用。其特点如下：
a)每一个数据库至少包含两个日志文件组。每个日志文件组至少包含两个日志文件成员。
b)日志文件组以循环方式进行写操作。
c)每一个日志文件成员对应一个物理文件。
30. SGA主要有那些部分，主要作用是什么
系统全局区（SGA）:是ORACLE为实例分配的一组共享缓冲存储区，用于存放数据库数据和控制信息，以实现对数据库数据的管理和操作。
SGA主要包括:
a)共享池(shared pool) ：用来存储最近执行的SQL语句和最近使用的数据字典的数据。
b)数据缓冲区 (database buffer cache)：用来存储最近从数据文件中读写过的数据。
c)重作日志缓冲区（redo log buffer）：用来记录服务或后台进程对数据库的操作。
另外在SGA中还有两个可选的内存结构：
d)Java pool:   用来存储Java代码。
e)Large pool: 用来存储不与SQL直接相关的大型内存结构。备份、恢复使用。
31. Oracle系统进程主要有哪些，作用是什么
数据写进程(DBWR)：负责将更改的数据从数据库缓冲区高速缓存写入数据文件
日志写进程(LGWR)：将重做日志缓冲区中的更改写入在线重做日志文件
系统监控   (SMON): 检查数据库的一致性如有必要还会在数据库打开时启动数据库的恢复
进程监控   (PMON): 负责在一个Oracle 进程失败时清理资源
检查点进程(CKPT)：负责在每当缓冲区高速缓存中的更改永久地记录在数据库中时,更新控制文件和数据文件中的数据库状态信息。
归档进程   (ARCH)：在每次日志切换时把已满的日志组进行备份或归档
恢复进程   (RECO): 保证分布式事务的一致性,在分布式事务中,要么同时commit,要么同时rollback;
作业调度器(CJQ ):   负责将调度与执行系统中已定义好的job,完成一些预定义的工作.
32. 归档是什么含义
归档是归档当前的联机redo日志文件。
SVRMGR> alter system archive log current;
数据库只有运行在ARCHIVELOG模式下，并且能够进行自动归档，才可以进行联机备份。有了联机备份才有可能进行完全恢复。
