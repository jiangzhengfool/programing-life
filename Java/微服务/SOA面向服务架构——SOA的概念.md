

##                      SOA面向服务架构——SOA的概念

*2018-05-01* 程序员内参

（点击上方公众号，可快速关注）

 SOA的概念是Gartner 在1996年提出来的，并于2002年12月进一步提出SOA是“现代应用开发领域最重要的课题”。

**一、SOA的定义**

SOA 分为广义的SOA和狭义的SOA，广义的SOA是指一种新的企业应用架构和企业IT基础架构，它可以使企业实现跨应用，跨部门，跨企业甚至跨 行业之间的离散系统实现互连。（注意：这里所指的服务并不单单是Web Service,它可以是以Web Service实现 ，也可以以业务方式实现，甚至是书面口头承诺实现）。

而狭义的SOA是指一种软件架构，它可以根据需求通过网络对松散耦合的粗粒度应用组件进行分布式部 署、组合和使用。服务层是SOA的基础，可以直接被应用调用，从而有效控制系统中与软件代理交互的人为依赖性。

 

**二、如何实现SOA**

目 前Web Service越来越流行，并成为实现SOA的一种手段。Web Service使应用功能通过标准化接口（WSDL）提供，使用标准化语言（XML）进行描述，并可基于标准化传输方式（HTTP和JMS）、采用标准化 协议（SOAP）进行调用，并使用XML SCHEMA方式对数据进行描述。你也可以不采用Web服务来创建SOA应用，但是这种标准的重要性日益增加、应用日趋普遍。

 

**三、Web Service实现SOA的好处**

第 一，Web Service是跨平台的，应用程序经常需要从运行在IBM主机上的程序中获取数据，然后把数据发送到主机或UNIX应用程序中去。即使在同一个平台上， 不同软件厂商生产的各种软件也常常需要集成起来。通过WebService，应用程序可以用标准的方法把功能和数据“暴露”出来，供其它应用程序使用。

第二，Web Service是无语言限制的，你可以使用.NET,JAVA,PHP,VB......等多种语言开发并进行相互调用。

第三， 使用SOAP时数据是以ASCII文本的方式传输，调用很方便，数据容易通过防火墙而实现无缝连接。

 

**四、WCF是什么**

WCF 是微软为了实现各个开发平台之间的无疑缝连接而开发一种崭新工具，它是为分布式处理而开发。WCF将DCOM、Remoting、Web Service、WSE、MSMQ、AJAX服务、TCP开发集成在一起，从而降低了分布式系统开发者的学习曲线，并统一了开发标准。

 

**五、WCF的优点**

第 一，开发的统一性。WCF是对于ASMX， Remoting，Enterprise Service，WSE，MSMQ，TCP开发等技术的整合。WCF是由托管代码编写，无论你是使用TCP通讯，Rmoting通讯还是Web Service ，我们都可以使用统一的模式进行开发，利用WCF来创建面向服务的应用程序。

第 二，WCF能够实现多方互操作。它是使用 SOAP通信机制，这就保证了系统之间的互操作性，即使是运行不同开发语言，也可以跨进程、跨机器甚至于跨平台的通信。例如：使用J2EE的服务器(如 WebSphere，WebLogic)，应用程序可以在Windows操作系统进行调用，也可以运行在其他的 操作系统，如Sun Solaris，HP Unix，Linux等等。

第 三，提供高效的安全与可信赖度，它可以使用不同的安全认证将WS-Security，WS-Trust和WS-SecureConversation等添 加到SOAP消息中。在SOAP的header中增加了WS-ReliableMessaging允许 可信赖的端对端通信。而建立在WS-Coordination和WS-AtomicTransaction之上的基于SOAP格式交换的信息，则支持两阶 段的事务提交(two-phase commit transactions)。

第四，WCF支持多支消息交换模式，如请求-应答，单工，双工等等。另外WCF还支持对等网——利用啮合网络址，客户端能在没有中心控制的情况下找到彼此并实现相互通信。

总括来说，WCF是实现SOA的的一个优秀选择，利用WCF能够实现跨平台，跨语言的无缝连接，从而实现Web服务的相互调用。

作者：风尘浪子

cnblogs.com/leslies2/archive/2011/01/26/1934162.html

