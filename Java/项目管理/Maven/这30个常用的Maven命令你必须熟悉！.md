

## 这 30 个常用的 Maven 命令你必须熟悉！

菜鸟要飞  

原文:转载自微信公众号【java技术栈】

maven 命令的格式为 mvn [plugin-name]:[goal-name]，可以接受的参数如下。

~~~
-D 指定参数，如 -Dmaven.test.skip=true 跳过单元测试；-P 指定 Profile 配置，可以用于区分环境；-e 显示maven运行出错的信息；-o 离线执行命令,即不去远程仓库更新包；-X 显示maven允许的debug信息；-U 强制去远程更新snapshot的插件或依赖，默认每天只更新一次。
~~~

## 常用maven命令

~~~
创建maven项目：mvn archetype:create 指定 group： -DgroupId=packageName 指定 artifact：-DartifactId=projectName创建web项目：-DarchetypeArtifactId=maven-archetype-webapp  验证项目是否正确：mvn validatemaven 打包：mvn package只打jar包：mvn jar:jar生成源码jar包：mvn source:jar产生应用需要的任何额外的源代码：mvn generate-sources编译源代码： mvn compile编译测试代码：mvn test-compile运行测试：mvn test运行检查：mvn verify清理maven项目：mvn clean生成eclipse项目：mvn eclipse:eclipse清理eclipse配置：mvn eclipse:clean生成idea项目：mvn idea:idea安装项目到本地仓库：mvn install发布项目到远程仓库：mvn:deploy在集成测试可以运行的环境中处理和发布包：mvn integration-test显示maven依赖树：mvn dependency:tree显示maven依赖列表：mvn dependency:list下载依赖包的源码：mvn dependency:sources安装本地jar到本地仓库：mvn install:install-file -DgroupId=packageName -DartifactId=projectName -Dversion=version -Dpackaging=jar -Dfile=path
~~~

## web项目相关命令

~~~
启动tomcat：mvn tomcat:run启动jetty：mvn jetty:run运行打包部署：mvn tomcat:deploy撤销部署：mvn tomcat:undeploy启动web应用：mvn tomcat:start停止web应用：mvn tomcat:stop重新部署：mvn tomcat:redeploy部署展开的war文件：mvn war:exploded tomcat:exploded    
~~~

