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

---

> 15 核心配置文件之 environments

```xml
<!--
    environments: 配置多个连接数据库的环境
    属性:
        default: 设置默认使用的环境的id
-->
<environments default="development">
    <!--
        environment: 配置某个具体的环境
        属性:
        id: 表示连接数据库的环境的唯一标识，不能重复
    -->
    <environment id="development">
        <!--
            transactionManager: 设置事务管理方式
            属性:
                type="JDBC|MANAGED"
                JDBC: 表示当前环境中，执行SQL时，使用的是JDBC中原生的事务管理方式，事务的提交或回滚需要手动处理。
                MANAGED: 被管理，例如Spring
        -->
        <transactionManager type="JDBC"/>
        <!--
            dataSource: 配置数据源
            属性:
                type: 设置数据源的类型
                type="POOLED|UNPOOLED|JNDI"
                POOLED: 表示使用数据库连接池缓存数据库连接。
                UNPOOLED: 表示不使用数据库连接池。
                JNDI: 表示使用上下文中的数据源。
        -->
        <dataSource type="POOLED">
            <!-- 设置连接数据库的驱动 -->
            <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
            <!-- 设置连接数据库的连接地址 -->
            <property name="url" value="jdbc:mysql://localhost:3310/atguigu_mybatis"/>
            <!-- 设置连接数据库的用户名 -->
            <property name="username" value="root"/>
            <!-- 设置连接数据库的密码 -->
            <property name="password" value="123456"/>
        </dataSource>
    </environment>
</environments>
```

- 上面这些配置内容以后都会在`Spring`中进行配置。

---

> 16 核心配置文件之 properties

- 上面数据库的连接信息都是写死的，可以将这些信息放到`properties`文件中来管理。
- 在`IDEA`工程中的`resources`目录中创建`Resource Bundle`文件，默认就是`properties`文件。

`src/main/resources/jdbc.properties`

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3310/atguigu_mybatis
jdbc.username=root
jdbc.password=123456
```

- 在核心配置文件中引用上面配置的数据库连接信息。

```xml
<configuration>
    <!-- 引入properties文件 -->
    <properties resource="jdbc.properties"/>

    <!-- ... -->

    <dataSource type="POOLED">
        <!-- 设置连接数据库的驱动 -->
        <property name="driver" value="${jdbc.driver}"/>
        <!-- 设置连接数据库的连接地址 -->
        <property name="url" value="${jdbc.url}"/>
        <!-- 设置连接数据库的用户名 -->
        <property name="username" value="${jdbc.username}"/>
        <!-- 设置连接数据库的密码 -->
        <property name="password" value="${jdbc.password}"/>
    </dataSource>

    <!-- ... -->
</configuration>
```

---

> 17 核心配置文件之 typeAliases

- 核心配置文件中的标签之间是有顺序的。

```text
    The content of element type "configuration" must match
    "(properties?,settings?,typeAliases?,typeHandlers?,objectFactory?,objectWrapperFactory?,
    reflectorFactory?,plugins?,environments?,databaseIdProvider?,mappers?)".
```

```xml
<!-- 设置类型别名 -->
<typeAliases>
    <!--
        typeAlias: 设置某个类型的别名。
        属性:
            type: 设置需要设置别名的类型的全类名。
            alias: 设置别名。如果不设置alias的值，那么其默认值是类的简单名称。
                    别名是不区分大小写的，即在配置中设置了alias="User"后，在映射文件中写resultType="user"也是没问题的。
    -->
    <!-- 这种方式用的很少，因为只能一次设置一个类的别名。而是会使用下面的package标签来批量地设置别名。
    <typeAlias type="com.atguigu.mybatis.pojo.User"/>
    -->
    <!-- 以包为单位，将包下所有的类型设置默认的类型别名，即类的简单名称，且不区分大小写。 -->
    <package name="com.atguigu.mybatis.pojo"/>
</typeAliases>
```

- 设置别名后，在映射文件中就可以使用类的`别名`来指定`resultType`了。

```xml
<!-- User getUserById(); -->
<select id="getUserById" resultType="user">
    select *
    from t_user
    where id = 3
</select>

<!-- List<User> getAllUsers(); -->
<select id="getAllUsers" resultType="User">
    select *
    from t_user
</select>
```

---

> 18 核心配置文件之 mappers

- 映射文件也可以以包为单位进行引入，但是需要满足一些要求。

```xml
<!-- 引入映射文件 -->
<mappers>
    <!-- <mapper resource="mappers/UserMapper.xml"/> -->
    <!--
        以包为单位引入映射文件
        要求:
            1. mapper接口所在的包要和映射文件所在的包一致。
            2. mapper接口要和映射文件的名字一致。
    -->
    <package name="com.atguigu.mybatis.mapper"/>
</mappers>
```

![mapper package](./images/18_01_mappers_package.png)

---

> 19 思考: 映射文件中的 SQL 该如何拼接

- 到目前为止，写到 SQL 语句都是写死到，以后需要学习如何动态获得从客户端传来到参数，并动态地拼接到 SQL 映射文件中。

---

> 20 在`IDEA`中配置核心配置文件到模板

- 首先准备核心配置文件的内容

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties resource="jdbc.properties"/>

    <typeAliases>
        <package name=""/>
    </typeAliases>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <package name=""/>
    </mappers>
</configuration>
```

- 在`IDEA`中按下 2 次`Shift`后，搜索`File and Code Templates`

![mybatis config template](./images/20_01_mybatis_config_template.png)

---

> 21 在`IDEA`中配置映射文件到模板

- 首先准备映射文件的内容

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="">

</mapper>
```

- 在`IDEA`中按下 2 次`Shift`后，搜索`File and Code Templates`

![mybatis mapper template](./images/21_01_mybatis_mapper_template.png)

---

> 22 封装`SqlSessionUtils`工具类并测试功能

`src/main/java/com/atguigu/mybatis/utils/SqlSessionUtils.java`

```java
public class SqlSessionUtils {
    public static SqlSession getSqlSession() {
        SqlSession sqlSession = null;
        try {
            InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
            sqlSession = sqlSessionFactory.openSession(true);
        } catch (IOException e) {
            e.printStackTrace();
        }
        return sqlSession;
    }
}
```

为学习获取参数值做一些准备:

`src/main/java/com/atguigu/mybatis/mapper/ParameterMapper.java`

```java
public interface ParameterMapper {

    /**
     * 查询所有的员工信息
     */
    List<User> getAllUsers();
}
```

`src/main/resources/com/atguigu/mybatis/mapper/ParameterMapper.xml`

```java
<mapper namespace="com.atguigu.mybatis.mapper.ParameterMapper">

    <!-- List<User> getAllUsers(); -->
    <select id="getAllUsers" resultType="User">
        select *
        from t_user
    </select>
</mapper>
```

---

> 23 MyBatis 获取参数值的两种方式`#{}`和`${}`

- MyBatis 获取参数值的两种方式: ${}和#{}

  - ${}的本质是字符串拼接
  - #{}的本质是占位符赋值

- JDBC 中相应的使用方式

```java
public void testJDBC() throws Exception {
    String username = "Tom";
    Class.forName("");
    Connection conn = DriverManager.getConnection("", "", "");
    // 字符串拼接的方式
    // Statement stmt = conn.createStatement();
    // stmt.execute("select * from t_user where username = '" + username + "'");
    // 使用占位符的方式
    PreparedStatement ps = conn.prepareStatement("select * from t_user where username = ?");
    ps.setString(1, "Tom");
}
```

---

> 24 MyBatis 获取参数值的各种情况(1)

- Mapper 接口方法的参数为单个的字面量类型
- 可以通过`${}`和`#{}`以`任意的名称`获取参数值，但是为了见名知意，建议使用和参数名一致的名称。

`src/main/java/com/atguigu/mybatis/mapper/ParameterMapper.java`

```java
/*
根据用户名查询用户信息
*/
User getUserByUsername(String username);
```

`src/main/resources/com/atguigu/mybatis/mapper/ParameterMapper.xml`

```xml
<!-- User getUserByUsername(String username); -->
<select id="getUserByUsername" resultType="User">
    select *
    from t_user
    where username = #{username}
</select>
```

`src/test/java/com/atguigu/mybatis/test/B_ParameterMapperTest.java`

```java
@Test
public void testGetUserByUsername() {
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    ParameterMapper mapper = sqlSession.getMapper(ParameterMapper.class);
    User admin = mapper.getUserByUsername("admin");
    System.out.println(admin);
}
```

---

> 25 MyBatis 获取参数值的各种情况(2)

- Mapper 接口方法的参数为多个时
- 此时 MyBatis 会将这些参数放在一个`map`集合中，以两种方式进行存储

  - 以`arg0`, `arg1`...为键，以参数值为值
  - 以`param1`, `param2`...为键，以参数值为值

- 如果没有按照 MyBatis 提供的键在映射文件中进行参数的获取，则会报一下的异常:

```text
Error querying database.  Cause: org.apache.ibatis.binding.BindingException: Parameter 'username' not found. Available parameters are [arg1, arg0, param1, param2]
```

```java
 /*
验证登录
*/
User checkLogin(String username, String password);
```

```xml
<!-- User checkLogin(String username, String password); -->
<select id="checkLogin" resultType="User">
    select *
    from t_user
    where username = #{param1}
        and password = #{param2}
</select>
```

```java
@Test
public void testCheckLogin() {
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    ParameterMapper mapper = sqlSession.getMapper(ParameterMapper.class);
    User user = mapper.checkLogin("admin", "123456");
    System.out.println(user);
}
```

---

> 26 MyBatis 获取参数值的各种情况(3)

- 若 Mapper 接口方法的参数有多个时，可以手动将这些参数放在一个`Map`集合中
- 然后在 SQL 映射文件中就可以使用在`Map`集合中设置的`Key`来获取参数的值

```java
/*
验证登录(参数为Map)
*/
User checkLoginByMap(Map<String, Object> map);
```

```xml
<!-- User checkLoginByMap(Map<String, Object> map); -->
<select id="checkLoginByMap" resultType="User">
    select *
    from t_user
    where username = #{username}
        and password = #{password}
</select>
```

```java
@Test
public void testCheckLoginByMap() {
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    ParameterMapper mapper = sqlSession.getMapper(ParameterMapper.class);
    Map<String, Object> map = new HashMap<>();
    map.put("username", "admin");
    map.put("password", "123456");
    User user = mapper.checkLoginByMap(map);
    System.out.println(user);
}
```

---

> 27 MyBatis 获取参数值的各种情况(4)

- Mapper 接口方法的参数为实体类类型时
- 然后在 SQL 映射文件中通过实体类的`属性`来获取参数值

```java
/*
添加用户信息
*/
int insertUser(User user);
```

```xml
<!-- int insertUser(User user); -->
<insert id="insertUser">
    insert into t_user
    values (null, #{username}, #{password}, #{age}, #{sex}, #{email})
</insert>
```

```java
@Test
public void testInsertUser() {
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    ParameterMapper mapper = sqlSession.getMapper(ParameterMapper.class);
    User user = new User(null, "Tom", "111222", 18, "女", "tom@gmail.com");
    int result = mapper.insertUser(user);
    System.out.println("result: " + result);
}
```

---

> 28 MyBatis 获取参数值的各种情况(5)

- 使用命名参数`@Param("在映射文件中使用的Key")`
- 此时 MyBatis 会将值参数放在一个 Map 集合中，以两种方式进行存储
  - 以@Param 注解的值为键，以参数为值
  - 以 param1, param2..为键，以参数为值(此时 arg0, arg1...不可用)

```java
/*
验证登录(使用@Param)
*/
User checkLoginByParam(@Param("username") String username, @Param("password") String password);
```

```xml
<!-- User checkLoginByParam(@Param("username") String username, @Param("password") String password); -->
<select id="checkLoginByParam" resultType="User">
    select *
    from t_user
    where username = #{param1}
        and password = #{param2}
</select>
```

```java
@Test
public void testCheckLoginByParam() {
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    ParameterMapper mapper = sqlSession.getMapper(ParameterMapper.class);
    User user = mapper.checkLoginByParam("admin", "123456");
    System.out.println(user);
}
```

---

> 29 `@Param`源码分析

```java
public ParamNameResolver(Configuration config, Method method) {
    this.useActualParamName = config.isUseActualParamName();
    final Class<?>[] paramTypes = method.getParameterTypes();
    final Annotation[][] paramAnnotations = method.getParameterAnnotations();
    final SortedMap<Integer, String> map = new TreeMap<>();
    int paramCount = paramAnnotations.length;
    // get names from @Param annotations
    for (int paramIndex = 0; paramIndex < paramCount; paramIndex++) {
      if (isSpecialParameter(paramTypes[paramIndex])) {
        // skip special parameters
        continue;
      }
      String name = null;
      for (Annotation annotation : paramAnnotations[paramIndex]) {
        if (annotation instanceof Param) {
          hasParamAnnotation = true;
          name = ((Param) annotation).value();
          break;
        }
      }
      if (name == null) {
        // @Param was not specified.
        if (useActualParamName) {
          name = getActualParamName(method, paramIndex);
        }
        if (name == null) {
          // use the parameter index as the name ("0", "1", ...)
          // gcode issue #71
          name = String.valueOf(map.size());
        }
      }
      map.put(paramIndex, name);
    }
    names = Collections.unmodifiableSortedMap(map);
  }

public Object getNamedParams(Object[] args) {
    final int paramCount = names.size();
    if (args == null || paramCount == 0) {
      return null;
    }
    if (!hasParamAnnotation && paramCount == 1) {
      Object value = args[names.firstKey()];
      return wrapToMapIfCollection(value, useActualParamName ? names.get(names.firstKey()) : null);
    } else {
      final Map<String, Object> param = new ParamMap<>();
      int i = 0;
      for (Map.Entry<Integer, String> entry : names.entrySet()) {
        param.put(entry.getValue(), args[entry.getKey()]);
        // add generic param names (param1, param2, ...)
        final String genericParamName = i < 10 ? GENERIC_NAME_CACHE[i] : GENERIC_NAME_PREFIX + (i + 1);
        // ensure not to overwrite parameter named with @Param
        if (!names.containsValue(genericParamName)) {
          param.put(genericParamName, args[entry.getKey()]);
        }
        i++;
      }
      return param;
    }
  }
```

---

> 30 MyBatis 的各种查询功能(1)

- MyBatis 的各种查询功能
  1. 若查询的数据只有一条  
     a> 可以通过实体类对象接收  
     b> 可以通过 List 集合接收
  2. 若查询的数据多条  
     a> 可以通过 List 集合接收  
     b>  
     注意: 一定不能通过实体类对象接收，否则会报如下异常:  
     `org.apache.ibatis.exceptions.TooManyResultsException: Expected one result (or null) to be returned by selectOne(), but found: 5`

`src/main/java/com/atguigu/mybatis/mapper/SelectMapper.java`

```java
public interface SelectMapper {
    /**
     * 根据id查询用户信息
     */
    User getUserById(@Param("id") Integer id);
    // 这种情况也可以用List集合接收
    // List<User> getUserById(@Param("id") Integer id);

    /**
     * 查询所有的用户信息
     */
    List<User> getAllUsers();
}
```

`src/main/resources/com/atguigu/mybatis/mapper/SelectMapper.xml`

```xml
<mapper namespace="com.atguigu.mybatis.mapper.SelectMapper">
    <!-- User getUserById(@Param("id") Integer id); -->
    <select id="getUserById" resultType="User">
        select *
        from t_user
        where id = #{id}
    </select>

    <!-- List<User> getAllUsers(); -->
    <select id="getAllUsers" resultType="User">
        select *
        from t_user
    </select>
</mapper>
```

`src/test/java/com/atguigu/mybatis/test/C_SelectMapperTest.java`

```java
public class C_SelectMapperTest {

    @Test
    public void testGetUserById() {
        SqlSession sqlSession = SqlSessionUtils.getSqlSession();
        SelectMapper mapper = sqlSession.getMapper(SelectMapper.class);
        System.out.println(mapper.getUserById(3));
        // 结果: User{id=3, username='admin1', password='123456', age=23, sex='男', email='admin@qq.com'}
    }

    @Test
    public void testGetAllUsers() {
        SqlSession sqlSession = SqlSessionUtils.getSqlSession();
        SelectMapper mapper = sqlSession.getMapper(SelectMapper.class);
        List<User> list = mapper.getAllUsers();
        list.forEach(System.out::println);
        // 结果:
        /*
        User{id=2, username='admin', password='123456', age=23, sex='男', email='admin@qq.com'}
        User{id=3, username='admin1', password='123456', age=23, sex='男', email='admin@qq.com'}
        User{id=4, username='张三', password='123456', age=23, sex='男', email='admin@qq.com'}
        ...
         */
    }
}
```

---

> 31 MyBatis 的各种查询功能(2)

- MyBatis 中设置了默认的类型`别名`

  - `java.lang.Integer` -> `int`, `integer`
  - `int` -> `_int`, `-integer`
  - `Map` -> `map`
  - `String` -> `string`

- 查询`单行单列`的值

`src/main/java/com/atguigu/mybatis/mapper/SelectMapper.java`

```java
/**
 * 查询用户信息的总记录数
 */
Integer getUserCount();
```

`src/main/resources/com/atguigu/mybatis/mapper/SelectMapper.xml`

```xml
<!-- Integer getUserCount(); -->
<select id="getUserCount" resultType="_int">
    select count(*)
    from t_user
</select>
```

`src/test/java/com/atguigu/mybatis/test/C_SelectMapperTest.java`

```java
@Test
public void testGetUserCount() {
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    SelectMapper mapper = sqlSession.getMapper(SelectMapper.class);
    Integer userCount = mapper.getUserCount();
    System.out.println("userCount: " + userCount);
    // 结果: userCount: 5
}
```

---

> 32 MyBatis 的各种查询功能(3)

- 将查询出的一条记录使用`Map`集合来接收

  1. 若查询的数据只有一条  
     a> 可以通过实体类对象接收  
     b> 可以通过 List 集合接收
     c> 可以通过 Map 集合接收
  2. 若查询的数据多条  
     a> 可以通过 List 集合接收  
     b>

`src/main/java/com/atguigu/mybatis/mapper/SelectMapper.java`

```java
/**
 * 根据id查询用户信息为一个Map集合
 */
Map<String, Object> getUserByIdToMap(@Param("id") Integer id);
```

`src/main/resources/com/atguigu/mybatis/mapper/SelectMapper.xml`

```xml
<!-- Map<String, Object> getUserByIdToMap(@Param("id") Integer id); -->
<select id="getUserByIdToMap" resultType="map">
    select *
    from t_user
    where id = #{id}
</select>
```

`src/test/java/com/atguigu/mybatis/test/C_SelectMapperTest.java`

```java
@Test
public void testGetUserByIdToMap() {
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    SelectMapper mapper = sqlSession.getMapper(SelectMapper.class);
    System.out.println(mapper.getUserByIdToMap(3));
    // 结果: {password=123456, sex=男, id=3, age=23, email=admin@qq.com, username=admin1}
}
```

---

> 33 MyBatis 的各种查询功能(4)

- 将查询出的多条记录使用`Map`集合来接收

  1. 若查询的数据只有一条  
     a> 可以通过实体类对象接收  
     b> 可以通过 List 集合接收  
     c> 可以通过 Map 集合接收
  2. 若查询的数据多条  
     a> 可以通过 List 集合接收  
     b> 可以通过 Map 类型的 List 集合接收  
     c> 可以在 Mapper 接口的方法上添加`@MapKey`注解，此时就以注解中指定的字段作为键，以查询出的每条记录作为值，放在同一个`Map`集合中

`src/main/java/com/atguigu/mybatis/mapper/SelectMapper.java`

```java
/**
 * 查询所有的用户信息为Map集合
 */
@MapKey("id")
Map<String, Object> getAllUsersToMap();
```

`src/main/resources/com/atguigu/mybatis/mapper/SelectMapper.xml`

```xml
<!-- Map<String, Object> getAllUsersToMap(); -->
<select id="getAllUsersToMap" resultType="map">
    select *
    from t_user
</select>
```

`src/test/java/com/atguigu/mybatis/test/C_SelectMapperTest.java`

```java
@Test
public void testGetAllUsersToMap() {
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    SelectMapper mapper = sqlSession.getMapper(SelectMapper.class);
    System.out.println(mapper.getAllUsersToMap());
    // 结果: {2={password=123456, sex=男, id=2, age=23, email=admin@qq.com, username=admin},
    // 3={password=123456, sex=男, id=3, age=23, email=admin@qq.com, username=admin1},
    // 4={password=123456, sex=男, id=4, age=23, email=admin@qq.com, username=张三},
    // 5={password=123456, sex=男, id=5, age=23, email=admin@qq.com, username=2},
    // 6={password=111222, sex=女, id=6, age=18, email=tom@gmail.com, username=Tom}}
}
```

---

> 34 MyBatis 处理模糊查询

- 根据用户名模糊查询用户信息
- 可以尝试下面几种写法:

1. 使用`#{username}`，直接这么写会报下面的异常:

```text
Caused by: org.apache.ibatis.type.TypeException: Could not set parameters for mapping:
ParameterMapping{property='username', mode=IN, javaType=class java.lang.Object, jdbcType=null, numericScale=null,
resultMapId='null', jdbcTypeName='null', expression='null'}.
Cause: org.apache.ibatis.type.TypeException: Error setting non null for parameter #1 with JdbcType null .
Try setting a different JdbcType for this parameter or a different configuration property.
Cause: java.sql.SQLException: Parameter index out of range (1 > number of parameters, which is 0).
```

2. 使用`${username}`，这种方式看似可以用，但是会有`SQL注入`的问题。
3. 使用`concat()`函数。
4. 使用`"%" #{username} "%"`。

`src/main/java/com/atguigu/mybatis/mapper/SQLMapper.java`

```java
public interface SQLMapper {

    /**
     * 根据用户名模糊查询用户信息
     */
    List<User> getUserByNameLike(@Param("username") String username);
}
```

`src/main/resources/com/atguigu/mybatis/mapper/SQLMapper.xml`

```xml
<mapper namespace="com.atguigu.mybatis.mapper.SQLMapper">

    <!-- List<User> getUserByNameLike(@Param("username") String username); -->
    <select id="getUserByNameLike" resultType="User">
        select *
        from t_user
        where username like "%" #{username} "%"
    </select>
    <!--
        where username like '%#{username}%'
        where username like '%${username}%'
        where username like concat('%', #{username}, '%')
    -->
</mapper>
```

`src/test/java/com/atguigu/mybatis/test/D_SQLMapperTest.java`

```java
public class D_SQLMapperTest {
    @Test
    public void testGetUserByLike() {
        SqlSession sqlSession = SqlSessionUtils.getSqlSession();
        SQLMapper mapper = sqlSession.getMapper(SQLMapper.class);
        // 下面是可以导致SQL注入的写法
        // List<User> list = mapper.getUserByNameLike("' or true or '");
        List<User> list = mapper.getUserByNameLike("a");
        list.forEach(System.out::println);
    }
}
```

---

> 35 MyBatis 处理批量删除

- 本示例想要使用`delete from t_user where id in (1,2,3)`的方式批量删除，使用`#{}`的方式会直接报错。
- 因为对于 MySQL 数据库，此 SQL 语句为`delete from t_user where id in ('1,2,3')`，执行时是会报错的。`ERROR 1292 (22007): Truncated incorrect DOUBLE value: '1, 2, 3'`
- 这是 MySQL 数据库出于安全考虑，不让这个 SQL 语句执行，如果想要让它能执行，虽然不推荐，但是可以通过配置修改`sql_mode`的值，让其执行。
- 但是即使执行，也是会抛出警告。

```bash
# 查看sql_mode
SELECT @@SESSION.sql_mode;
SELECT @@GLOBAL.sql_mode;

# ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION

# 只对当前连接生效
SET SESSION sql_mode =
REPLACE(@@SESSION.sql_mode, 'STRICT_TRANS_TABLES', '');

# 对所有新的连接生效
SET GLOBAL sql_mode =
REPLACE(@@GLOBAL.sql_mode, 'STRICT_TRANS_TABLES', '');

# 永久修改: my.cnf
[mysqld]
sql_mode=ONLY_FULL_GROUP_BY,NO_ENGINE_SUBSTITUTION
```

- 修改了`sql_mode`的值后，现退出当前连接，再次连接后，就会看到下面的效果:

```text
mysql> delete from t_user where id in ('1, 2, 3');
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> show warnings;
+---------+------+---------------------------------------------+
| Level   | Code | Message                                     |
+---------+------+---------------------------------------------+
| Warning | 1292 | Truncated incorrect DOUBLE value: '1, 2, 3' |
+---------+------+---------------------------------------------+
1 row in set (0.00 sec)
```

- 但是上面的命令仅仅是在 MySQL 内部的命令行是可以执行的，在其他的客户端，如`DataGrip`和`IDEA`，因为安全级别的设定，还是会报错。

- 所以在 MyBatis 中实现批量删除时:
  1. 可以使用`${}`来实现，但是不推荐，因为有`SQL注入`的问题
  2. 使用`<foreach>`循环结构，搭配`#{}`，这才是比较靠谱的方式

```java
/**
 * 批量删除
 */
// int deleteBatch(@Param("ids") String ids);
int deleteBatch(@Param("ids") String[] ids);
```

```xml
<!-- int deleteBatch(@Param("ids") String ids); -->
<delete id="deleteBatch">
    delete
    from t_user
    where id in
    <foreach collection="ids" item="id" separator="," open="(" close=")">
        #{id}
    </foreach>
</delete>
<!-- where id in (${ids}) -->
```

```java
@Test
public void testDeleteBatch() {
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    SQLMapper mapper = sqlSession.getMapper(SQLMapper.class);
    String ids = "1,2,3,4,5,6";
    // int i = mapper.deleteBatch(ids);
    String[] idArr = ids.split(",");
    int i = mapper.deleteBatch(idArr);

    System.out.println("i: " + i);
}
```

---

> 36 MyBatis 处理动态设置表明

- 对于一些特殊的情况，只能用`${}`来动态地设置表明，因为查询中的表名是不能用引号引起来的，所以不能使用`#{}`。

```java
/**
 * 查询指定表中的数据
 */
List<User> selectUserByTableName(@Param("tableName") String tableName);
```

```xml
<!-- List<User> selectUserByTableName(@Param("tableName") String tableName); -->
<select id="selectUserByTableName" resultType="User">
    select *
    from ${tableName}
</select>
```

```java
@Test
public void testGetUserByTableName() {
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    SQLMapper mapper = sqlSession.getMapper(SQLMapper.class);
    List<User> list = mapper.selectUserByTableName("t_user");
    list.forEach(System.out::println);
}
```

---

> 37 获取添加功能自增的主键

- 在原始的`JDBC`中，获取自增 ID 的方法如下:

```java
public class AutoGeneratedTest {
    @Test
    public void testJDBCGetAutoGeneratedKey() throws Exception {
        Connection conn = JDBCUtils.getConnection();
        String sql = "insert into customers (name, email, birth) values (?, ?, ?)";
        // 想要获取自动递增的id必须指定第2个参数为PreparedStatement.RETURN_GENERATED_KEYS
        // 否则会报下面的异常
        // java.sql.SQLException: Generated keys not requested.
        // You need to specify Statement.RETURN_GENERATED_KEYS to Statement.executeUpdate(),
        // Statement.executeLargeUpdate() or Connection.prepareStatement().
        PreparedStatement ps = conn.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS);
        ps.setObject(1, "xixi");
        ps.setObject(2, "xixi@126.com");
        ps.setObject(3, new Date(432213982123L));

        int i = ps.executeUpdate();
        System.out.println(i);

        ResultSet rs = ps.getGeneratedKeys();
        if (rs.next()) {
            Object id = rs.getObject(1);
            System.out.println("id: " + id);
        }
        JDBCUtils.closeResources(conn, ps);
    }
}
```

- 在 MyBatis 中获取自增的方式如下:

```java
/**
 * 添加用户信息
 */
void insertUser(User user);
```

```xml
<!-- void insertUser(User user); -->
<insert id="insertUser" useGeneratedKeys="true" keyProperty="id">
    insert into t_user
    values (null, #{username}, #{password}, #{age}, #{sex}, #{email})
</insert>
```

```java
@Test
public void testInsertUser() {
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    SQLMapper mapper = sqlSession.getMapper(SQLMapper.class);
    User user = new User(null, "Longbottom", "123456", 18, "男", "long@123.com");
    mapper.insertUser(user);
    System.out.println(user);
    // 结果: User{id=6, username='Longbottom', password='123456', age=18, sex='男', email='long@123.com'}
}
```

- `useGeneratedKeys`: 设置当前标签中的 SQL 使用了自增的主键
- `keyProperty`: 将自增主键的值赋给传输到映射文件中参数到某个属性
