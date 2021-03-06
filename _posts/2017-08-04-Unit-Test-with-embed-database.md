---
layout: post
title: "Junit进行内嵌数据库单元测试"
---

现在很多网络应用都是基于Spring开发的，前面的文章中提到的纯粹的单元测试UT（@RunWith(MockitoJUnitRunner.class)）运行的非常快，
而集成测试IT由于引入了很多外部的资源和真实的数据库，运行又会比较慢，所以，引入了组件测试CT的概念。

在CT中，要求测试的案例仍然需要在1秒左右的级别完成，所以，一般情况想要求Mock掉以来的外部组件或者硬件，同时，使用内嵌数据库来
代替真实的数据库，这样既满足了尽量大的在组件级别集成系统中的被测单元，有满足的对测试案例运行性能的要求。

整体来说，要进行CT级别的测试，开发者需要解决如下几个问题：
* 如何在Spring的注解盛行的情况下顺利的Mock掉一些依赖的外部组件
* 如何平滑的将内嵌数据库集成到测试中，有不改动以后的代码和配置

### 测试配置 vs 生产配置

为了尽量减少对生产配置文件的影响，建议如果有需要可以允许为测试复制一份专门用于测试的配置文件。
放在test文件夹下的与src文件加下的目录结构对应的位置即可。有一些项目中采取了通过源代码进行配置的情况，
此时一些启动加载的源代码，如果需要也可以有一份专门用于测试的。

### Spring注入服务和组件的Mock问题


    <bean id="passUtil" class="org.mockito.Mockito" factory-method="mock">
        <constructor-arg value="com.test.demo.PasswordUtil" />
    </bean>
    <bean id="hsmService" class="org.mockito.Mockito" factory-method="mock">
        <constructor-arg value="com.test.demo.HsmService" />
    </bean>



### 用内嵌数据库模拟真实数据库的问题

    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
          init-method="init" destroy-method="close">
        <property name="url" value="jdbc:h2:~/test;MODE=MySQL"/>
        <!--<property name="filters" value="${dataSource.filters}"/>-->
        <!--<property name="connectionProperties" value="${dataSource.connectionProperties}"/>-->
    </bean>

    <!--<jdbc:embedded-database id="dataSource" type="H2" >-->
        <!--<jdbc:script location="classpath:schema.sql"/>-->
        <!--<jdbc:script location="classpath:data.sql"/>-->
    <!--</jdbc:embedded-database>-->

    <jdbc:initialize-database data-source="dataSource" ignore-failures="ALL">
        <jdbc:script location="classpath:schema.sql"/>
        <jdbc:script location="classpath:data.sql"/>
    </jdbc:initialize-database>
    
    <dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>4.1.4.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>com.h2database</groupId>
			<artifactId>h2</artifactId>
			<version>1.4.187</version>
		</dependency>