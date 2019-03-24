####

#### 
#### 测试工具:[mqtt-spy](https://github.com/eclipse/paho.mqtt-spy/wiki)



#### 参考
[MQTT协议中文版](https://mcxiaoke.gitbooks.io/mqtt-cn/content/)
[MQTT协议通俗讲解](https://blog.csdn.net/u011216417/article/details/69666752)
[MQTT--入门](https://blog.csdn.net/qq_28877125/article/details/78325003)
[programcreek-fusesource.mqtt.clien](https://www.programcreek.com/java-api-examples/index.php?api=org.fusesource.mqtt.client.MQTT)
[MQTT开源方案笔记](https://www.jianshu.com/p/56a63d3661ab)

MQTT（Message Queuing Telemetry Transport，消息队列遥测传输）
IBM
物联网
传感器和制动器的通信协议
为大量计算能力有限，且工作在低带宽、不可靠的网络的远程传感器和控制设备通讯而设计的协议

### 特点
- 发布/订阅消息模式,一对多的消息发布,解除应用程序耦合
- 对负载内容屏蔽
- 使用TCP/IP提供网络连接
- 三种消息发布服务质量
    + "至多一次"
    + "至少一次"
    + "只有一次"
- 小型传输,开销很小（固定长度的头部是 2 字节），协议交换最小化，以降低网络流量
- 使用 Last Will 和 Testament 特性通知有关各方客户端异常中断的机制

#  Apollo(MQTT) 
### 服务搭建
- [MQTT——服务器搭建（一）](http://www.cnblogs.com/chenrunlin/p/5090916.html)
### demo
- [MQTT——java简单测试（二）](http://www.cnblogs.com/chenrunlin/p/5109028.html)

### Reference
- [MQTT协议简记 ](http://www.cnblogs.com/caca/p/mqtt.html)
- [聂永的博客](http://www.blogjava.net/yongboy/category/54835.html)