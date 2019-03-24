# Linux的系统机构

# 虚拟网络的配置(4种)

# Linux常用命令
### [Linux命令大全](http://man.linuxde.net/)
### [linux常用命令](https://www.jianguoyun.com/p/DcXCOnIQkJSpBhjYmC8)
### [Linux常用命令详解](https://www.jianguoyun.com/p/DdGm1PEQkJSpBhjcmC8)
### [Linux命令大全完整版](https://www.jianguoyun.com/p/DcqDlMUQkJSpBhjdmC8)

# 远程登录的方式及其区别

# Linux的目录结构

# 用户和用户组的管理

# 文件的权限管理

# 标准的输入和输出

# 管道

# DNS解析

# 重要软件
### crontab 
### SSH免密码
### VIM编辑器的使用
- [Vim命令小抄](http://i.imgur.com/u3clIhh.png)
- [Vim键盘图](http://i.imgur.com/GE5ZqjT.png)
### nginx
- [nginx静态资源配置](http://www.cnblogs.com/zhoubang521/p/5200280.html)


进程同步
缓冲区溢出
分段和分页
linux操作系统熟悉以centos为例： 
常用简单命令：ssh、vim、scp、ps、gerp、sed、awk、cat、tail，df、top，shell、chmod、sh、tar、find、wc、ln、|
目录结构明细:/etc/、~/、/usr/、/dev/、/home/、/etc/init.d/
服务端：jdk、tomcat、nginx、mysql、jedis、neo4j启动与配置（特别说明的是该死的防火墙，nginx启动后一直访问不了，查找一下午查不到原因，最后发现是防火墙问题）
监控服务器状态（cpu，磁盘，内存），定位pid，日志查看
nginx负载均衡、反向代理、配置
自动化部署脚本
监控系统，代码抛fatal异常自动发邮件，系统指标持续偏高自动发邮件。
虚拟内存与主存

## 监控
监控什么	CPU、内存、磁盘I/O、网络I/O等
监控手段	进程监控、语义监控、机器资源监控、数据波动
监控数据采集	日志、埋点
Dapper	
负载均衡	tomcat负载均衡、Nginx负载均衡
DNS	DNS原理、DNS的设计
CDN	数据一致性

## CentOS 7查看系统版本及查看机器位数x86-64
https://www.linuxidc.com/Linux/2016-11/137550.htm

## 卸载CentOS7-x64自带的OpenJDK
https://www.cnblogs.com/CuteNet/p/3947193.html

## centos7关闭图形界面启动系统
https://www.cnblogs.com/hanggegege/p/6558945.html

## CentOS7--系统设置语言环境
https://www.cnblogs.com/zydev/p/7794525.html

## centos7 选定默认启动内核，及删除无用内核
https://www.cnblogs.com/niyeshiyoumo/p/6762193.html
## pcie-bus-error
[Ryzen Threadripper kernel problems with PCIe – wifi, network, video problems](https://ahelpme.com/linux/ryzen-threadripper-kernel-problems-with-pcie-wifi-network-video-problems/)](https://ahelpme.com/tag/pcie-bus-error/)

Java常用问题排查工具及用法（top, iostat, vmstat, sar, tcpdump, jvisualvm, jmap, jconsole）