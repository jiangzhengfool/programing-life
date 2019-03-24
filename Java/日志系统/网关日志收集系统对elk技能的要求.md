### elasticsearch

#### 掌握

1. es配置和系统配置
2. 映射
3. 分析
4. 模块
5. 索引模块
6. Ingest Node
7. Monitoring Elasticsearch
8. 性能优化
9. Elasticsearch Resiliency Status

#### 熟悉

1. 安全设置
2. es更新
3. 文档APIs
4. 搜索APIs
5. 聚合
6. 索引APIs
7. cat APIs
8. 集群APIs
9. 查询DSL
10. 测试
11. Painless Scripting Language
12. 插件集成
13. es 客户端: Java REST Client,Java API
14. Curator Index Management

### kibana(node.js)

#### 掌握

1. 配置kibana 
2. 生产环节的kibana
3. Discover(Lucene Query Syntax)
4. Visualize
5. 监控管理
6. kibana 插件及插件开发
7. Developing Visualizations

#### 熟悉

1. 安全设置
2. Dashboard
3. Timelion
4. Reporting from Kibana

### Logstash

#### 掌握

1. 在配置文件中使用`Event`数据中的属性
2. 多管道的使用 `pipelines.yml`
3. 配置文件的自动重加载 --config.reload.automatic
4. 多行合并 multiline
5. 数据恢复
- Persistent Queues
- Dead Letter Queues
6. filter: date, drop, mutate, dissect, grok, dns, geoip, translate, alter,clone,json,range,split,translate
7. codec: json,json_lines,line,multiline plain,protobuf,truncate,rubydebug
8. output: elasticsearch, file,stdout
9. 部署和扩展logstash
10. 性能调优
- 性能故障的检查排除
- 调优和分析日志存储性能
11. 监控logstash
12. Logstash 监控APIs
13. input:elasticsearch, file, generator, http, stdin, syslog, tcp,
14. **Logstash 配置**


#### 熟悉

1. Glob Pattern
2. `@metadata`
3. Filebeat module
4. Converting Ingest Node Pipelines
5. Logstash modules
6. filter:fingerprint filter, ruby, kv, jdbc_streaming, useragent, aggregate,cidr,cipher,csv,de_dot,dns,elapsed,elasticsearch ,i18n,json_encode,metricize,metrics,prune,sleep,syslog_pri,throttle,tld,urldecode,uuid,xml
7. codec: avro, csv, fluent, xml
8. 插件开发
9. input: beat, dead_letter_queue, exec, github, jdbc, jms,kafka,log4j,meetup,rabbitmq,redis,rss,websocket
10. output: email, exec, http, kafka, rabbitmq, redis,tcp