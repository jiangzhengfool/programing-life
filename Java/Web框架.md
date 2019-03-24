## Hibernate
### Hibernate与MyBatis对比
### 性能优化
- 将所有的集合属性配置设置为懒加载(使用延迟加载)Hibernate2.x默认是false, hibernate3.x默认是true（1+n）
- 在定义关联关系时,集合首选Set,如果集合中的实体存在重复,则选择List,数组性能最差
- 在一对多的双向关联中,一般将集合的inverse设置为true,让集合的多方维护关联关系
- HQL子句本身大小写无关,但是其中出现的类名和属性名必须注意大小写区分
- 对大数据量查询时,慎用list() 返回查询结果（hql）
- 在性能瓶颈的地方使用JDBC
- 使用双向关联在大型应用中,几乎所有的关联必须在查询中可以双向导航       
- 制定合理的缓存策略
- 采用合理的Session管理机制
- 尽量使用延迟加载特性
- 设定合理的批处理参数
- 如果可以, 选用UUID作为主键生成器
- 如果可以, 选用基于version的乐观锁替代悲观锁
- 数据库本身的优化(合理的索引, 缓存, 数据分区策略等)也会对持久层的性能带来可观的提升, 这些需要专业的DBA提供支持
- 1+N问题(一条sql语句能解决的问题用了很多条sql语句来实现) 1-->N 
### Query接口的list方法和iterate方法有什么区别？
### HIbernate-fetch抓取策略
### Session加载实体对象的步骤是：
- Session在调用数据库查询功能之前, 首先会在缓存中进行查询, 在一级缓存中, 通过实体类型和主键进行查找, 如果一级缓存查找命中且数据状态合法, 则直接返回
- 如果一级缓存没有命中, 接下来Session会在当前NonExists记录(相当于一个查询黑名单, 如果出现重复的无效查询可以迅速判断, 从而提升性能)中进行查找, 如果NonExists中存在同样的查询条件,则返回null
- 对于load方法, 如果一级缓存查询失败则查询二级缓存, 如果二级缓存命中则直接返回
- 如果之前的查询都未命中, 则发出SQL语句, 如果查询未发现对应记录则将此次查询添加到Session的NonExists中加以记录, 并返回null
- 根据映射配置和SQL语句得到ResultSet,并创建对应的实体对象
- 将对象纳入Session(一级缓存)管理
- 执行拦截器的onLoad方法(如果有对应的拦截器)
- 将数据对象纳入二级缓存
- 返回数据对象
### 注解详解
### [Hibernate的过程图](http://i.imgur.com/Zk0ddsR.png)
### hibernate的主配置文件常用属性
- [1](http://i.imgur.com/Cpc1plY.png)
- [2](http://i.imgur.com/OxNTEKK.png) 
### [Hibernate基本数据类型](http://i.imgur.com/juJBmIE.png)
### 常见的主键生成器
### 深入了解hibernate的会话工厂
- SessionFactory
- Session
- Transaction
### [Hibernate中可持久化对象的状态](http://i.imgur.com/n8h5P28.png)
### 表关系的配置
- one-to-one关联
- 单向many-to-one关联
- 双向one-to-many关联
- many-to-many关联
### Set元素的几个属性
### 注解配置关联关系
### 继承映射
### Hibernate三种查询方式的特点
### 一级缓存（ Session缓存）
### 二级缓存
- [各种二级缓存插件](http://i.imgur.com/jTF28J3.png)
### 查询缓存的使用
### [Hibernate的缓存机制详细分析](http://www.cnblogs.com/xiaoluo501395377/p/3377604.html)
### 缓存算法(缓存满了后，将内存中哪个对象清除)
### 锁
- 乐观锁Optimistic Locking 
- 悲观锁Pessimistic Locking
- 乐观锁和悲观锁比较
### Hibernate的优缺点
### Reference
- [官方文档](http://docs.jboss.org/hibernate/orm/current/)

## Spring
### Spring原理(IOC,AOP)
### 常用注解
### IOC
### AOP
- 通知Advice
- 顾问Advisor
- 自动代理生成器
- 切入点表达式
- AspectJ对AOP的实现
### DAO
- Spring与JDBC模板
### Spring的事件和监听器

## SpringMVC
### [Spring MVC的工作原理](https://www.jianguoyun.com/p/DcMtXTgQkJSpBhj0xC0)
### [SpringMVC拦截器-性能监控](https://www.jianguoyun.com/p/DT5AfG4QkJSpBhj_xC0)
### [笔记_SpringMVC(动力节点)](https://www.jianguoyun.com/p/DcpouQ4QkJSpBhiGxS0)

## MyBatis
### [MyBatis中文文档](http://www.mybatis.org/mybatis-3/zh/index.html)
### 参数传递
### MyBatis的缓存
### MyBatis中使用#和$书写占位符
### 表关系配置
### 多表查询
### 注解开发

## Spring Boot

## Spring Security


> [【译】什么是 web 框架？](https://www.cnblogs.com/hazir/p/what_is_web_framework.html)
> [你真的理解了MVC, MVP, MVVM吗？](http://zhuanlan.51cto.com/art/201803/568600.htm)
> [Java Web应用的代码分层最佳实践。](https://blog.csdn.net/hollis_chuang/article/details/80221780)
> [架构以及我理解中架构的本质](https://blog.csdn.net/bigtree_3721/article/details/51005836)
> [从兄弟到父子：动态代理在民间是怎么玩的？](http://www.sohu.com/a/202504543_465221)
> [如何打造一个全满分网站](https://segmentfault.com/a/1190000011867361)
> [自己动手实现JDK动态代理](https://www.cnblogs.com/rjzheng/p/8750265.html)