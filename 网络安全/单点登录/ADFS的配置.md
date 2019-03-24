先安装并配置好AD后再安装ADFS


## 问题：
域控制器升级的先决条件验证失败。当您创建一个新域时，本地管理员帐户就成为域管理员帐户。无法创建新域，因为本地管理员帐户密码不符合要求。

解决方案：
net user administrator /passwordreq:yes 

https://{server}/federationmetadata/2007-06/federationmetadata.xml

> 参考资料：[实战：ADFS3.0单点登录系列-ADFS3.0安装配置](https://www.cnblogs.com/hudun/p/5915960.html)