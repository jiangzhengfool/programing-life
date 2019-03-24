## Spring Boot的starter原理，自己实现一个starter

## 多环境配置

## 服务发现和注册

## SpringBOOT常用注解
#### @SpringBootApplication
等价于以默认属性使用 @Configuration ， @EnableAutoConfiguration 和 @ComponentScan

#### @EnableEurekaClient
通过注解@EnableEurekaClient 表明自己是一个eurekaclient。

服务发现注解。如果选用的注册中心是eureka，就推荐@EnableEurekaClient，如果是其他的注册中心，推荐使用@EnableDiscoveryClient。

#### @ConfigurationProperties
通过 @ConfigurationProperties(prefix = "home”) 注解，将配置文件中以 home 前缀的属性值自动绑定到对应的字段中。

## webflux