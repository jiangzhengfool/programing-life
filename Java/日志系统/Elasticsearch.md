# Elasticsearch 笔记

## 配置 Elasticsearch

#### configuration management tools
	Puppet	puppet-elasticsearch
	Chef	cookbook-elasticsearch
	Ansible	ansible-elasticsearch

#### system configuration file(/etc/sysconfig/elasticsearch)

#### Secure setting

#### Logging configuration

#### 重要的 Elasticsearch 配置
	path.data and path.logs
	cluster.name
	node.name
	bootstrap.memory_lock
	network.host
	discovery.zen.ping.unicast.hosts	# 单播,集群ip
	discovery.zen.minimum_master_nodes	# 防止脑震荡
	JVM heap dump path					# 堆的dump文件

#### 重要的 System 配置(产品化的必须配置)
- Set JVM heap size
- Disable swapping
- Increase file descriptors
- Ensure sufficient virtual memory
- Ensure sufficient threads

#### Bootstrap Checks

#### Upgrading Elasticsearch

## X-Pack

#### X-Pack 配置 SSL/TLS

## 6.0 的重大变化

## API 约定

#### 多索引

#### 索引名字中的日期支持

#### 特定符号的 URI encode

#### 通用选项
- pretty
- human=true
- Date Math
- Response Filtering (filter_path=..,..,..)
- flat_settings  默认是false
- error_trace

## Documents APIs

#### documents 的读写原理

#### Index API
- Automatic Index Creation
- version
- op_type
- routing

#### Get API
- _source

	GET twitter/tweet/0?_source=false
	GET twitter/tweet/0?_source_include=*.id&_source_exclude=entities
	GET twitter/tweet/0?_source=*.id,retweeted

#### Delete API
- _delete_by_query
- conflicts=proceed
- scroll_size
- GET _tasks?detailed=true&actions=*/delete/byquery
- POST _tasks/task_id:1/_cancel
- POST _delete_by_query/task_id:1/_rethrottle?requests_per_second=-1

## 疑问

#### 安装完成后,本地可以访问,但远程无法访问?
network.host:  0.0.0.0 (原因未知)

#### [POST 与 PUT 的区别?](http://blog.csdn.net/kaaosidao/article/details/77489373)

### 概念及架构原理
- 核心概念，比如cluster集群，shards分片，replicas副本
### Set up Elasticsearch 
### API Conventions
### 精确查询、字段置顶查询、高亮显示、过滤、排序、聚合Aggregate
### Documents APIs
### Search APIs
### Aggregations
### Indices APIs
### cat APIs
### Cluster APIs
### Query DSL
### Mapping
### Analysis
### Modules
### Index Modules
### Ingest Node
### Tune
### 企业级优化


