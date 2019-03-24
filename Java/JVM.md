## 内存泄漏
[关于Java内存泄漏](https://github.com/scalad/Note/tree/master/Java_Memory)

- [JVM内存模型](http://www.jianshu.com/p/a60d6ef0771b)
- [35 个 Java 代码性能优化总结](http://www.open-open.com/lib/view/open1487924479608.html)
- 垃圾回收机制
- 类加载器
- Java虚拟机
- [其他_java中会存在内存泄漏吗，请简单描述](http://i.imgur.com/e7gyBKP.png)

JVM内存结构	堆、栈、方法区、直接内存、堆和栈区别

Java内存模型	内存可见性、重排序、顺序一致性、volatile、锁、final

垃圾回收	内存分配策略、垃圾收集器（G1）、GC算法、GC参数、对象存活的判定 

JVM参数及调优	

Java对象模型	oop-klass、对象头

HotSpot	即时编译器、编译优化

类加载机制	classLoader、类加载过程、双亲委派（破坏双亲委派）、模块化（jboss modules、osgi、jigsaw）

虚拟机性能监控与故障处理工具	jps, jstack, jmap、jstat, jconsole, jinfo, jhat, javap, btrace，TProfiler


自己编写各种outofmemory，stackoverflow程序	

HeapOutOfMemory、 Young OutOfMemory、MethodArea OutOfMemory、ConstantPool OutOfMemory、DirectMemory OutOfMemory、Stack OutOfMemory Stack OverFlow

*   JVM堆的基本结构。
*   JVM的垃圾算法有哪几种？CMS垃圾回收的基本流程？
*   JVM有哪些常用启动参数可以调整，描述几个？
*   如何查看JVM的内存使用情况？
*   Java程序是否会内存溢出，内存泄露情况发生？举几个例子。
*   你常用的JVM配置和调优参数都有哪些？分别什么作用？
*   JVM的内存结构？
*   常用的GC策略，什么时候会触发YGC，什么时候触发FGC？





JVM
	虚拟机实现
		HotSpot
		J9
		JRockit
	类加载机制
		双亲委派模型
		OSGI
	运行时内存组成
	内存模型
		主内存&工作内存
	GC原理&调优
		垃圾回收器
			ParNEW
			CMS
			G1
			Parallell GC
	性能调优/监测
		jmap
		jstack
		jcmd
		jconsole
		jinfo
		btrace




