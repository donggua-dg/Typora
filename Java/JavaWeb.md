经典面试问题：浏览器输入url后发生了什么

![img](E:\Typora\笔记\typora图片复制存储\webp.webp)



# JavaWeb 应用结构

------

- 由一组 Servlet/JSP、HTML 文件、相关 Java 类、以及其他的资源组成，它可以在由各种供应商提供的 Servlet 容器中运行。

- webapps 的目录结构：
  - Demo：根目录，所有资源都存放在这个目录下
  - WEB-INF：存放 web.xml、lib 目录以及 classes 目录等
  - classes：存放各种 .class 文件
  - lib：存放应用所需的各种 jar 包
  - web.xml：包含应用程序的配置和部署信息

![image-20211125113829257](E:\Typora\笔记\typora图片复制存储\image-20211125113829257.png)





# web服务器

------

## 技术

**ASP**

- 微软：国内最早使用
- html中嵌入VB脚本，ASP+COM
- 一个页面都有几千行代码，页面乱
- 维护成本高



**php:**

- PHP开发速度很快，功能很强大，跨平台，代码很简单

- 无法承载大访问量的情况（局限性）



**JSP/Servlet:**

B／S：浏览器和服务器

C／S：客户端和服务器

- sun公司主推的B／S架构

- 基于Java语言的（所有的大公司，或者一些开源的组件，都是用Java写的）

- 可以承载三高问题带来的影响；
- 运行于tomcat



## 服务器

服务器是一种被动操作，用来处理用户的请求和响应信息

- IIS

微软的；asp..，weindows自带

- tomcat
  - Apache、Sun 和其他一些公司及个人共同开发而成
  - 免费开源
  - 轻量级
  - 并发访问用户不是很多的场合下，开发和调试JSP 程序的首选





# Tomcat详解

------

## 目录结构

- bin：存放各种平台下启动和关闭Tomcat的脚本文件

- conf：存放不同的配置文件

  - server.xml

    用于配置和server相关的信息，比如tomcat启动的端口号、配置host主机、配置Context

  - web.xml

    部署描述文件，描述了一些默认的servlet

    部署每个webapp时，都会调用这个文件，配置该web应用的默认servlet

  - tomcat-users.xml

    配置tomcat的用户密码与权限

  - context.xml

    定义web应用的默认行为

- lib：存放运行需要的库文件（jars）

- logs：存放日志文件

- temp：存放运行时产生的垃圾

- webapps：web资源目录，外界访问的web资源的存放目录

- work：工作目录，存放jsp编译后产生的class文件





**servicce.xml**

配置启动的端口号

- 默认端口号：8080
- mysql：3306
- http：80
- https：443

```xml
   <Connector executor="tomcatThreadPool"
               port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```

可以配置主机的名称

- 默认主机名：localhost

```xml
<Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">
```



发布一个web网站

- 将写的网站，放到tomcat指定的web应用的文件夹（webapps）下，就可以访问了
- 网站应该有的结构

```java
--webapps
  -Root
  -kuangstudy：网站的目录名
  	-WEB-INF
  		-classes：java程序
  		-lib：依赖包
  		-web.xml:网络配置文件
  	-index.html：默认首页
  	-static
  		-css
  			-style.css
  		-js
  		-img
  	-...
```





# Http

------

## http

- 超文本传输协议，通常运行在[TCP](https://baike.baidu.com/item/TCP/33012)之上

  - 文本：由可打印字符组成，可以直接阅读，常用的word文档以及文本文档

  - 超文本

    - 一种用户接口范式，用以显示文本及与文本相关的内容，

    -  其中的文字包含有可以链接到其他字段或者文档的超文本链接，允许从当前阅读位置直接切换到超文本链接所指向的文字
    - 网页属于超文本

- 端口：80

## https

- 安全超文本传输协议，在 HTTP与 TCP 之间

- http数据不安全，数据明文传送和缺乏消息完整性的检测

- 在HTTP的基础上通过**传输加密和身份认证**保证了传输过程的安全性
- 两部分组成：HTTP + SSL / TLS
- 端口：443



## http1.0和1.1的区别

- 1.0：短链接，请求一次连接一次

  1.1：长连接，同一个tcp连接可以有多次请求，无请求后才断开

- 1.1：增加了与身份认证、状态管理和Cache缓存等机制相关的请求头和响应头

- 1.1：增加了host字段，在1.0的基础上加入了一些cache的新特性



## http请求

一个HTTP请求到服务器的请求消息包括以下4个部分：

- 请求行（request line）
- 请求头部（header）
- 空行
- 请求数据

![image-20211117010148899](E:\Typora\笔记\typora图片复制存储\image-20211117010148899.png)



#### 请求行

- 由三部分组成：请求方法，请求URL（不包括域名），HTTP协议版本

- 请求方法：get  post  head  put  delete  options  trace  connect，常用 get 和 post
  - get
    - 传递参数长度受限制，不适合拿来传递大量数据，但高效
    - 会在浏览器的URL地址栏显示数据，不安全
    - 一般的HTTP请求大多都是GET
  - post
    - 可以传输大量数据，对数据量没有限制
    - 传递的数据封装在HTTP请求数据中，以名称/值的形式出现，不高效
    - 不会在浏览器的URL地址栏显示数据，安全
    - 表单的提交用的是POST

```
Request URL:请求地址
Request Method:请求方法
Status code:状态码
Remote Address:远程地址
```

![](E:\Typora\笔记\typora图片复制存储\image-20211116234322492.png)

#### 请求头部

请求头部由关键字/值对组成，每行一对

```
Accept：客户端希望接受的数据类型
Content-Type：发送端发送的实体数据的数据类型
accept-encoding：支持的编码格式
accept-language：告诉浏览器，它的语言环境
cache-control：缓存控制
connection：请求完成是断开还是保持连接
User-Agent：产生请求的浏览器类型
Host : 请求的主机名，允许多个域名同处一个IP地址，即虚拟主机
...
```

![image-20211117005637755](E:\Typora\笔记\typora图片复制存储\image-20211117005637755.png)



#### 空行

请求头之后是一个空行，通知服务器以下不再有请求头



#### 请求体

GET没有请求数据，POST有

与请求数据相关的最常使用的请求头是 Content-Type 和 Content-Length 



## http响应

TTP响应也由四个部分组成，分别是：状态行、消息报头、空行和响应正文

- 状态行组成：服务器HTTP协议版本，响应状态码，状态码的文本描述
- 状态码
  - 1xx：表示请求已接收，继续处理
  - 2xx：表示请求已被成功接受，处理，（200 OK：客户端请求成功）
  - 3xx：重定向
  - 4xx：客户端错误
    - 404 Not Found：请求资源不存在
  - 5xx：服务器端错误，服务器未能实现合法的请求

```
Accept：客户端希望接受的数据类型
accept-encoding：支持的编码格式
accept-language：告诉浏览器，它的语言环境
cache-control：缓存控制
connection：请求完成是断开还是保持连接
Host : 请求的主机名
Refresh：告诉浏览器，多久刷新一次
Location：网页重定向
...
```

![image-20211116234450061](E:\Typora\笔记\typora图片复制存储\image-20211116234450061.png)





# Servlet

------

## 1	什么是servlet

- 一种基于 Java 技术的 Web 组件，运行在服务器端，由 Servlet 容器管理，用来生成动态的 Web 内容

- Servlet 程序其实就是一个按照 Servlet 规范编写的 Java 类

![image-20211125105010418](E:\Typora\笔记\typora图片复制存储\image-20211125105010418.png)

- Web 服务器是整个动态网站的“大门”
  -  HTTP 请求首先到达 Web 服务器，
  - 服务器判断请求是静态还是动态：静态直接返回，动态交给servlet处理



## 2	servlet作用

- 实现 Servlet 规范定义的各种接口和类，为 Servlet 的运行提供底层支持；
- 管理用户编写的 Servlet 类，以及实例化以后的对象；
- 提供 HTTP 服务，相当于一个简化的服务器。



## 3	三种创建方式

![image-20211125110433434](E:\Typora\笔记\typora图片复制存储\image-20211125110433434.png)

- Servlet接口
  -  Servlet API 的核心接口，所有的 Servlet 类都直接或间接地实现了这一接口
- GenericServlet抽象类
  -  实现了 Servlet 接口，提供了除 service() 方法以外的其他四个方法的简单实现
- HttpServlet抽象类
  - 继承了 GenericServlet 抽象类，用于开发基于 HTTP 协议的 Servlet 程序
  -  编写的 Servlet 类都继承自 HttpServlet



## 4	生命周期

三个阶段

- 初始化阶段：init()
- 运行时阶段：service()
- 销毁阶段：destory()

![image-20211126001125903](E:\Typora\笔记\typora图片复制存储\image-20211126001125903.png)



## 5	servlet配置

### xml

**单一映射**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         id="WebApp_ID" metadata-complete="false" version="4.0">
    <servlet>
      	<!--注册servlet-->
        <servlet-name>MyServlet</servlet-name>
        <servlet-class>net.biancheng.www.MyServlet</servlet-class>
    </servlet>
        <!--映射路径-->
    <servlet-mapping>
        <servlet-name>MyServlet</servlet-name>
        <url-pattern>/myServlet</url-pattern>
    </servlet-mapping>
</web-app>
```

**多重映射**

```xml
    <servlet>
        <servlet-name>MyServlet</servlet-name>
        <servlet-class>net.biancheng.www.MyServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>MyServlet</servlet-name>
        <url-pattern>/myServlet</url-pattern>
        <url-pattern>/myServlet2</url-pattern>
    </servlet-mapping>
```



### 注解 （@WebServlet ）

- 直接在类名或属性或方法上面标注

```java
@WebServlet("/MyServlet","/MyServlet2","...") //可写多个路径，用逗号分隔开
public class MyServlet extends HttpServlet{
  ...
}
```

- 注意事项：
  - 实现 Serlvet 接口或继承 GenericServlet 的 Servlet 类无法使用 @WebServlet 注解
  - 同时使用注解和xml配置同一个servlet，如果name相同，注解会被无视掉



###  注解 和 web.xml 的优缺点

- 注解
  - 优点：直接在 Servlet 类中使用，代码量少，配置简单
  - 缺点：Servlet 较多时，每个 Servlet 的配置分布在各自的类中，不便于查找和修改
- web.xml
  - 优点：集中管理 Servlet 的配置，便于查找和修改
  - 缺点：配置繁琐，代码冗余，可读性不强



## 6	虚拟路径匹配规则

Servlet 容器接收到请求后，容器会将请求的 URL 减去当前应用的上下文路径，使用剩余的字符串作为映射 URL 与 Servelt 虚拟路径进行匹配，匹配成功后将请求交给相应的 Servlet 进行处理



- Servlet 虚拟路径匹配规则有以下 4 种：

1. 完全路径匹配
2. 目录匹配
3. 扩展名匹配
4. 缺省匹配（默认匹配）

![image-20211126003927888](E:\Typora\笔记\typora图片复制存储\image-20211126003927888.png)



## 7	ServletContext（经典白学）

- Servlet 容器启动时创建，每个web应用唯一，该对象一般被称为“Servlet 上下文”
- 生命周期：容器启动时开始，到容器关闭或应用被卸载时结束
- 作用：
  - 获取上下文初始化参数
  - 实现不同 Servlet 之间的数据通讯




**获取上下文初始化参数**

使用 ServletContext 对象获取 Web 应用的上下文初始化参数，分为 2 步：

- web.xml中<context-param>元素设置上下文初始化参数

```xml
<context-param>
    <param-name>url</param-name>
    <param-value>jdbc：mysql：//localhost：3306/mybatis</param-value>
</context-param>
```

- 调用接口中方法获取初始化参数

```java
ServletContext context = this.getServletContext();
String url = context.getInitParameter("url");
resp.getWriter().print(url);
```



**实现数据通讯**

