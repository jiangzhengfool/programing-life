

# spark介绍

伯克利APMlab实验室，力图在算法（Algorithms） 、机器 （Machines） 、人（People） 之间通过大规模集成，来展现大数据应用的一个平台，核心引擎Spark

计算基础是弹性分布式数据集（RDD）

对海量不透明的数据进行甄别并转化为有用的信息

Spark涉及到机器学习、数据挖掘、数据库、信息检索、自然语言处理和语音识别等等

Apache Spark 提供了内存中的分布式计算能力，具有Java、 Scala、 Python、R四种编程语言的API编程接口

大部分代码使用 Scala语言编写

构建在Spark上的应用包括：Qlik、Talen、Tresata、atscale、platfora……

使用Spark的公司有： Verizon Verizon、 NBC、Yahoo、 Spotify......

# Spark组件

Spark生态圈以Spark为核心引擎，以HDFS、S3、Techyon为持久层读写原生数据，以 Mesos、YARN和自身携带的Standalone作为资源管理器调度job，来完成spark应用程序的计算；

## Spark Steaming

Spark Steaming是一个对实时数据进行高通量、容错处理的流式处理系统，可以对多种数据源（如Kdfka、Flume、Twitter、Zero和TCP 套接字）进行类似map、reduce、join、window等复杂操作，并将结果保存到外部文件系统、数据库或应用到实时仪表盘。

SparkStreaming流式处理系统特点有： 将流式计算分解成一系列短小的批处理作业 将失败或者执行较慢的任务在其它节点上并行执行，较强的容错能力(基于RDD继承关系Lineage) 使用和RDD一样的语义

## Spark SQL

Spark SQL是一个即席查询系统，可以通过SQL表达式、HiveQL或者Scala DSL在Spark 上执行查询。

DataFrame位于Spark SQL的核心，DataFrame将数据保存为行的集合，对应行中的各列都被命名，通过使用DataFrame，可以非常方便地查询、绘制和过滤数据

## MLlib

MLlib为Spark中的机器学习框架

## Graphx

Graphx为图计算框架，提供结构化数据的图计算能力

# 与hadoop的结合

## Hadoop

## Spark与Hadoop的对比

Hadoop实质上更多是一个分布式数据基础设施: 它将巨大的数据集分派到 一个由普通计算机组成的集群中的多个节点进行存储，意味着您不需要购买和维护 昂贵的服务器硬件。Hadoop还会索引和跟踪这些数据，让大数据处理和分析效率达到前所 未有的高度。

Spark则是那么一个专门用来对那些分布式存储的大数据进行处理 的工具，它并不会进行分布式数据的存储。

Hadoop还提供了叫做MapReduce的数据处理功能。

Spark没有提供文件管理系统，它必须和其他的分布式文件系统进行集 成才能运作。 这里我们可以选择Hadoop的HDFS,也可以选择其他的基于云的数据系统平台。但 Spark默认来说还是被用在Hadoop上面的，毕 竟，大家都认为它们的结合是最好 的。

Spark基于map reduce算法实现的分布式计算，拥有Hadoop MapReduce所具有的优点；但不同于MapReduce的是Job中间输出和结果可以保存在内存中，从而不再需要读写HDFS，因此Spark能更好地适用于数据挖掘与机器学习等需要迭代的map reduce的算法 和Hadoop MapRedeuce相比:更好的容错性和内存计算 高速，在内存中运算100倍速度于MapReduce 易用，相同的应用程序代码量要比MapReduce少2-5倍 提供了丰富的API 支持互动和迭代程序 Spark大数据平台之所以能日渐红火，得益于Spark内核架构的优秀：提供了支持DAG图的分布式并行计算框架，减少多次计算之间中间结果IO开销 提供Cache机制来支持多次迭代计算或者数据共享，减少IO开销 RDD之间维护了血统关系，一旦RDD fail掉了，能通过父RDD自动重建，保证了容错性。

RDD Partition可以就近读取分布式文件系统中的数据块到各个节点内存中进行计算

使用多线程池模型来减少task启动开稍

shuffle过程中避免不必要的sort操作

采用容错的、高可伸缩性的akka作为通讯框架

# hadoop生态

# spark应用场景

*   用户画像的建立
*   用户异常以为的发现
*   社交网络关系洞察
*   用户定向商品/活动推荐

实时的市场活动，在线产品推荐，网络安全分析，机器日记监控等

Spark适用场景：

1.  Spark是基于内存的迭代计算框架，适用于需要多次操作特定数据集的应用场合。需要反复操作的次数越多，所需读取的数据量越大，受益越大，数据量小但是计算密集度较大的场合，受益就相对较小。
2.  由于RDD的特性，Spark不适用那种异步细粒度更新状态的应用，例如web服务的存储或者是增量的web爬虫和索引。就是对于那种增量修改的应用模型不适合。
3.  数据量不是特别大，但是要求近实时统计分析需求

Spark不适用场景：

1.  内存hold不住的场景，在内存不足的情况下，Spark会下放到磁盘，会降低应有的性能
2.  有高实时性要求的流式计算业务，例如实时性要求毫秒级
3.  由于RDD设计上的只读特点，所以Spark对于待分析数据频繁变动的情景很难做（并不是不可以） ，比如题主例子里的搜索，假设你的数据集在频繁变化（不停增删改） ，而且又需要结果具有很强的一致性（不一致时间窗口很小） ，那么就不合适了。
4.  流线长或文件流量非常大的数据集不适合。你会发现你的内存不够用，集群压力大时一旦一个task失败会导致他前面一条线所有的前置任务全部重跑，然后恶性循环会导致更多的task失败，整个sparkapp效率极低。就不如MapReduce啦！

