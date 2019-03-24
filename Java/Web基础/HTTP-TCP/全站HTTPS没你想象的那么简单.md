

##                      全站 HTTPS 没你想象的那么简单

*2017-09-11* Java编程

> **来自：Mafly - 博客园**

> **链接：www.cnblogs.com/mafly/p/allhttps.html********（点击尾部阅读原文前往）******
> 
> 程序猿自媒体已获转载授权

##  **** 

## **对自己无知这件事本身的无知真的挺可怕**

  

认 知偏差现象一直存在于我们每个人身上，谁也避免不掉，不过是有的人了解这件事儿，有的人不怎么知道而已，这就产生了「无知而不自知」的认知偏差。当然，这 时候你自己忽悠自己倒没什么，顶多让自己每天感觉自己挺厉害的，沉浸于虚幻的优越感中，以为自己比大多数人都优秀，这倒不是一件什么坏事情，但是，如果你 和别人沟通交流中展现出来，那挺可怕的，况且有时候你自己并不知道，达克效应(Dunning-Kruger Effect)描述的就是这种现象。

 避免这种现象在自己身上的存在，没什么特殊方法，多学习那些本身就极其优秀的人是怎么思考和生存的，表现出谦逊算其中一种，还有就是多读书。就这样...

  

## **全站 HTTPS 必要准备工作**

  

做任何一件事情最好的情况就是你刚好做过，这倒没什么可说的，因为第二次总是要比第一次好。如果你没做过这件事怎么办？没事，去看看别人怎么做的。

升 级全站 HTTPS 工作在两年前左右应该是讨论最火的了，在2014年底，Google Chromium 安全团队提议将所有的 HTTP 协议网站标注为不安全，市场占有率较高的 Chrome 浏览器也是这么做的，所以在接下来一段时间内，各个大厂、大公司都逐步升级了 HTTPS 协议，当然，去年 Apple 也宣布所有应用开发者必须在 2017 年 1 月 1 日之前实现所有的 App 接入安全地服务器，即网络传输协议使用 HTTPS。所以呢，我们就简单的看一下国内这些顶尖互联网企业如何实现全站 HTTPS 的：

  

*   **淘宝**
    启用全站HTTPS后不仅更安全而且更快 看淘宝是如何做到的

*   **百度**
    大型网站的HTTPS实践一：HTTPS协议和原理
    HTTPS对网站性能SEO有哪些影响？
    大型网站HTTPS实践三：基于协议和配置的优化
    大型网站的HTTPS实践四：协议层以外的实践

  

看完这些文章后，估计你就可以知道要买 SSL 证书了，也可以去买 SSL 证书了，具体是使用各个云服务商家的免费单域名证书，还是业务需要更强大的泛域名证书、OV 证书等等，你就需要看看我写的这两篇文章了（好不要脸吖..）：

*   让你的网站免费支持 HTTPS 及 Nginx 平滑升级

*   一篇文章让你搞懂 SSL 证书

##   

## **分析整个系统制定计划**

  

有 计划才能没变化。其实也没什么要做的，只有一件事，你接下来要做的唯一一件事就是了解整个系统。统计出所有已用到的域名，需要购买什么类型域名证书，是二 级域名、三级域名还是各种乱七八糟的域名，自己分析；再然后，了解每个域名背后的服务是如何运作的，这里边会涉及到前端页面、一些资源文件的固定协议引 用，后端代码中关于协议获取是写死的还是动态的，数据库中存储的网址链接等等，这些统统要考虑到。

  

分 析完系统后，其中肯定会存在混合协议访问请求，HTTPS 下浏览器会拦截掉所有 HTTP 请求的，不同页面间跳转、不同服务域名间跳转如果是以固定的 HTTP 协议写死的，要支持全站 HTTPS 协议，首要解决的是以当前协议来灵活的区分不同域名服务间的跳转。其次，HTTPS 协议首次请求存在多次握手，因此网络耗时变长问题，可能会影响系统访问速度。所以，我是建议计划分为两个阶段来进行全站 HTTPS 升级：

  

*   **一阶段：**将目前所有域名配置为支持 HTTP 和 HTTPS 两种协议，不做 HTTP 请求强制 HTTPS 跳转。在验证及测试完成 HTTPS 下，系统所有服务以及访问速度均无问题后，进行实施二阶段计划。

*   **二阶段：**在上阶段不强制 HTTPS 访问验证通过后，域名做强制 HTTPS 协议。即当用户以 HTTP 协议访问系统时， 如用 Nginx 做强制 301 跳转到 HTTPS 协议，做到全站 HTTPS 安全访问协议。

  

不 出意外，按照这两步计划，应该可以稳妥是进行全站 HTTPS 升级工作，当然，期间不可避免的会踩一些坑，因为每个公司业务不同、系统环境不同等原因，都会遇到不可预估的问题，一个个解决就行了。我下面会写一下升级 期间共性的、也就是每个人都必须要踩的坑和如何解决这些问题。

  

## **十条注意事项**

  

**1、浏览器拦截混合访问请求**

 由于浏览器安全规则，在 HTTPS 请求下通过 JavaScript 请求 HTTP 请求或引入 HTTP 协议资源文件，会报“Mixed Content”错误，导致请求无法继续。

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

~~~
Mixed Content: The page at 'https://domain.com/' was loaded over HTTPS, but requested an insecure script 'http://domain.com/'. This request has been blocked; the content must be served over HTTPS.
~~~

**2、前端 HTML、JS 资源引用存在 HTTP**

 前端页面及 js 文件中，写死的 http:// 协议资源及跳转改为根据当前协议切换(//)。
使用相对协议，如：

~~~
<script src="//domain.com/jquery.js</script><img src="//domain.com/img/logo.png">
~~~

或者代码自行判断都可以。如果一个页面内包含多个域名请求，需所有域名均支持https，否则部分浏览器会有警告提醒或打不开。

  

**3、移动端适配 HTTPS**

 如 果你们存在移动客户端（APP）也需要适配 HTTPS，所以必须做调用接口的相应修改，当然，要注意运营商 DNS 劫持（尤其是移动）也会对 HTTPS 请求成功率造成很大影响，其实可以做好 HTTP/HTTPS 两种协议都支持，做好出问题随时动态降级切换准备。

 **** 

**4、项目中存在的配置问题**

 项 目中用到的配置文件中存在 HTTP 链接，要充分了解其用途。如果不是可以统一动态更新的配置文件，都要考虑更改后做服务的发布，这时候要来考虑对生成环境业务的影响以及测试、开发环境的影 响等问题。如页面间跳转、权限、登录验证、第三方服务（支付、推送）回调这些配置等。

  

**5、关于 request.getScheme() 这个坑**

 如 果你的架构上使用了 Nginx + Tomcat 集群, 且 Nginx 下配置了 SSL ，Tomcat 没有配置 SSL ，这时候其实客户端已经是使用 HTTPS 协议了，但你的 Tomcat 中用 request.getScheme()、request.getRequestURL() 会获取到的是 HTTP，并不是 HTTPS 。当然可以代码中避免，或者通过配置 Nginx 和 Tomcat 解决，看这篇文章：http://www.cnblogs.com/interdrp/p/4881785.html

 **** 

**6、SSL 证书类型**

 在 之前说选购 SSL 证书的时候，你就要充分考虑业务上域名需要的证书类型，避免需要泛域名证书而你买了单域名证书，当然泛域名证书还是分为支持级别的，如购 买 *.example.com 证书，那么该证书支持 a.example.com, a1.example.com, a2.example.com 以此类推域名，但是不支持 b.a.example.com（另一级）, b1.a.example.com 类域名，如需支持，需另外再购买一张 *.a.example.com 证书。

  

**7、Nginx 配置同一个 server 段不支持配置多个证书**

 **8、Nginx 配置 HTTP 强制跳转 HTTPS**

 通过配置 Nginx 域名 HTTP 请求 302 跳转实现全站 HTTPS。千万不能有 POST 请求，这时候浏览器会先做 302 跳转，在以 Get 方法请求会导致 Post Body 丢失。
具体配置如下：

~~~
server {       listen 80;       listen 443 ssl;       server_name  www.domain.com;       ssl on;       ssl_certificate 1_www.domain.com_bundle.crt;       ssl_certificate_key 2_www.domain.com.key;       ssl_session_timeout  5m;       ssl_protocols TLSv1 TLSv1.1 TLSv1.2;       ssl_ciphers  ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;       ssl_prefer_server_ciphers  on;       if ($scheme != 'https') {       rewrite ^(.*)$  https://$server_name$1 permanent;       }       location / {           root   html;           index  index.html index.htm;       }   }
~~~

**9、所有环境均要进行升级**

 不仅仅要考虑生成环境进行全站 HTTPS 升级问题，包括开发、测试、预发布等多种不同环境均要进行升级，来保持与生产环境的一致性，减小不可预估因素的发生。如果你没有完善的运维系统，一个个配置文件改的可是真的很痛苦，你试试想想看上百个配置，泪...

 **** 

**10、打死你都想不到的地方**

 太多了，自由发挥吧。做到兵来将挡水来土掩，佛来斩佛，魔来斩魔就行了。

注意事项写完了，现在插播一条硬广，我们团队目前正需要对技术有追求的小伙伴一起来共同学习进步，看到这篇文章有想换个工作环境的，当然你要基本了解使用过分布式架构，快快联系我。

  

## **总结一下**

  

不知道这一篇算不算所谓的「干货」了，现在好多人都喊着要所谓的干货，其实哪有那么多干货阿。**这一篇主要写了一下在全站升级 HTTPS 的过程与注意点，几乎都是在实际工作中步骤的重现了，当然，升级完成我们还是需要对整个系统进行性能、速度的测试，以及如何更好的利用 HTTPS ，比如上 HTTP 2.0 ，据说提升很大的。**

**希望对你有所帮助。**

