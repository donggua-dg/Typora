# 传统JDBC弊端

1. 频繁创建、释放数据库链接造成系统资源浪费从而影响系统性能，如果使用数据库链接池可解决此问题。
2.  Sql 语句在代码中硬编码，造成代码不易维护，实际应用 sql 变化的可能较大， sql 变动需要改变 java代码。
3. 使用 preparedStatement 向占有位符号传参数存在硬编码，因为 sql 语句的 where 条件不一定，可能多也可能少，修改 sql 还要修改代码，系统不易维护。
4. 对结果集解析存在硬编码（查询列名）， sql 变化导致解析代码变化，系统不易维护，如果能将数据库记录封装成 pojo 对象解析比较方便。



# mybatis使用流程

------

- 导入maven依赖

- 创建实体类和接口，接口中写对应sql的方法

```java
public class User {
    private int id;
    private String name;
    private String pwd;
    public User() {}
    public User(int id, String name, String pwd) {
        this.id = id;
        this.name = name;
        this.pwd = pwd;
      //省略get，set和 toString方法
    }

```

```java
public interface UserDao {
    List<User> getUserList();
  	User getUserById（int id）;
  	...
}
```

- 创建对应接口的mapper.xml

```xml
<!--UserMapper.xml-->
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace 绑定一个对应的Dao/Mapper接口-->
<mapper namespace="com.kuang.dao.UserDao">
  	<!-- id：namespace下的方法   resultType：sql语句返回值的类型-->
    <select id="getUserList" resultType="com.kuang.pojo.User">
        select * from mybatis.user
    </select>
  	<!-- parameterType传入的参数类型-->
  	<select id="getUserById" parameterType="int" resultType="com.kuang.pojo.User">
        select * from mybatis.user where id = ${id}
    </select>
</mapper>
```

- 创建核心配置文件mybatis-config.xml
  - 配置运行环境
  - 注册mapper.xml


```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 配置mybatis运行环境 -->
    <environments default="development">
        <environment id="development">
            <!-- 使用JDBC的事务管理 -->
            <transactionManager type="JDBC" />
            <dataSource type="POOLED">
                <!-- MySQL数据库驱动 -->
                <property name="driver" value="com.mysql.jdbc.Driver" />
                <!-- 连接数据库的URL -->
                <property name="url"
                    value="jdbc:mysql://localhost:3306/user?characterEncoding=utf8" />
                <property name="username" value="root" />
                <property name="password" value="251314fm" />
            </dataSource>
        </environment>
    </environments>
    <!-- 将mapper文件加入到配置文件中 -->
    <mappers>
        <mapper resource="com/kuang/mapper/UserMapper.xml" />
    </mappers>
</configuration>
```

- 编写测试类

```java
public class Test {
  			// 需要抛出io异常
    public static void main(String[] args) throws IOException {
        // 固定样式
      	//读取配置文件
      	//根据配置文件创建SqlSessionFactory
        //通过SqlSessionFactory创建SqlSession
        InputStream inputStream = Resources.getResourceAsStream("mybatis-config.xml");
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();
     	  // 获取接口和方法
        UserDao userDao = sqlSession.getMapper(UserDao.class);
      	//新建一个对象保存查询结果
        List<User> userList = userDao.getUserList();
        for (User user : userList) {
            System.out.println(user);
        }  
        // 关闭 SqlSession
        sqlSession.close();
    }
}
```



# 核心对象

MyBatis 有三个基本要素：

- 核心接口和类
- MyBatis核心配置文件（mybatis-config.xml）
- SQL映射文件（mapper.xml）

![image-20211121010708901](E:\Typora\笔记\typora图片复制存储\image-20211121010708901.png)



## SqlSessionFactoryBuilder

- 主要作用：根据配置信息或者代码生成 SqlSessionFactory

- 生命周期和作用域：用过即丢，创建 SqlSessionFactory 对象后，这个类就不存在了
  - 最佳范围就是存在于方法体内，也就是局部变量



## SqlSessionFactory

- 是工厂接口而不是现实类
- 所有的 MyBatis 应用都以 SqlSessionFactory 实例为中心
- 主要功能：创建 SqlSession
- 生命周期和作用域：一旦创建，始终存在（整个应用程序过程中），作用域是 Application
- 单例模式（指在运行期间有且仅有一个实例）



## SqlSession

- 用于执行持久化操作的对象，类似于 JDBC 中的 Connection
- 提供了面向数据库执行 SQL 命令所需的所有方法，可以实现增删改查
- 生命周期和作用域：
  - 访问一次数据库就创建一个 SqlSession 对象，用完就销毁
  - SqlSession 实例不能被共享，也不是线程安全的
  - 作用域范围是：request 



# 核心配置文件

------

**mybatis-config.xml 文件中的元素节点是有一定顺序的，节点位置必须按位置排序，否则会编译错误**

```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration><!-- 配置 -->
    <properties /><!-- 属性 -->
    <settings /><!-- 设置 -->
    <typeAliases /><!-- 类型命名 -->
    <typeHandlers /><!-- 类型处理器 -->
    <objectFactory /><!-- 对象工厂 -->
    <plugins /><!-- 插件 -->
    <environments><!-- 配置环境 -->
        <environment><!-- 环境变量 -->
            <transactionManager /><!-- 事务管理器 -->
            <dataSource /><!-- 数据源 -->
        </environment>
    </environments>
    <databaseIdProvider /><!-- 数据库厂商标识 -->
    <mappers /><!-- 映射器 -->
</configuration>
```

## 1.properties

- 可以通过 resource 属性指定外部 properties 文件（db.properties）

```xml
<properties resource="db.properties"/>
```

## 2.settings

- 配置 MyBatis 的运行时行为，它能深刻的影响 MyBatis 的底层运行
- 一般不需要大量配置，大部分情况下使用其默认值即可。

## 3.typeAliases

- 定义别名，这样就不用每次都书写类的全限定名称（路径全名）

```xml
<typeAliases>
    <typeAlias alias = "Student" type = "com.kuang.po.Student"/>
</typeAliases>
```

- 对同一个包下的多个类定义别名
  - MyBatis 将扫描 net.biancheng.po 包里面的类，将其第一个字母变为小写作为其别名
  - 如：Student 别名为 student，User 别名为 user

```xml
<typeAliases>
    <package name="com.kuang.po"/>
</typeAliases>
```

## 4.environments（核心）

-  **environments**
  - 可以配置多套运行环境，映射到多个不同的数据库上
  - 需设置默认运行环境，default指定
- **environment**：配置单个运行环境
  - **transactionManager**：设置事务管理器
    - JDBC		应用程序服务器负责事务管理操作，例如提交、回滚等
    - MANAGED        应用程序服务器负责管理连接生命周期
  - **dataSource**：配置连接属性
    - type		指定数据源类型
      - UNPOOLED		没有数据库连接池，效率低下
      - POOLED              创建一个数据库连接池
      - JNDI                      从 JNDI 数据源中获取连接

```xml
<environments default="development">
        <environment id="development">
            <transactionManager type="JDBC" />
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver" />
                <property name="url"
                          value="jdbc:mysql://localhost:3306/mybatis?userSSl=true&amp;useUnicode=true&amp;characterEncoding=utf8" />
                <property name="username" value="root" />
                <property name="password" value="251314fm" />
            </dataSource>
        </environment>
  			<environment id="xxx">
    			...
  			<environment/>
  </environments>
```

## 5.mappers（核心）

- 用于指定 MyBatis SQL 映射文件的路径

```xml
<mappers>
   <mapper resource="com/kuang/dao/UserMapper.xml"/>
</mappers>
```



# Mapper映射器

------

由 Java 接口和 XML 文件（或注解）共同组成

- 定义参数类型
- 配置缓存
- 提供 SQL 语句和动态 SQL
- 定义查询结果和 POJO 的映射关系

**注意：如果 SQL 语句存在动态 SQL 或者比较复杂，使用注解写在 Java 文件里可读性差，且增加了维护的成本。所以一般建议使用 XML 文件配置的方式，避免重复编写 SQL 语句。**



两种实现方式

- XML		mybatis-config.xml 文件中注入mapper.xml 文件

- 注解        使用 Configuration 对象注册 Mapper 接口



## XML映射器

- 定义接口，编写接口的Mapper.xml，在核心配置文件中配置

- 要求

  - 接口的方法名和mapper.xml中sql的id相同

  - 接口方法的输入参数类型和 mapper.xml 中定义的每个 sql 的parameterType 的类型相同

  - 接口方法的输出参数类型和 mapper.xml 中定义的每个 sql 的resultType 的类型相同

  - Mapper.xml 文件中的 namespace 即是 接口的类路径

  

```java
public interface UserDao {
    List<User> getUserList();
}
```

```xml
<!--UserMapper.xml-->
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace 绑定一个对应的Dao/Mapper接口-->
<mapper namespace="com.kuang.dao.UserDao">
    <select id="getUserList" resultType="com.kuang.pojo.User">
        select * from mybatis.user
    </select>
</mapper>
<!--
namespace：用来定义命名空间，该命名空间和定义接口的全限定名（路径名）一致
<select> ：表明这是一条查询语句，id：用来标识，resultType 表示返回值类型
-->
```

```xml
<!--mybatis-config.xml-->
<mappers>
   <mapper resource="com/kuang/dao/UserMapper.xml"/>
</mappers>
```



## 注解映射器（不常用）

- 只需要在接口中使用 Java 注解，注入 SQL 即可
- 只适用简单应用，复杂的sql语句会大大降低代码的可读性
- XML 可以相互引入，而注解不可以
- 同时使用注解和 XML 两种方式定义，那么 XML 将覆盖掉注解
- 复杂场景，使用 XML 方式会更加灵活和方便。因此大部分的企业都以 XML 为主

```java
public interface UserDao {
    @Select(value = "select * from mybatis.user")
    public List<User> getUserList();
}
```

- 这个接口可以在 XML 中定义

```xml
<mapper resource="com/kuang/dao/UserDao" />
```

- 也可以使用 configuration 对象注册这个接口

```xml
configuration.addMapper(UserDao.class);
```



# 执行sql的两种方式

------

- 通过 SqlSession 发送 SQL
- 通过 SqlSession 获取 Mapper 接口，通过 Mapper 接口发送 SQL



## SqlSession 发送 SQL

- **selectOne** 
  - 只返回单个结果集，必须指定查询条件
  - 只能查询 0 或 1 条记录，大于 1 条记录则运行错误

```java
List<User> userList = sqlSession.selectOne("getUserList",1);
```

- **selectList** 
  - 返回一个结果集列表，可以查询 0 或 N 条记录

```java
List<User> userList = sqlSession.selectList("getUserList");
```

也可指定参数：

```java
List<User> userList = sqlSession.selectList("getUserList",10);
```

- **selectOne 实现的 selectList 都可以实现，list 中可以只有一个对象**



## Mapper接口发送 SQL

- 通过 SqlSession 的 getMapper 方法获取一个 Mapper 接口，然后就可以调用它的方法
- 建议采用 Mapper 接口发送 SQL 

```java
UserDao userDao = sqlSession.getMapper(UserDao.class);
List<User> userList = userDao.getUserList();
```



## 区别

- Mapper ：消除 SqlSession 带来的功能性代码，提高可读性

  SqlSession：需要一个 SQL id 去匹配 SQL，比较晦涩难懂

- 使用 Mapper 接口， 则是完全面向对象的语言，更能体现业务的逻辑

- 使用 Mapper，IDE 会提示错误和校验

  而使用 sqlSession语法，只有在运行中才能知道是否会产生错误





# 增删改查 CRUD

------

## select

- 接口中写方法

```java
List<User> getUserList();
User getUserById(int id);
```

- mapper.xml中写对应的sql语句

```xml
<mapper namespace="com.kuang.dao.UserDao">
    <select id="getUserList" resultType="com.kuang.pojo.User">
        select * from mybatis.user
    </select>
    <select id="getUserById" parameterType="int" resultType="com.kuang.pojo.User">
        select * from mybatis.user where id = #{id}
    </select>
</mapper>
```

- 实现类

```java
public void test() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserDao userDao = sqlSession.getMapper(UserDao.class);
 				 List<User> userList = sqlSession.selectList("getUserList");
        for (User user : userList) {
            System.out.println(user);
        }
 			  System.out.println(userList);
        User user=userDao.getUserById(1);
        System.out.println(user);
        sqlSession.close();
    }
```



<font color=red>数据库中的增删改都需要提交事务才能更新数据库（commit）</font>



## insert

- 返回值：插入的行数（1行返回1）

- 接口

```java
int addUser(User user);
```

- mapper.xml中写对应的sql语句

```xml
<insert id="addUser" parameterType="com.kuang.pojo.User">
   insert into mybatis.user(id,name,pwd) values (#{id},#{name},#{pwd})
</insert>
```

- 实现类

```java
public void updateUser(){
     SqlSession sqlSession = MybatisUtils.getSqlSession();
     UserDao userDao = sqlSession.getMapper(UserDao.class);
     userDao.updateUser(new User(4,"呵呵","25424"));
     sqlSession.commit();
}
```



## update

- 返回值：更新的行数（1行返回1）
- 接口

```java
int updateUser(User user);
```

- mapper.xml中写对应的sql语句

```xml
<update id="updateUser" parameterType="com.kuang.pojo.User">
  update mybatis.user set name=#{name},pwd=#{pwd} where id=#{id};
</update>
```

- 实现类

```java
 public void updateUser(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserDao userDao = sqlSession.getMapper(UserDao.class);
        userDao.updateUser(new User(4,"呵呵","25424"));
        sqlSession.commit();
    }
```



## delete

- 返回值：删除的行数（1行返回1）
- 接口

```java
int deleteUser(int id);
```

- mapper.xml中写对应的sql语句

```xml
<delete id="deleteUser" parameterType="com.kuang.pojo.User">
        delete from mybatis.user where id=#{id}
    </delete>
```

- 实现类

```java
public void deleteUser(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserDao userDao = sqlSession.getMapper(UserDao.class);
        userDao.deleteUser(4);
        sqlSession.commit();
        System.out.println("删除成功");
    }
```



## 模糊查询

- $

```xml
<select id="getUserList" resultType="com.kuang.pojo.User">
    select * from mybatis.user where name like '${name}'
</select>
```

- #

```xml
<select id="getUserList" resultType="com.kuang.pojo.User">
    select * from mybatis.user where name like "%"#{name}"%"
</select>
```



## mybatis中的#{}和${}区别(高频)

- \#{} ：参数占位符
  - sql语句参数预编译处理
  - 执行sql时用 ? 号代替 #{} 
  - 使用时不需要关注数据类型、mybatis自动实现数据类型的转换
  - 可以防止sql注入，提高安全性

- ${} ：变量占位符
  - sql语句直接拼接
  - 不做数据类型转换。需要自行判断数据类型
  - 不能防止sql注入
  - 一般用于在使用中间件进行分库分表时，动态指定表名时可能会用到

- **一般情况用\#{}** 



## resultMap

- 主要用于解决实体类属性名与数据库表中字段名不一致的情况
- mapper.xml

```xml
<select id="selectAllWebsite" resultMap="myResult">
    select id,name,url from website
</select>

<!--使用自定义结果集类型 -->
<resultMap type="net.biancheng.po.Website" id="myResult">
    <!-- property 是 net.biancheng.po.Website 类中的属性 -->
    <!--property是实体类的属性名
 				column是数据库的列名，可以来自不同的表 -->
    <id property="id" column="id" />
    <result property="uname" column="name" />
</resultMap>
```

- **resultMap 和 resultType 不能同时使用**



## 注解开发

- 直接在接口方法上使用注解，适合简单的sql语句
  - @Select
  - @Insert
  - @Delete
  - @Update

```java
@Select（" select * from mybatis.user"）
List<User> getUserList();
```

- @Param
  - 基本类型或者String类型的参数，需要加上
  - 引用类型不需要加
  - 通过@Param中设定的属性名来使用

```java
@Select（" select * from mybatis.user"）
User getUserById(@Param（"id"） int id);
```





# 关联查询

