## Spring 特点
* 非侵入式：Spring框架的APi不会在业务逻辑上出现，即业务逻辑是POJO。由于业务逻辑中没有Spring的API，所以业务逻辑可以从Spring框架快速移植到其他框架，即与环境无关。
* 容器：Spring作为一个容器，可以管理对象的生命周期、对象与对象之间的依赖关系。可以通过配置文件，来定义对象以及设置与其他对象的依赖关系。
* IoC:
* AOP

## Spring原理(IOC,AOP)

## 常用注解

## IOC（控制反转，Inversion of Control）
IoC的两种实现：依赖注入、依赖查找
* 依赖查找：Dependency Lookup，DL，容器提供回调接口和上下文环境给组件，程序代码则需要提供具体的查找方式。（依赖于JNDI系统的查找）
* 依赖注入：Depenency Injection，DI，程序代码不做定位查询，这些工作交由容器自行完成

依赖注入是目前最优先的解耦方式
#### 基于XML的DI
注入分类：
* 设值注入
![](../images/screenshot_1530460128135.png)

* 构造注入(几乎不用)  (这里,加了带参构造器后,无参构造器不执行)
![](../images/screenshot_1530460148084.png)

* 	命名空间注入(了解)
p命名空间设值注入
![](../images/screenshot_1530460167249.png)
c命名空间构造注入
![](../images/screenshot_1530460176369.png)

集合属性注入
![](../images/screenshot_1530460219209.png)

对于域属性的自动注入
* byName方式自动注入
![](../images/screenshot_1530460291164.png) 
* byType方式自动注入
![](../images/screenshot_1530460294929.png)

* 使用SPEL注入
![](../images/screenshot_1530460327151.png)

使用内部Bean注入
			 ![](../images/screenshot_1530460361369.png)
		使用同类抽象Bean注入
			 ![](../images/screenshot_1530460365500.png)
		使用异类抽象注入
			 ![](../images/screenshot_1530460368954.png)
		为应用指定多个Spring配置文件
			平等关系的配置文件
				 ![](../images/screenshot_1530460372286.png)
			平等关系的配置文件
	 ![](../images/screenshot_1530460376363.png)

#### 基于注解的DI
![](../images/screenshot_1530460395748.png)
使用@Resource为域属性自动注入值
			 ![](../images/screenshot_1530460417401.png)
		使用@Autowired为域属性自动注入值
			  ![](../images/screenshot_1530460421288.png)
              ![](../images/screenshot_1530460437125.png)
		Bean的声明周期始末注解
			 ![](../images/screenshot_1530460441953.png)
使用JavaConfig进行配置(了解)
	 ![](../images/screenshot_1530460447525.png)
使用Junit4测试Spring(了解)
	 ![](../images/screenshot_1530460451248.png)

注解与XML共同使用时，xml起作用

## Bean的装配
默认装配方式：调用Bean类的无参构造器，创建空值的实例对象

动态工厂Bean
![](../images/screenshot_1530459855742.png)

静态工厂Bean
![](../images/screenshot_1530459870476.png)

## Bean后处理器
容器中所有的Bean在初始化时,都会自动执行该类的两个方法
![](../images/screenshot_1530459954026.png)
定制Bean的生命始末
![](../images/screenshot_1530460001859.png)

## Bean的生命周期
![](../images/screenshot_1530460034754.png)


## AOP
![](../images/screenshot_1530460511700.png)

#### AOP编程术语
![](../images/screenshot_1530460561019.png)

#### 通知Advice
前置通知MethodBeforeAdvice
	 ![](../images/screenshot_1530460615744.png)
		 
后置通知AfterReturningAdvice
     ![](../images/screenshot_1530460619778.png)
环绕通知MethodInterceptor
     ![](../images/screenshot_1530460634613.png)	 
异常通知
     ![](../images/screenshot_1530460645852.png) 

#### 顾问Advisor
名称匹配方法切入点顾问
		 ![](../images/screenshot_1530460677882.png)
	正则表达式方法切入点顾问
		 ![](../images/screenshot_1530460680452.png)

#### 自动代理生成器
默认Advisor自动代理生成器
		 ![](../images/screenshot_1530460701166.png)
	Bean名称自动代理生成器
		 ![](../images/screenshot_1530460703716.png)
#### 通知的其他用法
	没有接口时自动使用CGLB代理
	有接口时使用CGLB代理
		![](../images/screenshot_1530460739482.png)

#### 切入点表达式
![](../images/screenshot_1530460808276.png)
#### AspectJ对AOP的实现(用的比较多)
举例
	  ![](../images/screenshot_1530460821201.png)
			 ![](../images/screenshot_1530460824342.png)
	
		 
		 
AspectJ基于注解的AOP实现
	前置通知
		 ![](../images/screenshot_1530460849206.png)
		 
	后置通知
		 ![](../images/screenshot_1530460860357.png)
		 
	环绕通知  
		 ![](../images/screenshot_1530460865185.png)
	异常通知 @AfterThrowing
		 ![](../images/screenshot_1530460875164.png)
	最终通知 @After
	定义切入点
		 ![](../images/screenshot_1530460879810.png)
AspectJ基于XML的AOP实现(用的比较多)
	 ![](../images/screenshot_1530460888051.png)


## DAO
#### Spring与JDBC模板
![](../images/screenshot_1530460989827.png)
增删改的实现
			 ![](../images/screenshot_1530460993191.png)
		基本类型结果查询的实现
				 ![](../images/screenshot_1530460996370.png)
		注册JdbcTemplate模板
			 ![](../images/screenshot_1530461000173.png)
		查询结果为自定义类型对象的实现
			 ![](../images/screenshot_1530461003015.png)
			 
			 ![](../images/screenshot_1530461013988.png)
		JdbcTemplate是不用注册的
			 ![](../images/screenshot_1530461018709.png)
		JDBC模板对象是多例的
			![](../images/screenshot_1530461021892.png)
#### 事务管理
Spring事务管理API
			事务管理器接口
				 ![](../images/screenshot_1530461094449.png)
				 
			事务定义接口
				 ![](../images/screenshot_1530461097731.png)
		程序举例环境搭建
			购买股票
		使用Spring的事务代理工厂管理事务
			 ![](../images/screenshot_1530461101425.png)
		使用Spring的事务注解管理事务
			 ![](../images/screenshot_1530461107091.png)
			 ![](../images/screenshot_1530461112494.png)
			 ![](../images/screenshot_1530461116135.png)
		使用Aspect的AOP配置管理事务(重点)
			 ![](../images/screenshot_1530461120021.png)
             
## Spring的事件和监听器

Spring Bean的初始化
Spring的AOP原理
自己实现Spring的IOC

Spring IOC 	1.bean注入 注解方式方便易读，引用第三方（数据库连接，数据库连接池，JedisPool等）采用配置文件方式。 
	2.bean作用域：Singleton，prototype，request，session，globalsession
	3.bean生命周期:如下图所示（图片来源于互联网）：
Spring AOP 	关注点、切面Aspect、切入点pointcut、连接点joinpoint、通知advice、织入weave、引入introduction。
	事务传播属性和隔离级别


 Spring MVC如何对RESTful风格提供支持？.ziw
 Spring MVC的工作原理是怎样的？.ziw
 Spring 框架中都用到了哪些设计模式？.ziw
 Spring中Bean的作用域有哪些？.ziw
 Spring中如何使用注解来配置Bean？有哪些相关的注解？.ziw
 Spring中的BeanFactory和ApplicationContext有什么联系？.ziw
 Spring中的自动装配有哪些限制？.ziw
 Spring中自动装配的方式有哪些？.ziw
 Spring支持的事务管理类型有哪些？你在项目中使用哪种方式？.ziw
 什么是spring的IOC AOP.ziw
 你如何理解AOP中的连接点（Joinpoint）、切点（Pointcut）、增强（Advice）、引.ziw
 你对Spring的理解。.ziw
 你是如何理解“横切关注”这个概念的？.ziw
 你的项目选择使用Spring框架的原因是什么？.ziw
 依赖注入时如何注入集合属性？.ziw
 依赖注入的方式以及你在项目中的选择？.ziw
 和自动装配相关的注解有哪些？.ziw
 在Web项目中如何获得Spring的IoC容器？.ziw
 如何使用HibernateDaoSupport整合Spring和Hibernate？.ziw
 如何在Spring IoC容器中配置数据源？.ziw
 如何在Web项目中配置Spring MVC？.ziw
 如何在Web项目中配置Spring的IoC容器？.ziw
 如何理解Spring AOP中Advice这个概念？.ziw
 如何配置配置事务增强？.ziw
 提供Spring IoC容器配置元数据的方式？.ziw
 请说出Spring 中依赖注入和AOP的实现机制.ziw
 请阐述一下你对“面向接口编程”的理解.ziw
 
 ## Spring 5
 响应式编程
 
  > [向Spring大佬低头——大量源码流出解析](https://cloud.tencent.com/developer/article/1177441)
 > [Spring、Spring MVC、MyBatis整合文件配置详解](http://www.cnblogs.com/wxisme/p/4924561.html)
 > [Spring知识点提炼](https://blog.csdn.net/u013256816/article/details/51386182)
 > [Spring AOP是什么?你都拿它做什么?](https://my.oschina.net/liughDevelop/blog/1457097)
 > [Spring AOP的实现原理](http://listenzhangbin.com/post/2016/09/spring-aop-cglib/)
 > [Java 必看的 Spring 知识汇总！](https://cloud.tencent.com/developer/article/1142534)