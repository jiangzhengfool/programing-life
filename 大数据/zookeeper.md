## 操作权限
CREATE、READ、WRITE、DELETE、ADMIN 也就是增、删、改、查、管理，这5种权限简称为crwda。

DELTE是指对子节点的删除权限，其他4种权限指对自身节点的操作权限。

## 身份认证
#### 4种方式
world：默认方式，相当于全世界都能访问
auth：代表已经认证通过的用户(cli中可以通过addauth digest user:pwd 来添加当前上下文中的授权用户)
digest：即用户名:密码这种方式认证，这也是业务系统中最常用的
ip：使用Ip地址认证

## 设置用户名和密码
`setAcl /test digest:用户名:密码:权限` 给节点设置ACL访问权限时，密码必须是加密后的内容（SHA1加密，然后base64编码）

`addauth digest user1:12345` 给"上下文"增加了一个认证用户

~~~
addauth digest user1:12345	增加一个认证用户
setAcl /test auth:user1:12345:r 设置权限，跟刚才的效果一样，但是密码这里输入的是明文
~~~

## zookeeper有什么功能，选举算法如何进行

> [ZooKeeper 笔记(5) ACL(Access Control List)访问控制列表](http://www.cnblogs.com/yjmyzz/p/zookeeper-acl-demo.html)