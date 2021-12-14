---
title: Spring基础及入门
date: 2021-09-22 13:47:24
tags: 
- Spring
- Java
author: 别听学唱情歌
img: https://yzh5675.oss-cn-beijing.aliyuncs.com/blog/img/al01
categories:
- Spring
- Java
abbrlink: aci2
top: true
cover: true
toc: false
copyright: true
toc_number: true
mathjax: true
keywords: 
- Spring
description: Java中Spring框架概念概述
summary: Java中Spring框架概念概述
---

# Spring基础及入门

## 一、Spring框架概述

##### 	1、Spring：是一个**分层的 Java EE full-stack(一站式) 轻量级开源的 Java EE 框架**

##### 	2、为什么用它：Spring 使 Java **编程更快**、**更容易**、**更安全**

##### 	3、Spring的核心部分：

​		① IOC：控制反转，把创建的对象的过程交给Spring进行管理
​	    				底层原理：xml解析、工厂模式、反射
​		② AOP：面向切面，不修改源码进行功能增强
​		③ DI：依赖注入，在 spring 框架负责创建 Bean 对象时，动态的将依赖对象注入到 Bean 组件中目的都是为了降低代码耦合度

##### 	4、Spring特点:

​		（1）方便解耦，简化开发
​		（2）AOP 编程支持
​		（3）方便程序测试
​		（4）方便和其他框架进行整合
​		（5）方便进行事务操作
​		（6）降低 API 开发难度

#### 二、创建Spring容器

​	两种方式：
​		1、BeanFactory：IOC 容器基本实现，是 Spring 内部的使用接口，不提供开发人员进行使用加载配置文件时候不会创建对象，在获取对象（使用）才去创建对象

​		2、ApplicationContext：BeanFactory 接口的子接口，提供更多更强大的功能，一般由开发人员进行使用加载配置文件时候就会把在配置文件对象进行创建
​		ApplicationContext：Spring的上下文

```java
ApplicationContext ap = new FileSystemXmlApplicationContext("xml绝对路径");
ApplicationContext ap = new ClassPathXmlApplicationContext("xml相对路径");
```

​	一、SpringBean的概述
​	SpringBean的概述：Spring Bean 是一个被实例化、组装并通过 Spring IOC 容器所管理的对象；它是构成 Spring 应用程序的支柱；简单来说就是一个Java类

## 二、SpringBean的管理

#### 	1、Bean管理的三种方式

​		① 基于XML的配置文件
​		② 基于注解的配置
​		③ 基于java的配置

#### 	2、IOC对Bean的管理（XML）

​	       ① 基于xml方式创建对象
​		<bean id="user" class="类全路径"></bean>
​		（1）在 spring 配置文件中，使用 bean 标签，标签里面添加对应属性，就可以实现对象创建。 
​		（2）在 bean 标签有很多属性，介绍常用的属性。

```java
		* id 属性：唯一标识
		* class 属性：类全路径（包类路径）
	（3）创建对象时候，默认也是执行无参数构造方法完成对象创建。

       ② 基于xml方式注入属性
	（1）基于构造函数的依赖注入
		在bean标签里写这个标签：
		      <constructor-arg value="赋给属性的值"></constructor-arg>
	（2）基于设值函数的依赖注入
		在bean标签里写这个标签：
			<property name="属性名" value="赋给属性的值"></property>
```

#### 3、SpringBean的作用域

​	*singleton 	在 spring IOC 容器仅存在一个 Bean 实例，Bean 以单例方式存在，默认值
​	*prototype 	每次从容器中调用 Bean 时，都返回一个新的实例，即每次调用getBean()时，相当于执行 newXxxBean()
​	request 		每次 HTTP 请求都会创建一个新的 Bean，该作用域仅适用于WebApplicationContext 环境
​	session 		同一个 HTTP Session 共享一个 Bean，不同 Session 使用不同的 Bean，仅适用 WebApplicationContext 环境
​	global		session一般用于 Portlet 应 用 环 境 ， 该 运 用 域 仅 适 用 于WebApplicationContext 环境

```java
1、singleton 和 prototype 区别 
	① singleton 单实例，prototype 多实例。
	② 设置 scope 值是 singleton 时候，加载 spring 配置文件时候就会创建单实例对象。
	③ 设置 scope 值是 prototype 时候，不是在加载 spring 配置文件时候创建 对象，在调用 getBean 方法时候创建多实例对象

2、Bean的生命周期
	① 创建：通过构造器或工厂方法创建 Bean 实例
	② 调用属性及设置属性的值
	③ 初始化Bean
	④ 使用Bean（执行业务逻辑）
	⑤ 销毁Bean

3、延迟初始化Bean
	在Bean标签里加这个属性：lazy-init="true";
	提前实例化方式是好事，因为配置中或者运行环境的错误就会被立刻发现。延迟初始化 bean 会告诉 IOC 容器在第一次需要的时候才实例化而不是在容器启动时
	就实例化

4、自动装配属性
	① 在Bean标签里加这个属性：autowire="byType/byName/constructor";
	② byName：根据名字自动装配		byType：根据属性类别自动装配		constructor：通过构造器进行自动装配
	③ 优缺点：
		优点：简化了开发步骤
		缺点：autowire 属性要么根据类型，要么根据名称自动装配，不能二者兼而有之。基于xml 配置的 spring 工程，很少使用自动装配。而基于注解的 spring 工程，还是会用到自动装配。
```

#### 4、SpingJdbcTemplate

##### 	XML的配置

```java
	<!-- 注册数据访问层实现类 -->
	<bean id="数据访问层实现类名" class="数据访问层实现类的绝对路径" p:temp-ref="JdbcTemplate的属性名"></bean>
```

​	

```java
<!-- 注册JDBCTemplte -->
<bean id="temp" class="org.springframework.jdbc.core.JdbcTemplate" p:dataSource-ref="dataSource"></bean>

<!-- 注册数据源 -->
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
	<property name="driverClassName" value="org.springframework.jdbc.datasource.DriverManagerDataSource"></property>
	<property name="url" value="jdbc:mysql:///employee"></property>
	<property name="username" value="root"></property>
	<property name="password" value="root"></property>
</bean>
```



## 三、AOP概述

​	概念：**面向切面，把业务逻辑和系统级服务分开，可以把AOP看作一个插件，在不改变源码的基础上把一些功能进行增强或添加**

#### 二、AOP原理

​	原理：**动态代理**
​	

#### 三、AOP名词

​	切面：一个模块
​	织入：把切面代码插入到目标对象某个方法的过程
​	连接点：被织入的点，一个方法的前后
​	切入点：实际被切面织入的点
​	目标对象：切入点和连接点所属的类，被通知的对象
​	通知：切面要完成的功能
​	代理：向目标对象应用通知之后创建的对象

#### 四、AspectJ（基于java的AOP框架）

##### 	1、通知类型

```java
@Before 			// 前置通知
@AfterReturning 	 // 后置通知
@Around 			// 环绕通知
@AfterThrowing 		 // 抛出通知
@After 	// 最终 final 通知，不管是否异常，该通知都会执行无论程序是否正常执行
```

##### 2、切入点表达式（标识切面织入到哪些类的那些方法当中，通过 execution 函数，可以定义切点的方法切入）

​	① 语法：execution(<访问修饰符>?<返回类型><方法名>(<参数>)<异常>)	--- 如果没有这些参数可以不写，访问修饰符和java一样
​	② 特殊符号
​		* 代表 0 到多个任意字符
​		.. 放在方法参数中, 代表任意参数, 放在包名称后面表示当前包及其所有子包路径
​		+ 放在类名后, 表示当前 Java 类及其子类, 放在接口后, 表示当前接口及其实现类

```java
// 举几个常用的栗子
execution(* com.zb.dao.UserDao.*(..));	// 表示dao包下面的UserDao类下的任意方法
execution(* com.zb.dao..*(..));		// 表示dao包下面的任意类的任意方法
execution(public * *(..));		// 表示任意public修饰的方法
```

##### 3、环境搭建

​	导3个包（aspectjweaver-1.7.4.RELEASE.jar、spring-aspects-5.2.13.RELEASE.jar、aopalliance-1.0.jar）

```xml
① xml配置文件的方式
<!-- 配置AOP增强 -->
	<aop:config>
		<!-- 切入点 -->
		<aop:pointcut expression=" 切入点表达式 " id="book" />
		<!-- 配置切面 -->
		<aop:aspect ref="bookProxy">
			<!-- 织入：增强作用到方法上before:提前通知 -->
			<aop:before method=" 要织入的Bean对象名 " pointcut-ref=" 切入点名 "/>
		</aop:aspect>
	</aop:config>

② 注解方式
	<!-- 包扫描器 -->
	<context:component-scan base-package="com.zb"></context:component-scan>

	<!-- 自动代理 -->
	<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
```

```java
③ java配置文件的方式
  @Configuration
  @ComponentScan(basePackages = {"要扫描的包路径"})
  @EnableAspectJAutoProxy(proxyTargetClass = true)		// 自动代理对象
```



## 四、Spring事务

#### 一、事务概述

​	概念：**一系列的动作，可以当成一个单独的工作单元，这一系列的动作要么全部完成要么不完成**
​	

#### 二、事务的属性

​	① 原子性：某个一系列的动作要么全部完成要么不完成
​	② 一致性：一旦事务动作完成，事务就被提交，数据和资源就处于一种满足业务规则的状态中
​	③ 隔离性：每个事务都应该是独立的，这样可以防止数据的损坏
​	④ 持久性：一旦事务完成，无论发生什么错误，结果都不应该受到影响，事务的结果会写到持久化储存器中

#### 三、事务的分类

​	① 编程式事务：将事务管理的代码嵌入到业务方法中来控制事务的提交和回滚，这样降低了代码的重用性，提高了耦合度，不便于维护
​	② 声明式事务：将事务管理的代码分离出来，Spring通过Spring AOP框架来实现作为一个切面来使用，使其模块化，这样提高了代码的重用性，降低了代码的耦合度，便于后期维护
​	

#### 四、Spring事务实现 --- 注解方式

###### 	第一步：导入事务包（spring-tx-5.2.13.RELEASE.jar）

###### 	第二步：xml配置

```xml
	<!-- 配置bean的扫描器 -->
		<context:component-scan base-package="com.zb"></context:component-scan>
	<!-- 注册JDBCTemplte -->
	<bean id="temp" class="org.springframework.jdbc.core.JdbcTemplate" p:dataSource-ref="dataSource"></bean>

	<context:property-placeholder location="classpath:db.properties"/>

	<!-- 注册数据源 -->
	<!-- 配置文件代码 -->
	<!--
		jdbc.driverClassName=org.springframework.jdbc.datasource.DriverManagerDataSource
		jdbc.url=jdbc:mysql:///test
		jdbc.username=root
		jdbc.password=root
	-->
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="${jdbc.driverClassName}"></property>
		<property name="url" value="${jdbc.url}"></property>
		<property name="username" value="${jdbc.username}"></property>
		<property name="password" value="${jdbc.password}"></property>
	</bean>

	<!-- 注册事务管理器 -->
	<bean id="txMgr" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"></property>
	</bean>

	<!-- 开启事务注解驱动 -->
	<tx:annotation-driven transaction-manager="txMgr"/>

```

###### 	第三步：使用注解

#### 五、事务的属性

##### 	1、事务的传播属性（7种）

​		（常用）① REQUIRED：如果有事务在运行，当前的方法就在这个事务内运行，否则，就启动一个新的事务，并在自己的事务内运行，从始至终只有一个事务
​		（常用）② REQUIRED_NEW：当前方法必须启动新事务，并在他自己的事务内运行，如果用事务正在运行，应该将这个事务挂起
​		③ SUPPORTS：如果有事务在运行，当前的方法就在这个事务内运行，否则他不可以运行在这个事务中
​		④ NOT_SUPPORTS：当前的方法不应该运行在事务中，如果有运行的事务，将它挂起
​		⑤ MANDATORY：当前的方法必须运行在事务内部，如果没有正在运行的事务，就抛出异常
​		⑥ NEVER：当前的方法不应该运行在事务中，如果有运行的事务，就抛出异常
​		⑦ NESTED：如果有事务在运行，当前的方法就应该在这个事务的嵌套事务内运行，否则，就启动一个新的事务，并在他自己的事务内运行

##### 2、事务的隔离级别

​	① 事务并发产生的问题名词解释
​		脏读：一个事务读取到了另一个事务没有提交的数据（不能容忍）
​		不可重复读：在同一个事务中两次读取同一数据，得到的内容不一样（可以容忍）
​		幻读：同一事务，用同一种操作，得到的记录数不同（可以容忍）
​	② 隔离级别
​		DEFAULT：表示隔离级别位数据库默认的（READ_COMMITTED）
​		READ_UNCOMMITTED：表示允许读取尚未提交的更改，可能导致脏读、但幻读或不可重复读
​		READ_COMMITTED：表示从已经提交的并发事务读取，可以避免脏读，但是不可重复读和幻读可能会出现
​		REPEATABLE_READ：表示事务对相同字段的多次读取的结果是一致的，这个事务持续期间禁止其他事务修改这字段，除非数据被当前事务本身改变，可以避免脏读和不可重复读，但幻读可能会出现
​		SERIALIZABLE：表示完全服从ACID的隔离级别，确保不发生脏读、不可重复读和幻读但是性能低下		ACID：事务的特征
​		mysql只支持前面4种，Oracle支持二种：READ_COMMITTED，SERIALIZABLE：

##### 六、事务中的异常（自定义的异常类）

​	最好抛出 RuntimeException。
​	如果抛出 Exception，同时在事务声明中加上@Transactional(rollbackFor = Exception.class)。
​	事务只会捕获 RuntimeException。对于Exception 不进行事务回滚