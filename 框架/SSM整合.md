# SSM整合

- 整合思路

springmvc是spring框架的一个模块，所以springmvc和spring之间不需要整合，只需要整合spring和mybatis。

- 编写配置文件
  - db.properties
  - applicationContext.xml
  - mybatis-config.xml
  - springmvc-config.xml
  - web.xml



1. db.properties

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/boot_crm?useUnicode=true&characterEncoding=UTF-8&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC
jdbc.username=root
jdbc.password=251314fm
jdbc.maxTotal=30
jdbc.maxIdle=10
jdbc.initialSize=5
```

2. applicationContext.xml

- 配置数据库和连接池
- 配置事务和开启注解
- 配置mybatis工厂

```xml
<beans xmls:...>
  
	<!--读取db.properties-->
	<context:property-placheolder location="classpath:db.properties"/>
  
  <!-- 配置连接池数据源,使用dbcp2连接池-->
    <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource">
         <!--数据库驱动 -->
         <property name="driverClassName" value="${jdbc.driver}" />
         <!--连接数据库的url -->
         <property name="url" value="${jdbc.url}" />
         <!--连接数据库的用户名 -->
         <property name="username" value="${jdbc.username}" />
         <!--连接数据库的密码 -->
         <property name="password" value="${jdbc.password}" />
         <!--最大连接数 -->
         <property name="maxTotal" value="${jdbc.maxTotal}" />
         <!--最大空闲连接  -->
         <property name="maxIdle" value="${jdbc.maxIdle}" />
         <!--初始化连接数  -->
         <property name="initialSize" value="${jdbc.initialSize}" />
	</bean>
  
  <!--配置事务管理器-->
  <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<!-- 链接数据源-->
		<property name="dataSource" ref="dataSource" />
	</bean>
  
    <!--开启事务注解-->
  <tx:annotation-driven transaction-manager="transactionManager"/>
  
   <!--配置mybatis工厂-->
  <bean class="org.mybatis.spring.SqlSessionFactoryBean">
		<!-- 数据源 -->
		<property name="dataSource" ref="dataSource" />
		<!-- 配置MyBatis的核心配置文件所在位置 -->
		<property name="configLocation" value="classpath:mybatis-config.xml" />
	</bean>
  
   <!--配置mapper扫描器，此包下的接口即可被扫描到-->
  <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<property name="basePackage" value="com.itheima.core.dao" />
	</bean>
  
  <!-- 配置扫描Service包 -->
	<context:component-scan base-package="com.itheima.core.service"/>
		
</beans>
      
```

3. mybatis-config.xml

在spring中已经配置了数据源和mapper接口扫描器，所以在此文件中只需根据pojo类路径进行别名配置即可。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 别名定义 -->
	<typeAliases>
		<package name="com.itheima.core.po" />
	</typeAliases>
</configuration>
```

4. springmvc-config.xml

```xml
<beans xmls:...>
 
  	<!-- 配置扫描器 -->
    <context:component-scan base-package="com.itheima.core.web.controller" />

    <!-- 注解驱动-->
    <mvc:annotation-driven />
  
    <!--静态资源过滤-->
    <mvc:default-servlet-handler/>

    <!-- 配置视图解释器-->
    <bean id="jspViewResolver" class=
    "org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/jsp/" />
		<property name="suffix" value=".jsp" />
    </bean>	
  
    	//根据需要配置拦截器
    <!-- 配置拦截器 -->
		<mvc:interceptors>
    	<mvc:interceptor>
        	<mvc:mapping path="/**" />
        	<bean class="com.itheima.core.interceptor.LoginInterceptor" />
    	</mvc:interceptor>
	</mvc:interceptors>	
</beans>
```

4. web.xml

```xml
<!--加载applicationContext.xml文件 -->
		<context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>
 <!-- 配置加载Spring文件的监听器-->
		//不配置会发生异常 HTTP Status 500 - Servlet.init() for servlet springmvc threw exception
    <listener>
        <listener-class>
            org.springframework.web.context.ContextLoaderListener
        </listener-class>
    </listener>

<!-- 配置Spring MVC前端核心控制器 -->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>
            org.springframework.web.servlet.DispatcherServlet
        </servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-config.xml</param-value>
        </init-param>
        <!--服务器启动后立即加载Spring MVC配置文件 -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

 <!-- 编码过滤器 -->
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>
            org.springframework.web.filter.CharacterEncodingFilter
        </filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/</url-pattern>
    </filter-mapping>

		<!-- 系统默认页面 -->
    <welcome-file-list>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>

	
    
```





- 导入依赖
  - springmvc
  - servlet
  - jsp
  - jstl：jsp标签库
  - aspectjweaver
  - log4J：日志
  - mysql 和 dbcp2连接池
  - junit
  - mybtis
  - mybatis-spring



