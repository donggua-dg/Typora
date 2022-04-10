# SpringMVC



## 第一个springmvc

- 新建空白maven项目，引入依赖：springmvc，servlet，jsp，mybatis，mysql，mybatis-spring，数据库连接池

- 新建Moudle，添加web支持
- web.xml中定义DispatcherServlet

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

- resource文件下创建springmvc-servlet.xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
        https://www.springframework.org/schema/mvc/spring-mvc.xsd">
<!--    自动扫描包，让指定包下的注解生效，有Ioc容器同意管理-->
    <context:component-scan base-package="controller"/>
<!--    让springmvc不处理静态资源-->
    <mvc:default-servlet-handler/>
<!--    要想@RequestMapping注解生效，必须向上下文中注册DefaultAnnotationHandlerMapping
        和AnnotationMethodHandlerAdapter实例，而annotation-driven会自动注入这两个实例-->
    <mvc:annotation-driven/>
<!--    视图解析器，自动拼接路径地址的前后缀-->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```

- 新建controller类

```java
<!--helloController-->
@Controller
public class HelloController {
    @RequestMapping("/h1")
    public String hello(Model model){
        model.addAttribute("msg","Hello,SpringMVCAnnotation!");
        return "test";
    }
}
```

- 新建视图类

```jsp
<!--web/jsp/test.jsp-->
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>视图</title>
</head>
<body>
${msg}
</body>
</html>
```



## springmvc实现转发和重定向

- 默认采用转发

```java
@Controller
public class HelloController {
    @RequestMapping("/h1")
    public String hello(Model model){
        model.addAttribute("msg","Hello,SpringMVCAnnotation!");
        return "test";
    }
}
```

- 重定向需要加 redirect

```java
@Controller
public class HelloController {
    @RequestMapping("/h1")
    public String hello(Model model){
        model.addAttribute("msg","Hello,SpringMVCAnnotation!");
        return " redirect:/test";
    }
}
```

