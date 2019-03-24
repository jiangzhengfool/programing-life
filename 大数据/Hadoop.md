### 需要的基础
- 编程思维
- Linux操作基础,会Shell编程
- 网络基础,c/s
- Java,Python
### 发展方向
- 云计算大数据
- 自然语言处理
- 搜索引擎
- 数据挖掘
- 机器学习
- 推荐系统
- 广告系统

# 大数据
### hadoop环境的搭建
### HDFS文件系统
##### HDFS特点
- 分布式存储超大文件,块的概念
- 一次写入,多次读取
- 普通廉价的机器上
##### 不适用HDFS场景
- 低延迟
- 大量小文件
- 多用户更新
- 结构化数据
- 数据量并不大
##### HDFS体系架构
- 主/从（Master/Slave）体系架构
- NameNode,一个,中心服务器,管理存储和检索元数据
- DataNode,多个,数据存储
##### 副本管理策略
##### HDFS 读取和写入流程
- 读或写文件的过程,原理性的东西,很重要
##### 操作 HDFS 的基本命令(与Linux的文件操作基本类似)
- ls
- put,copyFromLocal
- get,copyToLcal
- cp
- mv
- rm
- cat,tail
- touchz
- mkdir
- du
### MapReduce计算框架
##### MapReduce的流程
##### MapReduce的原理
##### MapReduce的错误处理机制

### Zookeeper
应用于分布式应用的协作服务

### HBase
### Hive
### Strom

### 数据挖掘--推荐算法