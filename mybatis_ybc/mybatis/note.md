# MyBatis

## 环境搭建

- 创建一个`Maven`的父工程，在`pom.xml`中配置一些公用的依赖。

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.kodeco</groupId>
    <artifactId>atguigu_mybatis_ybc</artifactId>
    <packaging>pom</packaging>
    <modules>
        <module>mybatis</module>
    </modules>
    <version>1.0-SNAPSHOT</version>
    <name>atguigu_mybatis_ybc</name>
    <dependencies>
        <!-- JUnit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.1</version>
            <scope>test</scope>
        </dependency>
        <!-- logback -->
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.5.13</version>
        </dependency>
    </dependencies>
</project>
```

- 创建一个`Maven`的子模块，在`pom.xml`中配置模块需要的依赖。

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>com.kodeco</groupId>
        <artifactId>atguigu_mybatis_ybc</artifactId>
        <version>1.0-SNAPSHOT</version>
    </parent>

    <artifactId>mybatis</artifactId>
    <packaging>jar</packaging>

    <name>mybatis</name>
    <url>http://maven.apache.org</url>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <!-- MyBatis核心 -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.19</version>
        </dependency>

        <!-- MySQL驱动 -->
        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <version>9.3.0</version>
        </dependency>
    </dependencies>
</project>
```

- 在模块中加入日志到配置文件: `src/main/resources/logback.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <!-- 콘솔 출력 설정 -->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>[%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <!-- MyBatis SQL 로그 설정 -->
    <logger name="com.atguigu.mybatis.mapper" level="DEBUG"/>

    <root level="INFO">
        <appender-ref ref="STDOUT"/>
    </root>
</configuration>
```

- 创建`MyBatis`的核心配置文件: `src/main/resources/mybatis-config.xml`
- 习惯上命名为`mybatis-config.xml`，并非强制要求。
- 将来与 Spring 整合后，这个配置文件可以省略。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--
    environments: 配置多个连接数据库的环境
-->
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3310/atguigu_mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    <!-- 引入映射文件 -->
    <mappers>
        <mapper resource="mappers/UserMapper.xml"/>
    </mappers>
</configuration>
```

- 在数据库中创建测试用的表

```sql
DROP DATABASE IF EXISTS `atguigu_mybatis`;
CREATE DATABASE IF NOT EXISTS `atguigu_mybatis` /*!40100 DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci */ /*!80016 DEFAULT ENCRYPTION = 'N' */;
USE `atguigu_mybatis`;

DROP TABLE IF EXISTS t_user;
CREATE TABLE IF NOT EXISTS t_user
(
    id       INT AUTO_INCREMENT
        PRIMARY KEY,
    username VARCHAR(20) NULL,
    password VARCHAR(20) NULL,
    age      INT         NULL,
    sex      CHAR        NULL,
    email    VARCHAR(20) NULL
);

DROP TABLE IF EXISTS t_emp;
CREATE TABLE IF NOT EXISTS t_emp
(
    eid      INT AUTO_INCREMENT
        PRIMARY KEY,
    emp_name VARCHAR(20) NULL,
    age      INT         NULL,
    sex      CHAR        NULL,
    email    VARCHAR(20) NULL,
    did      INT         NULL
);

DROP TABLE IF EXISTS t_dept;
CREATE TABLE IF NOT EXISTS t_dept
(
    did       INT AUTO_INCREMENT
        PRIMARY KEY,
    dept_name VARCHAR(20) NULL
);
```

- 创建实体类: `com.atguigu.mybatis.pojo.User`

```java
public class User {
    private Integer id;
    private String username;
    private String password;
    private Integer age;
    private String sex;
    private String email;

    // 无参构造器
    // 有参构造器
    // getters and setters
    // toString()
}
```

- 创建 Mapper 接口: `com.atguigu.mybatis.mapper.UserMapper`

```java
public interface UserMapper {
    /**
     * 添加用户信息
     */
    int insertUser();
}
```

- 创建 MyBatis 的映射文件: `src/main/resources/mappers/UserMapper.xml`

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.atguigu.mybatis.mapper.UserMapper">

    <!-- int insertUser(); -->
    <insert id="insertUser">
        insert into t_user
        values (null, 'admin', '123456', 23, '男', 'admin@qq.com')
    </insert>
</mapper>
```

- MyBatis 面向接口编程的两个一致:

  1. 映射文件的 namespace 要和 mapper 接口的全类名保持一致。
  2. 映射文件中 SQL 语句的 id 要和 mapper 接口中的方法名一致。

- 表 -- 实体类 -- mapper 接口 -- 映射文件

- 测试

```java
public class MyBatisTest {

    @Test
    public void testMyBatis() throws IOException {
        // 加载核心配置文件
        InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
        // 获取SqlSessionFactoryBuilder
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        // 获取SqlSessionFactory
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(is);
        // 获取SqlSession
        SqlSession sqlSession = sqlSessionFactory.openSession();
        // 设置自动提交: sqlSessionFactory.openSession(true);
        // 获取mapper接口对象
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        // 测试功能
        int result = mapper.insertUser();
        // 提交事务
        sqlSession.commit();
        System.out.println("result: " + result);
    }
}
```

- 注意: 上面的测试方法中必须有提交事务的代码，否则数据无法插入到数据表中。
- 要想设置为`自动提交`，在创建`SqlSession`时，参数传入`true`。

---

> 13 测试修改和删除功能

`src/main/resources/mappers/UserMapper.xml`

```java
/**
 * 修改用户信息
 */
void updateUser();

/**
 * 删除用户信息
 */
void deleteUser();
```

`src/main/resources/mappers/UserMapper.xml`

```xml
<!-- void updateUser(); -->
<update id="updateUser">
    update t_user
    set username = '张三'
    where id = 4
</update>

<!-- void deleteUser(); -->
<delete id="deleteUser">
    delete
    from t_user
    where id = 1
</delete>
```

`src/test/java/com/atguigu/mybatis/test/MyBatisTest.java`

```java
@Test
public void testCRUD() throws IOException {
    InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
    SqlSession sqlSession = sqlSessionFactory.openSession(true);
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    // mapper.updateUser();
    mapper.deleteUser();
}
```

---

> 14 测试查询功能

`src/main/resources/mappers/UserMapper.xml`

```java
/**
 * 根据id查询用户信息
 */
User getUserById();

/**
 * 查询所有的用户信息
 */
List<User> getAllUsers();
```

`src/main/resources/mappers/UserMapper.xml`

```xml
<!-- 在视频里，仅仅这么写是会报错的...，但目前使用的版本不会报错，会自动匹配能匹配的字段 -->
<!-- <select id="getUserById"> -->
<!--
    查询功能的标签需要设置resultType或resultMap。
    resultType: 设置默认的映射关系
    resultMap: 设置的自定义映射关系
-->
<!-- User getUserById(); -->
<select id="getUserById" resultType="com.atguigu.mybatis.pojo.User">
    select *
    from t_user
    where id = 3
</select>

<!-- List<User> getAllUsers(); -->
<select id="getAllUsers" resultType="com.atguigu.mybatis.pojo.User">
    select *
    from t_user
</select>
```

`src/test/java/com/atguigu/mybatis/test/MyBatisTest.java`

```java
@Test
public void testCRUD() throws IOException {
    InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
    SqlSession sqlSession = sqlSessionFactory.openSession(true);
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    // User user = mapper.getUserById();
    // System.out.println(user);
    List<User> list = mapper.getAllUsers();
    list.forEach(System.out::println);
}
```
