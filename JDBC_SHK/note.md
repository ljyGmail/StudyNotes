# JDBC

- 使用 Docker 创建`mysql`容器

```bash
docker volume create mysql-volume

# 需要注意下面的配置文件目录的挂载配置需要根据不同的电脑设置合适的路径
docker run -d -p3310:3306  -v mysql-volume:/var/lib/mysql -v /Users/liangjinyong/Desktop/Playground/test:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:8.0
```

- 连接`mysql`数据库后，导入数据

```sql
/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET NAMES utf8 */;
/*!50503 SET NAMES utf8mb4 */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
/*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;


-- 导出 jdbc_learn 的数据库结构
DROP DATABASE IF EXISTS `jdbc_learn`;
CREATE DATABASE IF NOT EXISTS `jdbc_learn` /*!40100 DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci */ /*!80016 DEFAULT ENCRYPTION='N' */;
USE `jdbc_learn`;

-- 导出  表 jdbc_learn.customers 结构
DROP TABLE IF EXISTS `customers`;
CREATE TABLE IF NOT EXISTS `customers` (
  `id` int NOT NULL AUTO_INCREMENT,
  `name` varchar(15) DEFAULT NULL,
  `email` varchar(20) DEFAULT NULL,
  `birth` date DEFAULT NULL,
  `photo` mediumblob,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=19 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- 正在导出表  jdbc_learn.customers 的数据：~0 rows (大约)
/*!40000 ALTER TABLE `customers` DISABLE KEYS */;
INSERT INTO `customers` (`id`, `name`, `email`, `birth`, `photo`) VALUES
	(1, '汪峰', 'wf@126.com', '2010-02-02', NULL),
	(2, '王菲', 'wangf@163.com', '1988-12-26', NULL),
	(3, '林志玲', 'linzl@gmail.com', '1984-06-12', NULL),
	(4, '汤唯', 'tangw@sina.com', '1986-06-13', NULL),
	(5, '成龙', 'Jackey@gmai.com', '1955-07-14', NULL),
	(6, '迪丽热巴', 'reba@163.com', '1983-05-17', NULL),
	(7, '刘亦菲', 'liuyifei@qq.com', '1991-11-14', NULL),
	(8, '陈道明', 'bdf@126.com', '2014-01-17', NULL),
	(10, '周杰伦', 'zhoujl@sina.com', '1979-11-15', NULL),
	(12, '黎明', 'LiM@126.com', '1998-09-08', NULL),
	(13, '张学友', 'zhangxy@126.com', '1998-12-21', NULL),
	(16, '朱茵', 'zhuyin@126.com', '2014-01-16', NULL),
	(18, '贝多芬', 'beidf@126.com', '2014-01-17', NULL);
/*!40000 ALTER TABLE `customers` ENABLE KEYS */;

-- 导出  表 jdbc_learn.examstudent 结构
DROP TABLE IF EXISTS `examstudent`;
CREATE TABLE IF NOT EXISTS `examstudent` (
  `FlowID` int NOT NULL AUTO_INCREMENT,
  `Type` int DEFAULT NULL,
  `IDCard` varchar(18) DEFAULT NULL,
  `ExamCard` varchar(15) DEFAULT NULL,
  `StudentName` varchar(20) DEFAULT NULL,
  `Location` varchar(20) DEFAULT NULL,
  `Grade` int DEFAULT NULL,
  PRIMARY KEY (`FlowID`)
) ENGINE=InnoDB AUTO_INCREMENT=7 DEFAULT CHARSET=gb2312;

-- 正在导出表  jdbc_learn.examstudent 的数据：~0 rows (大约)
/*!40000 ALTER TABLE `examstudent` DISABLE KEYS */;
INSERT INTO `examstudent` (`FlowID`, `Type`, `IDCard`, `ExamCard`, `StudentName`, `Location`, `Grade`) VALUES
	(1, 4, '412824195263214584', '200523164754000', '张锋', '郑州', 85),
	(2, 4, '222224195263214584', '200523164754001', '孙朋', '大连', 56),
	(3, 6, '342824195263214584', '200523164754002', '刘明', '沈阳', 72),
	(4, 6, '100824195263214584', '200523164754003', '赵虎', '哈尔滨', 95),
	(5, 4, '454524195263214584', '200523164754004', '杨丽', '北京', 64),
	(6, 4, '854524195263214584', '200523164754005', '王小红', '太原', 60);
/*!40000 ALTER TABLE `examstudent` ENABLE KEYS */;

-- 导出  表 jdbc_learn.order 结构
DROP TABLE IF EXISTS `order`;
CREATE TABLE IF NOT EXISTS `order` (
  `order_id` int NOT NULL AUTO_INCREMENT,
  `order_name` varchar(20) DEFAULT NULL,
  `order_date` date DEFAULT NULL,
  PRIMARY KEY (`order_id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- 正在导出表  jdbc_learn.order 的数据：~0 rows (大约)
/*!40000 ALTER TABLE `order` DISABLE KEYS */;
INSERT INTO `order` (`order_id`, `order_name`, `order_date`) VALUES
	(1, 'AA', '2010-03-04'),
	(2, 'BB', '2000-02-01'),
	(4, 'GG', '1994-06-28');
/*!40000 ALTER TABLE `order` ENABLE KEYS */;

-- 导出  表 jdbc_learn.user 结构
DROP TABLE IF EXISTS `user`;
CREATE TABLE IF NOT EXISTS `user` (
  `id` int NOT NULL AUTO_INCREMENT,
  `name` varchar(10) NOT NULL,
  `password` varchar(15) NOT NULL DEFAULT '123456',
  `address` varchar(25) DEFAULT NULL,
  `phone` varchar(15) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- 正在导出表  jdbc_learn.user 的数据：~5 rows (大约)
/*!40000 ALTER TABLE `user` DISABLE KEYS */;
INSERT INTO `user` (`id`, `name`, `password`, `address`, `phone`) VALUES
	(1, '章子怡', 'qwerty', 'Beijing', '13788658672'),
	(2, '郭富城', 'abc123', 'HongKong', '15678909898'),
	(3, '林志玲', '654321', 'Taiwan', '18612124565'),
	(4, '梁静茹', '987654367', 'malaixiya', '18912340998'),
	(5, 'LadyGaGa', '123456', 'America', '13012386565');
/*!40000 ALTER TABLE `user` ENABLE KEYS */;

-- 导出  表 jdbc_learn.user_table 结构
DROP TABLE IF EXISTS `user_table`;
CREATE TABLE IF NOT EXISTS `user_table` (
  `user` varchar(20) DEFAULT NULL,
  `password` varchar(20) DEFAULT NULL,
  `balance` int DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- 正在导出表  jdbc_learn.user_table 的数据：~4 rows (大约)
/*!40000 ALTER TABLE `user_table` DISABLE KEYS */;
INSERT INTO `user_table` (`user`, `password`, `balance`) VALUES
	('AA', '123456', 1000),
	('BB', '654321', 1000),
	('CC', 'abcd', 2000),
	('DD', 'abcder', 3000);
/*!40000 ALTER TABLE `user_table` ENABLE KEYS */;

/*!40101 SET SQL_MODE=IFNULL(@OLD_SQL_MODE, '') */;
/*!40014 SET FOREIGN_KEY_CHECKS=IF(@OLD_FOREIGN_KEY_CHECKS IS NULL, 1, @OLD_FOREIGN_KEY_CHECKS) */;
/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40111 SET SQL_NOTES=@OLD_SQL_NOTES */;

CREATE TABLE IF NOT EXISTS goods
(
    id   INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(25)
);
```

![alt text](./images/07_01_idea_project.png)

- `pom.xml`

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.atguigu</groupId>
    <artifactId>jdbc_shk</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <version>9.1.0</version>
        </dependency>

        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>5.12.2</version>
            <scope>compile</scope>
        </dependency>
    </dependencies>
</project>

```

## 第 2 章 获取数据库的连接

- 测试类
- 方式一

```java
public class ConnectionTest {

  @Test
  public void testConnection1() throws SQLException {
      // 获取Driver实现类对象
      Driver driver = new com.mysql.cj.jdbc.Driver();
      // jdbc:mysql: 协议
      // localhost: IP地址
      // 3310: 数据库端口号
      // jdbc_learn: 数据库名称
      String url = "jdbc:mysql://localhost:3310/jdbc_learn";
      // 将用户名和密码封装在Properties中
      Properties info = new Properties();
      info.setProperty("user", "root");
      info.setProperty("password", "123456");

      Connection conn = driver.connect(url, info);
      System.out.println("方式一: " + conn);
  }
}
```

- 方式二: 对方式一的迭代
- 为了让上面的程序能有更好的移植性，不能在程序里看到直接使用第三方类的代码(com.mysql.cj.jdbc.Driver)。

```java
  @Test
  public void testConnection2() throws Exception {
      // 1. 获取Driver实现类对象
      Class clazz = Class.forName("com.mysql.cj.jdbc.Driver");
      Driver driver = (Driver) clazz.newInstance();

      // 2. 提供要连接的数据库
      String url = "jdbc:mysql://localhost:3310/jdbc_learn";

      // 3. 提供连接需要的用户名和密码
      Properties info = new Properties();
      info.setProperty("user", "root");
      info.setProperty("password", "123456");

      // 4. 获取连接
      Connection conn = driver.connect(url, info);
      System.out.println("方式二: " + conn);
  }
```

- 方式三: 使用 DriverManager 替代 Driver

```java
  @Test
  public void testConnection3() throws Exception {
      // 1. 获取Driver实现类对象
      Class clazz = Class.forName("com.mysql.cj.jdbc.Driver");
      Driver driver = (Driver) clazz.newInstance();

      // 2. 提供另外三个连接的基本信息
      String url = "jdbc:mysql://localhost:3310/jdbc_learn";
      String user = "root";
      String password = "123456";

      // 注册驱动
      DriverManager.registerDriver(driver);

      // 获取连接
      Connection conn = DriverManager.getConnection(url, user, password);
      System.out.println("方式三: " + conn);
  }
```

- 方式四: 使用 DriverManager 替代 Driver
- 可以只是加载驱动，不用显式地注册驱动了。

```java
  @Test
  public void testConnection4() throws Exception {
      // 1. 提供三个连接的基本信息
      String url = "jdbc:mysql://localhost:3310/jdbc_learn";
      String user = "root";
      String password = "123456";

      // 2. 获取Driver实现类对象 --> 该步骤可以优化 -> 加载Driver
      // 对于MySQL数据库，甚至下面这一行也可以省略 -> mysql-connector-j-9.1.0.jar > META-INF > services > java.sql.Driver
      // 但是建议不要省略，因为如果换成其他数据库，省略这一行可能就不能正常运行了
      Class.forName("com.mysql.cj.jdbc.Driver");
      // 相较于方式三，可以省略如下的操作:
      // Driver driver = (Driver) clazz.newInstance();
      // 注册驱动
      // DriverManager.registerDriver(driver);

      /*
      此代码块是MySql驱动的源码，可以看到Driver的静态代码块中已经注册了驱动，所以注册的步骤可以省略
      static {
          try {
              java.sql.DriverManager.registerDriver(new com.mysql.cj.jdbc.Driver());
          } catch (SQLException E) {
              throw new RuntimeException("Can't register driver!");
          }
      }
        */

      // 3. 获取连接
      Connection conn = DriverManager.getConnection(url, user, password);
      System.out.println("方式四: " + conn);
  }
```

- 方式五(final 版): 将数据库连接需要的 4 个基本信息声明在配置文件中，通过读取配置文件的方式获取连接
- 此种方式的好处?

1. 实现了数据与代码的分离，实现了解耦。
2. 如果需要修改配置文件信息，可以避免程序重新打包。

- 目前使用`JUnit`测试时，`properties`配置文件的位置是在`resources`下

```properties
user=root
password=123456
url=jdbc:mysql://localhost:3310/jdbc_learn
driverClass=com.mysql.cj.jdbc.Driver
```

```java
  @Test
  public void getConnection5() throws Exception {
      // 1. 读取配置文件中的4个基本信息
      InputStream is = ConnectionTest.class.getClassLoader().getResourceAsStream("jdbc.properties");

      Properties props = new Properties();
      props.load(is);

      String user = props.getProperty("user");
      String password = props.getProperty("password");
      String url = props.getProperty("url");
      String driverClass = props.getProperty("driverClass");

      // 2. 加载驱动
      Class.forName(driverClass);

      // 3. 获取连接
      Connection conn = DriverManager.getConnection(url, user, password);
      System.out.println(conn);
  }
```

## 第 3 章 使用 PreparedStatement 实现 CRUD 操作

- 使用 Statement 操作数据表存在弊端:
  1. 存在拼串操作，繁琐
  2. 存在 SQL 注入问题: 利用某些系统没有对用户输入的数据进行充分的检查，而在用户输入的数据中注入非法的 SQL 语句段或命令，从而利用系统的SQL引擎完成恶意行为的做法。

![alt text](./images/12_01_sql_injection.png)

- 如何避免出现SQL注入? 只要用`PreparedStatement`(从`Statement`扩展而来)取代`Statement`。

---

- 使用`PreparedStatement`

```java
package com.atguigu3.preparedstatement.crud;

import org.junit.jupiter.api.Test;

import java.io.InputStream;
import java.sql.*;
import java.text.SimpleDateFormat;
import java.util.Properties;

/**
 * 使用PreparedStatement来替换Statement，实现对数据表的增删改操作。
 * 增删改; 查
 */
public class PreparedStatementUpdateTest {
    // 向customers表中添加一条记录
    @Test
    public void testInsert() {
        Connection conn = null;
        PreparedStatement ps = null;

        try {
            // 1. 读取配置文件中的4个基本信息
            InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties");

            Properties props = new Properties();
            props.load(is);

            String user = props.getProperty("user");
            String password = props.getProperty("password");
            String url = props.getProperty("url");
            String driverClass = props.getProperty("driverClass");

            // 2. 加载驱动
            Class.forName(driverClass);

            // 3. 获取连接
            conn = DriverManager.getConnection(url, user, password);

            // 4. 预编译SQL语句，返回PreparedStatement的实例
            String sql = "insert into customers(name, email, birth) values(?, ?, ?)"; // ?: 占位符
            ps = conn.prepareStatement(sql);

            // 5. 填充占位符
            ps.setString(1, "哪吒");
            ps.setString(2, "nezha@gmail.com");
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
            java.util.Date date = sdf.parse("1942-04-22");
            ps.setDate(3, new Date(date.getTime()));

            // 6. 执行SQL
            ps.execute();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            // 7. 资源的关闭
            try {
                if (ps != null)
                    ps.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
            try {
                if (conn != null)
                    conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

--- 

- 对于`增删改`操作，有一些固定的步骤，比如`获取连接`，`关闭资源`。
- 可以把这些固定的操作封装到`工具类`中。

```java
/**
 * 操作数据库的工具类
 */
public class JDBCUtils {

    /**
     * 获取数据库连接
     *
     * @return
     * @throws Exception
     */
    public static Connection getConnection() throws Exception {
        // 1. 读取配置文件中的4个基本信息
        InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties");

        Properties props = new Properties();
        props.load(is);

        String user = props.getProperty("user");
        String password = props.getProperty("password");
        String url = props.getProperty("url");
        String driverClass = props.getProperty("driverClass");

        // 2. 加载驱动
        Class.forName(driverClass);

        // 3. 获取连接
        return DriverManager.getConnection(url, user, password);
    }

    /**
     * 关闭连接和Statement
     *
     * @param conn
     * @param stmt
     */
    public static void closeResources(Connection conn, Statement stmt) {
        try {
            if (stmt != null)
                stmt.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }

        try {
            if (conn != null)
                conn.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

---

- 使用封装好的工具类来执行一个修改的操作

```java
  // 修改customers表中的一条记录
  @Test
  public void testUpdate() {
      Connection conn = null;
      PreparedStatement ps = null;
      try {
          // 1. 获取数据库的连接
          conn = JDBCUtils.getConnection();
          // 2. 预编译SQL语句，返回PreparedStatement的实例
          String sql = "update customers set name = ? where id = ?";
          ps = conn.prepareStatement(sql);
          // 3. 填充占位符
          ps.setObject(1, "莫扎特");
          ps.setObject(2, 18);
          // 4. 执行
          ps.execute();
          System.out.println("修改成功");
      } catch (Exception e) {
          e.printStackTrace();
      } finally {
          // 5. 资源的关闭
          JDBCUtils.closeResources(conn, ps);
      }
  }
```
