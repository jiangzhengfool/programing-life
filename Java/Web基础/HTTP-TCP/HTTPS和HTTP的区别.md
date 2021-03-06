

##                      HTTPS和HTTP的区别

*2018-03-06* 菜鸟要飞

#### **什么是 HTTPS?**

HTTPS (基于安全套接字层的超文本传输协议 或者是 HTTP over SSL) 是一个 Netscape 开发的 Web 协议。

你也可以说：HTTPS = HTTP + SSL

HTTPS 在 HTTP 应用层的基础上使用安全套接字层作为子层。

#### **为什么需要 HTTPS ？**

超 文本传输协议 (HTTP) 是一个用来通过互联网传输和接收信息的协议。HTTP 使用请求/响应的过程，因此信息可在服务器间快速、轻松而且精确的进行传输。当你访问 Web 页面的时候你就是在使用 HTTP 协议，但 HTTP 是不安全的，可以轻松对窃听你跟 Web 服务器之间的数据传输。在很多情况下，客户和服务器之间传输的是敏感歇息，需要防止未经授权的访问。为了满足这个要求，网景公司(Netscape)推出 了HTTPS，也就是基于安全套接字层的 HTTP 协议。

#### **HTTP 和 HTTPS 的相同点**

大 多数情况下，HTTP 和 HTTPS 是相同的，因为都是采用同一个基础的协议，作为 HTTP 或 HTTPS 客户端——浏览器，设立一个连接到 Web 服务器指定的端口。当服务器接收到请求，它会返回一个状态码以及消息，这个回应可能是请求信息、或者指示某个错误发送的错误信息。系统使用统一资源定位器 URI 模式，因此资源可以被唯一指定。而 HTTPS 和 HTTP 唯一不同的只是一个协议头(https)的说明，其他都是一样的。

#### **HTTP 和 HTTPS 的不同之处**

HTTP 的 URL 以 http:// 开头，而 HTTPS 的 URL 以 https:// 开头

HTTP 是不安全的，而 HTTPS 是安全的
HTTP 标准端口是 80 ，而 HTTPS 的标准端口是 443

在 OSI 网络模型中，HTTP 工作于应用层，而 HTTPS 工作在传输层

HTTP 无需加密，而 HTTPS 对传输的数据进行加密

HTTP 无需证书，而 HTTPS 需要认证证书

#### **HTTPS 如何工作?**

使用 HTTPS 连接时，服务器要求有公钥和签名的证书。

当 使用 https 连接，服务器响应初始连接，并提供它所支持的加密方法。作为回应，客户端选择一个连接方法，并且客户端和服务器端交换证书验证彼此身份。完成之后，在确保 使用相同密钥的情况下传输加密信息，然后关闭连接。为了提供 https 连接支持，服务器必须有一个公钥证书，该证书包含经过证书机构认证的密钥信息，大部分证书都是通过第三方机构授权的，以保证证书是安全的。

换句话说，HTTPS 跟 HTTP 一样，只不过增加了 SSL。

HTTP 包含如下动作：

1.  浏览器打开一个 TCP 连接

2.  浏览器发送 HTTP 请求到服务器端

3.  服务器发送 HTTP 回应信息到浏览器

4.  TCP 连接关闭

SSL 包含如下动作：

1.  验证服务器端

2.  允许客户端和服务器端选择加密算法和密码，确保双方都支持

3.  验证客户端(可选)

4.  使用公钥加密技术来生成共享加密数据

5.  创建一个加密的 SSL 连接

6.  基于该 SSL 连接传递 HTTP 请求

#### **什么时候该使用 HTTPS?**

银行网站、支付网关、购物网站、登录页、电子邮件以及一些企业部门的网站应该使用 HTTPS，例如：

*   PayPal: https://www.paypal.com

*   Google AdSense: https://www.google.com/adsense/

如果某个网站要求你填写信用卡信息，首先你要检查该网页是否使用 https 加密连接，如果没有，那么请不要输入任何敏感信息如信用卡号。

#### **浏览器集成**

多数浏览器在收到一个无效证书的时候都会显示警告信息，而一些老的浏览器会弹出对话框让用户选择是否继续浏览。新的浏览器一般在整个窗口显示横幅的警告信息，同时在地址栏上显示该网站的安全信息。如果网站中包含加密和非加密的混合内容，多数浏览器会提示警告信息。

原文转载自：http://blog.csdn.net/whatday/article/details/38147103

如有侵权，请及时联系，谢谢！

