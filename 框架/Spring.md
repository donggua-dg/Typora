# 1	Spring

## 1.1	简介

- 背景：EJB学习成本高，开发效率低，Spring 由 Rod Johnson 创立，2004 年发布了 Spring 框架的第一版，其目的是用于简化企业级应用程序开发的难度和周期。

- Spring理念：为解决企业应用开发复杂性，使现有技术更容易使用，整合了现有的技术框架！

  在实际开发中，服务器端通常采用三层体系架构，分别为表现层（web）、业务逻辑层（service）、持久层（dao）。

  Spring 致力于 Java EE 应用各层的解决方案，对每一层都提供了技术支持。在表现层提供了与 Spring MVC、Struts2 框架的整合，在业务逻辑层可以管理事务和记录日志等，在持久层可以整合 MyBatis、Hibernate 和 JdbcTemplate 等技术。这就充分体现出 Spring 是一个全面的解决方案，对于已经有较好解决方案的领域，Spring 绝不做重复的事情。



- SSH：Struct2 + Spring +Hibernate
- SSM：SpringMVC + Spring + Mybatis



maven依赖

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.12</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.12</version>
</dependency>
```



## 1.2	优点和弊端

- 开源免费

- 轻量级的、非入侵式

  侵入式：会对项目产生影响

  非侵入式：不会对项目产生任何影响

- 控制反转（IOC）、面向切面编程（AOP）！

  IoC：对象的创建权交给 Spring，不再人为来new

  AOP ：封装多个类的公共行为，减少重复代码，降低模块间耦合度。

- 支持事务的处理，对框架整合的支持，支持所有主流框架！



**弊端：spring发展太久，整合太多技术，配置繁琐，人称:“配置地狱”，在spring boot出现后好转。**



Spring 框架具有以下几个特点。

1）方便解耦，简化开发

2）方便集成各种优秀框架

3）降低 Java EE API 的使用难度

4）方便程序的测试

5）AOP 编程的支持

6）声明式事务的支持

只需要通过配置就可以完成对事务的管理，而无须手动编程。



==总结一句话：Spring就是一个轻量级的控制反转（IOC）和面向切面编程（AOP）的框架！==



## 1.3	组成



![image-20211026222327171](E:\Typora\笔记\typora图片复制存储\image-20211026222327171.png)

1. Data Access/Integration（数据访问／集成）
2. Web模块
3. AOP、Aspects、Instrumentation和Messaging
5. Test模块


## 1.4	扩展

- spring boot
  - 一个快速开发的脚手架，旨在通过最少的spring前期配置使你尽快启动并运行
  - 基于spring boot可以快速开发单个微服务
  - 约定大于配置！

- spring cloud
  - spring cloud是基于spring boot实现的



因为现在大多数公司都在使用spring boot进行快速开发，学习spring boot的前提，需要完全掌握spring和springMVC！



# IOC容器

-  Spring 的核心，也可以称为 Spring 容器。
- 作用：管理对象的实例化和初始化，以及对象从创建到销毁的整个生命周期。



Spring 提供 2 种不同类型的 IoC 容器

- BeanFactory 容器
  - 管理 Bean 的工厂，负责初始化各种 Bean，并调用它们的生命周期方法
  - 采用懒加载（lazy-load），容器启动快

```java
Resource resource = new ClassPathResource("applicationContext.xml"); 
BeanFactory factory = new XmlBeanFactory(resource);
```

- ApplicationContext 容器
  -  继承了 BeanFactory 接口，对象在启动容器时加载
  - 在 BeanFactory 的基础上增加了很多企业级功能，例如 AOP、国际化、事件支持等。
  - ApplicationContext 接口有两个常用的实现类:

```java
//1.ClassPathXmlApplicationContext(常用)
// 该类从类路径 ClassPath 中寻找指定的 XML 配置文件
ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
Hello hello = (Hello) context.getBean("helloWorld");

//2.FileSystemXmlApplicationContext
// 该类从指定的文件系统路径中寻找指定的 XML 配置文件，以获取类路径之外的资源
ApplicationContext applicationContext = new FileSystemXmlApplicationContext(String configLocation);
```



# Bean的定义

- 什么是bean？

​	由 Spring IoC 容器管理的对象称为 Bean，Bean 根据 Spring 配置文件中的信息创建。

- Spring 配置文件支持两种格式，即 XML 文件格式和 Properties 文件格式。
  -  Properties：以 key-value 键值对形式存在，只能赋值，不能进行其他操作，适用于简单的属性配置。
  - XML（常用）：树形结构，比 Properties 灵活，文件结构清晰，但是内容繁琐，适用于大型复杂的项目。

- 编写bean.xml文件（beans可通过spring官网-project-Spring Framework-learn-core 获取）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

   <bean id="helloWorld" class="net.biancheng.HelloWorld">
        <property name="message" value="Hello World!" />
   </bean>
</beans>
```

- 实现类编写：


```java
ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
Hello hello = (Hello) context.getBean("helloWorld");
```



# 依赖注入

依赖注入：调用者调用实例时，容器自动将需要的对象实例注入给调用者

- 未注入的属性被调用将显示为null

**依赖注入主要有两种实现方式：**



## 构造函数注入

- 通过调用带参数的构造函数实现依赖注入，每个参数代表一个依赖。
- 使用 <constructor-arg>标签实现构造函数注入

```xml
<!--通过参数下标赋值（下标从0开始）-->
<bean id="hello" class="com.kuang.pojo.Hello">
    <constructor-arg index="0" value="string"/>
    <constructor-arg index="1" value="10"/>
</bean>

<!--通过属性名赋值（常用）-->
<bean id="hello" class="com.kuang.pojo.Hello">
    <constructor-arg name="name" value="string"/>
    <constructor-arg name="age" value="10"/>
</bean>
```

## setter注入（重点）

```xml
<bean id="address" class="com.kuang.pojo.Address"/>

<bean id="hello" class="com.kuang.pojo.Hello">
   <!--普通值注入-->
   <property name="message" value="Hello World!" />
   <!--bean注入-->
   <property name="address" ref="address(已注册的bean标识符)" />
   <!--数组注入-->
   <property name="数组名">
  	<array>
    	<value></value>
      <value></value>
      ...
    </array>
   </property>
    <!--List注入，允许重复-->
   <property name="List集合名">
  	 <list>
    	 <value></value>
       <value></value>
      ...
     </list>
    </property>
  	<!--Map注入-->
    <property name="Map名">
  	  <map>
    	 <entry key="" value=""/>
       <entry key="" value=""/>
      ...
       </map>
     </property>
     <!--set注入，不允许重复-->
   <property name="">
  	 <set>
    	 <value></value>
       <value></value>
      ...
     </set>
    </property>
    <!--null空值注入-->
   <property name="属性名">
  	 <null/>
    </property>
    <!--Properties配置注入-->
   <property name="属性名">
  	 <props>
     	<prop key="driver">xx</prop>
      <prop key="url">xx</prop>
      <prop key="username">xx</prop>
      <prop key="password">xx</prop>
     </props>
    </property>
</bean>

```

-

# Bean作用域

Spring 容器在初始化一个 Bean 实例时，同时会指定该实例的作用域。Spring 5 支持以下 6 种作用域

- singleton

​	单例模式，表示在 Spring 容器中只有一个 Bean 实例对象，全局唯一。

- prototype

​	原型模式，每请求一次就会在容器中创建一个新的bean实例

- request（web开发中才能使用）

​	每次 HTTP 请求，容器都会创建一个 Bean 实例。该作用域只在当前 HTTP Request 内有效。

- session（web开发中才能使用）

​	同一个session 共享一个 Bean 实例，不同的Session 使用不同的 Bean 实例。该作用域仅在当前 HTTP Session 内有效。

- application（web开发中才能使用）

​	同一个 Web 应用共享一个 Bean 实例，该作用域在当前 ServletContext 内有效。

- websocket

​	websocket 的作用域是 WebSocket ，即在整个 WebSocket 中有效。

```xml
<bean id="hello" class="com.kuang.pojo.Hello" scope="singleton"></bean>
```



# Bean的自动装配

自动装配就是指 Spring 容器在不使用 <constructor-arg> 和<property> 标签的情况下，可以自动装配（autowire）相互协作的 Bean 之间的关联关系，将一个 Bean 注入其他 Bean 的 Property 中。

## xml

```xml
<bean id="cat" class="com.kuang.pojo.Cat" ></bean>
<bean id="dog" class="com.kuang.pojo.Dog"></bean>

<!--不使用自动装配-->
<bean id="hello" class="com.kuang.pojo.Hello">
	<property name="name" value=""/>
  <property name="cat" ref="cat"/>
  <property name="dog" ref="dog"/>
</bean>

<!--使用自动装配-->

<!--byName-->
<bean id="hello" class="com.kuang.pojo.Hello" autowire="byName">
	<property name="name" value=""/>
</bean>
<!--byType-->
<!--使用byType，注入的bean可以省略id-->
<bean class="com.kuang.pojo.Cat" ></bean>
<bean class="com.kuang.pojo.Dog"></bean>

<bean id="hello" class="com.kuang.pojo.Hello" autowire="byType">
	<property name="name" value=""/>
</bean>

```



**使用自动装配需要配置 <bean> 元素的 autowire 属性。autowire 属性有五个值：**

| 名称                        | 说明                                                         |
| --------------------------- | :----------------------------------------------------------- |
| no                          | 默认值，表示不使用自动装配，Bean 依赖必须通过 ref 元素定义。 |
| byName                      | 根据 Property 的 name 自动装配，如果一个 Bean 的 name 和另一个 Bean 中的 Property 的 name 相同，则自动装配这个 Bean 到 Property 中。 |
| byType                      | 根据 Property 的数据类型（Type）自动装配，如果一个 Bean 的数据类型兼容另一个 Bean 中 Property 的数据类型，则自动装配 |
| constructor                 | 类似于 byType，根据构造方法参数的数据类型，进行 byType 模式的自动装配。 |
| autodetect（3.0版本不支持） | 如果 Bean 中有默认的构造方法，则用 constructor 模式，否则用 byType 模式。 |



**自动装配的优缺点**

- 优点
  - 自动装配只需要较少的代码就可以实现依赖注入。

- 缺点
  - 不能自动装配简单数据类型，比如 int、boolean、String 等
  - 相比较显示装配，自动装配不受程序员控制



## 注解

在 Spring 中，尽管可以使用 XML 配置文件实现 Bean 的装配工作，但如果应用中 Bean 的数量较多，会导致 XML 配置文件过于臃肿，从而给维护和升级带来一定的困难。

使用方法：

```xml
 <!--1.添加context命名空间-->
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context  http://www.springframework.org/schema/context/spring-context.xsd
">
   
  <!--2.开启注解装配（重要）-->
 <context:annotation-config/>
  
 <bean id="cat" class="com.kuang.pojo.Cat"/>
 <bean id="dog" class="com.kuang.pojo.Dog"/>
  
<bean id="people" class="com.kuang.pojo.People"/>
  

```



**Spring 中常用的注解如下：**

1. @Autowired（常用）

- 可以应用到 Bean 的属性变量、属性的 setter 方法、非 setter 方法及构造函数等
- 默认按照 Bean 的类型（byType）进行装配

- 应用到属性变量，可以不写set方法

```java
 @Autowired
 private Cat cat;
 @Autowired
 private Dog dog;
 public Cat getCat() {
        return cat;
    }

 public Dog getDog() {
        return dog;
    }
```

2. @Qualifier

- @Autowired 	默认按照 Bean 的类型进行装配
- 当容器（xml）中出现多个相同类型的bean，但id不同时，使用@Qualifier可以指定具体的id的bean注入实例。
- 需与 @Autowired 注解配合使用

```xml
 <bean id="cat" class="com.kuang.pojo.Cat"/>
 <bean id="cat222" class="com.kuang.pojo.Cat"/>
 <bean id="dog" class="com.kuang.pojo.Dog"/>
 <bean id="dog222" class="com.kuang.pojo.Dog"/>
```

```java
 @Autowired
 @Qualifier（value=“cat222”）
 private Cat cat;
 @Autowired
 @Qualifier（value=“dog222”）
 private Dog dog;
 public Cat getCat() {
        return cat;
    }

    public Dog getDog() {
        return dog;
    }
```

3.  @Resource

- 作用与 Autowired 相同，区别在于
  -  @Autowired 默认按照 Bean 类型（byType）装配
  -  @Resource 默认按照 Bean 实例名称（byName）进行装配。

- @Resource 中有两个重要属性：name 和 type。
  - 如果指定 name 属性，则按实例名称进行装配
  - 如果指定 type 属性，则按 Bean 类型进行装配
  - 不指定，先按 byName，后按 byType 进行装配
  - 两个属性都不匹配，抛出 NoSuchBeanDefinitionException 异常



# 注解开发

- applicationContext.xml

```xml
<beans 
   xmlns:...
       ...>
  <!-- 扫描指定的包，让包下的注解生效-->
<context:component-scan base-package="con.kuang.xx"/>
<context:annotation-config/>
</beans>
```

- @Component ：相当于在xml中注册一个bean

```java
//标记这个类为一个组件，加载进容器中，相当于在xml中注册了一个bean
//等价于 <bean id="user" class="com.kuang.pojo.User"/>
@Component  
public class User{
  ...
}
```

- 属性注入（@Value）

```java
@Component  
public class User{
  private String name;
  
  @Value("kuangshen")
  public void serName(String name){
    this.name=name;
  //等价于  <proterty name="name" value="kuansghen"/>
  }
}
```

- @Component   的衍生注解
  - 按照mvc三层架构分层
    - dao（@Repository）
    - service（@Service）
    - controller（@Conteoller）
  - 四个注解的功能相同，都相当于将一个类注册到spring容器中，装配bean







# 代理模式

- 经典设计模式之一
- 官方的定义：“为其他对象提供一种代理以控制对这个对象的访问”
- A类自己做一件事，在使用代理之后，A类不直接去做，而是由A类的代理类B来去做



## 静态代理

角色分析：

- 抽象角色：一般会使用接口或者抽象类来解决
- 真实角色：被代理的角色
- 代理角色：代理真实角色，代理真实角色后，我们一般会做一些附属操作
- 客户：访问代理对象的人！

代码步骤：

1. 接口

```java
//租房行为
public interface Rent {
    public void rent();
}
```

2. 真实角色

```java
//房东要出租房子
public class Host implements Rent{
    @Override
    public void rent() {
        System.out.println("房东要出租房子");
    }
}
```

3. 代理角色

```java
//中介
public class Proxy {
    private Host host; //定义一个代理对象

    public Proxy() {
    }
		//有参构造获取代理对象
    public Proxy(Host host) {
        this.host = host;
    }
    public void rent(){
        host.rent();
        seeHouse();
    }
//    代理类还可自己扩展功能
    public void seeHouse(){
        System.out.println("中介带你看房");
    }
}
```

4. 客户端访问代理角色

```java
public class Client {
    public static void main(String[] args) {
        Host host = new Host();
        Proxy proxy=new Proxy(host);
        proxy.rent();
    }
}
```



优点：

1.  实现松散耦合
2. 做到在不修改目标对象的功能前提下，对目标功能扩展

缺点：

- 如果项目中有多个类，则需要编写多个代理类，工作量大，不好修改，不好维护，不能应对变化





## 动态代理

- jdk动态代理（重点）
- CGLIB动态代理

区别：

- jdk动态代理
  - 利用反射机制生成一个实现代理接口的匿名类，在调用具体方法前调用InvokeHandler来处理
  - 只能对实现了接口的类生成代理，而不能针对类

- cglib动态代理
  - 利用asm开源包，对代理对象类的class文件加载进来，通过修改其字节码生成子类来处理
  - 针对类实现代理，主要是对指定的类生成一个子类，覆盖其中的方法

- jdk 比 cglib 速度快

### jdk动态代理

实现 InvocationHandler 接口，重写 invoke 方法，客户端使用 Java.lang.reflect.Proxy 类产生动态代理类的对象。

编写一个代理类，继承InvocationHandler接口，重写 invoke

```java
public class JdkProxy implements InvocationHandler {
    private Object target; // 定义需要代理的目标对象
  	//获取代理目标对象
    private void setTarget(Object target){
      this.target=target;
    }
  
  	//获取代理对象方法，生成得到代理类（固定样式）
  	private Object getJDKProxy(Object target) {      
        return Proxy.newProxyInstance(target.getClass().getClassLoader(), target.getClass().getInterfaces(),
                this);
    }
  
    //处理代理实例，并返回结果（固定样式）
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        Object result = method.invoke(target, args);
      	return result;
    }
```

实现类

```java
 public static void main(String[] args) {
        JdkProxy jdkProxy = new JdkProxy();    // 实例化JDKProxy对象
        UserManager user = (UserManager) jdkProxy.getJDKProxy(new UserManagerImpl());    // 获取代理对象
}
```

定义	UserManager（用户管理接口）、UserManagerImpl（用户管理接口实现类）

```java
 public interface UserManager {
    // 新增用户抽象方法
    void addUser(String userName, String password);
    // 删除用户抽象方法
    void delUser(String userName);
}
  public class UserManagerImpl implements UserManager {
    @Override
    public void addUser(String userName, String password) {
        System.out.println("正在执行添加用户方法");
        System.out.println("用户名称: " + userName + " 密码: " + password);
    }
    @Override
    public void delUser(String userName) {
        System.out.println("正在执行删除用户方法");
        System.out.println("用户名称: " + userName);
    }
}
```







# AspectJ AOP（面向切面编程）

- 和 OOP（面向对象编程）类似，也是一种编程思想

- 采取横向抽取机制（动态代理），取代了传统纵向继承机制的重复性代码
- 主要体现在事务处理、日志管理、权限控制、异常处理等方面
- 作用：不修改源代码的前提下，为系统中的业务组件添加某种通用功能
- AOP 就是代理模式的典型应用



使用 AspectJ 开发 AOP 通常有以下 2 种方式：

- 基于 XML 的声明式 AspectJ
- 基于 Annotation 的声明式 AspectJ



## XML 声明式 

通过 Spring 配置文件的方式来定义切面、切入点及通知，而所有的切面和通知都必须定义在 aop:config 元素中

步骤：

1. 导入Aspectjweaver依赖

```xml
<dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.6</version>
            <scope>runtime</scope>
        </dependency>

```

2. 导入spring aop命名空间

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
    http://www.springframework.org/schema/aop
    http://www.springframework.org/schema/aop/spring-aop-3.0.xsd ">
    
    ...
</beans>
```

3. 定义切面，切入点，通知

```xml
<aop:config>
  <!--定义切面，新建切面类，ref 指定要引用的切面类-->
    <aop:aspect id="diy" ref="DiyPointCut">
      	<!--定义切入点-->
         <aop:pointcut id="point"
                       //* com  中间要有空格
        expression="execution(* com.kuang.service.UserServiceImpl.*(..))"/>
     		 <!--定义通知类型-->
      	<aop:before method="" pointcut-ref="point"/>
    </aop:aspect>
</aop:config>
```

```xml
<!--		execution基本语法格式 
execution("修饰符（可忽略） *切入的包名.类名.指定方法（指定参数） 指定抛出的异常类型（可忽略）")
比如：
execution（"*包名.*.*（..）")  表示匹配包中任意类的任意方法
execution（"*包名.类.*（..）")  表示匹配包中指定类的任意方法
-->
```



**通知类型**

AspectJ 支持 5 种类型的 advice：

- before：方法执行前

- after：方法执行后

- after-returning：返回通知, 在方法返回结果之后执行

- around：方法执行前后都执行

- after-throwing：异常通知, 在方法抛出异常之后

  

## 基于注解开发

启用 @AspectJ 注解有以下两种方法：

1）使用@Configuration和@EnableAspectJAutoProxy注解

```java
@Configuration 
@EnableAspectJAutoProxy
public class Appconfig {
}
```

2）基于XML配置

在 XML 文件中添加以下内容启用 @AspectJ

```xml
<aop:aspectj-autoproxy>
```



**定义切面@Aspect	切入点@Pointcut	通知advice**

AspectJ 类和其它普通的 Bean 一样，可以有方法和字段，不同的是 AspectJ 类需要使用 @Aspect 注解

```java
//定义切面
@Aspect
public class AspectModule {
  // 切入点，要求：方法必须是private，返回值类型为void，名称自定义，没有参数
	@Pointcut("execution(*com.kuang..*.*(..))")
	private void myPointCut(){}
  //定义通知
  @Before("myPointCut()")
	public void beforeAdvice(){
    	...
		}
}
```





# 事务

事务（Transaction）是面向关系型数据库（RDBMS）企业应用程序的重要组成部分，用来确保数据的完整性和一致性。

- 事务具有以下 4 个特性：ACID 
  - 原子性（Atomicity）：事务中包括的动作要么都执行要么都不执行
  - 一致性（Consistency）：事务前后数据的完整性必须保持一致
  - 隔离性（Isolation）：一个事务的执行不能被其它事务干扰，并发执行的各个事务之间不能互相打扰。
  - 持久性（Durability）：持久性也称为永久性，指一个事务一旦提交，它对数据库中数据的改变就是永久性的，后面的其它操作和故障都不会对其有任何影响。

