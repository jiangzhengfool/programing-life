面向Windiws Standaed Server、Windows Enterprise Server以及Windows Datacenter Server的目录服务。（AD不能允许运行在Windows Web Server上，但可以通过它对Windows Web Server的计算机进行管理）AD存储了有关网络对象的信息，并且让管理员和用户能够轻松地查找和使用这些信息。AD使用了一种结构化的数据存储方式，并以此作为基础对目录信息进行合乎逻辑的分层组织。

Windows平台的核心组件

## 步骤
- 配置虚拟机AD环境https://www.cnblogs.com/IFire47/p/6683435.html
- 加入虚拟机域环境
- 使用Java操作ADhttps://www.jianshu.com/p/7e4d99f6baaf
- 制作Demo环境

## 数据存储
目录存储在被称为域控制器的服务器上，并且可以被网络应用程序或者服务访问。一个域可以拥有一台以上的域控制器。每台域控制器都拥有它所在域的目录的一个可写副本。对目录的任何修改都可以从源域控制器复制到域、域树或者森林中的其他域控制器上。由于目录可以被复制，而且所有的域控制器都拥有目录的一个可写副本，所有用户和管理员便可以在域的任何位置方便地获取所需的目录信息。

目录数据存储在域控制器上的Ntds.dit文件中。建议将该文件存储在一个NTFS分区上。有些数据保存在目录数据库文件中，但有些数据则保存在一个被复制的文件系统上，如登录脚本和组策略。

有三种类型的目录数据会在各台域控制器之家进行复制：

域数据。包含了与域中的对象有关的信息。一般来说，这些信息可以使诸如电子邮件联系人、用户和计算机账户属性已经已发布资源等这样的目录信息

例如，在向网络中添加了一个用户帐户的时候，用户帐户对象以及属性数据便被保存在域数据中。如果您修改了组织的目录对象，例如创建、删除对象或者修改了某个对象的属性，相关的数据都会被保存在域数据中。

配置数据。描述了目录的拓扑结构。包括一个包含了所有域、域树和森林的列表，并指出了域控制器和全局编录所处的位置。

架构数据。是对目录中存储的所有对象和属性数据的正式定义。架构对象受访问控制列表（ACL）的保护，这确保了只有进过授权的用户才能改变架构。

## 功能
- 服务器及客户端计算机管理：管理服务器及客户端计算机账户，所有服务器及客户端计算机加入域管理并实施组策略
- 用户服务：管理用户域账户、用户信息、企业通讯录（与电子邮件系统集成）、用户组管理、用户身份认证、用户授权管理等，按省实施组管理策略
- 资源管理：管理打印机、文件共享服务等网络资源
- 桌面配置：集中配置各种桌面配置策略，如：用户使用域中资源权限限制、界面功能的限制、应用程序执行特征限制、网络连接限制、安全配置限制等
- 应用系统支撑：支持财务、人事、电子邮件、企业信息门户、办公自动化、补丁管理、防病毒系统等各种应用系统

## 常用对象
#### 域（Domain）  A公司总部
域是AD的根，是AD的管理单位。域中包含大量地域对象，如：组织单位(Organizational Unit)，组(Group)，用户(User)，计算机(Computer)，联系人(Contact)，打印机，安全策略等。

#### 组织单位（Organization Unit）：A公司的分公司
是一个容器对象，可以把域中的对象组织成逻辑组，帮助网络管理员将管理组。组织单位包括下列类型的对象：用户，计算机，工作组，打印机，安全策略，其他组织单位等。可以在组织单位基础上部署组策略，统一管理组织单位中的域对象。

#### 群组（Group）：某公司里的某事业部
是一批具有相同管理任务的用户账户，计算机账户或者其他域对象的一个集合。例如公司的开发组，产品组，运维组等。

#### 用户（User）：某个工作人员
AD中域用户的最新管理单位，域用户最容易管理又最难管理，如果赋予域用户的权限过大，将带来安全隐患，如果权限过小域用户无法正常工作。

常见的域用户类型：
- 普通域用户：创建的域用户默认就添加到"Domain Users"中
- 域管理员：普通域用户添加进"Domain Admins"中，其权限升为域管理员
- 企业管理员：普通域管理员添加进"Enterprise Admins"后，其权限提升为企业管理员，企业管理员具有最高权限。

一个大致的AD图如下

![](https://images2015.cnblogs.com/blog/1140527/201704/1140527-20170408234015941-1792491553.jpg)

## 数据结构
域(Domin)--->组织单位(Organization Unit)--->群组(Group)--->用户(User)从左至右依次嵌套，形成树状型结构

需要注意的点：
- OU可以多级嵌套。
- 在不区分主域控服务器和辅域控服务器时候，在同一域控服务器内，不能存在同名的域。正如不会存在两个同名的公司一样。
- 同一层级的OU中不能存在相同名称的OU，但是在不同层级的OU中是可以有相同名称的OU存在。
- User在相同OU中不能出现同名的情况，在不同OU中是可以有相同名称的User存在的。但是在全局即整个Domain中是不允许出现相同用户登录名的用户存在的。


## 安全性
安全性通过登录身份验证以及目录对象的访问控制集成在AD之中。通过单点网络登录，管理员可以管理分散在网络各处的目录数据和组织单位，进过授权的网络用户可以访问网络任意位置的资源。基于策略的管理则简化了网络的管理。

AD通过对象访问控制列表已经用户凭据保护其存储的用户账户和组信息。

## 架构
#### 分类
描述管理员所能创建的目录对象。每个分类都是一组对象的集合。AD中的所有对象都是某个对象分类的一个实例

#### 扩展
通过为对现有分类定义新的属性或者新的分类来动态地拓展架构。

架构的内容由充当架构操作主控角色的域控制器进行控制。架构的副本被复制到森林中的所有控制器上。

还可以使用“AD架构”管理单元对架构加以扩展。为了修改架构，需满足一下三个要求：

- 成为“Schema Administrators”（架构管理员）组的成员
- 在充当架构操作主控角色的计算机上安装“AD架构”管理单元
- 拥有修改主控架构所需的管理员权限

与系统有关的架构分类不能被修改。对架构的扩展不可撤销。但可以废除相关定义并且重新使用对象标识符（OID）或者显示名称来撤销一个架构定义。AD不支持对架构对象的删除，但，对象可以被标记为“非激活”，以便实现与删除等同的诸多益处。

#### 属性
属性和分类单独进行定义。每个属性仅仅定义一次，但可以在多个分类中使用。

属性用来描述对象。架构中的每一个属性都可以在“Attribute-Schema”分类中指定，改分来决定了每个属性定义所必须包含的信息。

能够应用到某个特殊对象上的属性列表由分类（对象是该分类的一个实例）已经对象分类的任何超类所决定。


- 多值属性。属性可以是单值也可以是多值。属性的架构定义了属性的实例是否必须是多值的。单值属性的实例可以为空，也可以包含一个单值。多值属性的实例可以为空，也可以包含一个单值或多值。多值属性的每一个值都必须是唯一的。
- 索引属性。索引应用于属性，而不是分类。对属性进行索引有助于更快速地查询到改属性的对象。当您将一个属性标记为“已索引”之后，该属性的所有实例都会被添加到改索引，而不是仅仅将作为某个特定分类成员的实例添加到索引。添加经过索引的属性会影响AD的复制时间、可用内存以及数据库大小。因为数据库变得更大了，所以需要花费更多的时间进行复制。多值属性也可以被索引。同单值属性的索引相比，多值属性的索引进一步增加了AD的大小，并且需要更多的时间来创建对象。在选择需要进行索引的属性时，请确信所选择的共用属性，而且能够在开销和性能之间取得平衡。一个经过索引的架构属性还可以被用来存储属性的容器所搜索，从而避免了对整个AD数据库进行搜索。这样不仅缩短了搜索所需花费的时间，而且减少了在搜索期间需要使用的资源数量。

#### 全局编录
全局编录在森林最初的一台域控制器上自动创建。您可以为任何一台域控制器添加全局编录功能，或者将全局编录的默认位置修改到另一台域控制器上。

查找对象全局编录允许用户搜索森林所有域的目录信息，而不管数据存储在何处。森林内部的搜索可以利用最快的速度和最小的网络流量得以执行。

提供了根据用户主名的身份验证。在进行身份验证的域控制器不知道某个账户是否合法时，全局编录便可以对用户的主名进行解析。

在多域环境下提供通用组的成员身份信息。

即便全局编录不可用，“Domain Administrators”（域管理员）组的成员也可以登录到网络中。

#### 查找目录信息
AD的主要益处就在于它能够存储有关网络对象的丰富信息。在AD中发布的有关的用户、计算机、文件和打印机的信息可以被网络用户所使用。这种可用性能够通过查看信息所需的安全权限加以控制。AD就像是一个在企业中共享的地址簿，例如，您可以按照姓、名、电子邮件地址、办公室位置或者其它用户账户属性查找用户。

## 复制
复制为目录信息提供了可用性、容错能力、负载均衡已经性能优势。AD使用多主控复制。

域控制器可以储存和复制：架构信息、配置信息、域信息、应用程序信息。

## 客户端
在客户端可以使用众多特性：
- 站点感知。可以登录到网络中距离最近的一台域控制器上。
- AD服务接口（ADSI）。您可以使用它为AD编写脚本。ADSI还为AD编程人员提供了一个公共的编程API。
- 分布式文件系统（DFS）容错客户端
- NTLM version 2身份验证.
- AD Windows地址簿（WAB）属性页。
- AD的搜索你能力
