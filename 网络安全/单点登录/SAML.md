断言，做出断言的一方必须被信赖。校验来自断言方的断言必须通过一些手段，比如数字签名，以确保断言的确实来自断言方。

目标：让多个应用间实现联邦身份（Identity Federeration）（联邦：例如欧盟）。

通常有下面3中实体：

* Subject（主题）：包括了User、Entity、Workstation等能够象征一个参与信息交换的实体。
* Relying Party（信任方）：就是提供服务的一方。即服务提供方（Service Provider）：一个应用。
* Asserting Party（断言方）：SAML中的Identity Provider角色，用于提供对主题的身份信息的正确断言，类似一个公证机构。即身份认证中心。有所有Subject信息。

服务提供方（Service Provider）不负责审核用户的身份信息，它依赖于1个或者多个Identity Provider来完成此任务。

ADFS是断言方。有所有的用户数据。提供SAMl断言。
应用是信任方，应用要信任ADFS的SAML断言。
使用非对称认证，验证SAMl断言来自ADFS.

## 2种典型模式
#### SP拉方式
SP主动到IDP去了解Subject的身份断言

#### IDP推方式
IDP主动把Subject的身份断言通过某种途径告诉SP