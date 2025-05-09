## 002 SpringBoot3 快速入门 特新介绍

## 003 SpringBoot3 快速入门 实例demo

## 004 SpringBoot3 快速入门 demo小结

## 开发流程

### 1. 开发流程

#### 1. 创建项目

maven项目
```xml
 <!-- 所有springboot的项目都必须继承自spring-boot-starter-parent -->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.0.5</version>
</parent>
```

#### 2. 导入场景

场景启动器
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <version>3.4.1</version>
    </dependency>
</dependencies>
```

#### 3. 主程序

```java
@SpringBootApplication
public class MainApplication {

    public static void main(String[] args) {
        SpringApplication.run(MainApplication.class, args);
    }
}
```

#### 4. 业务

```java
@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello, Spring Boot 3!!!";
    }
}
```

#### 5. 测试
默认启动访问: `localhost:8080`

#### 6. 打包

```xml
<!-- SpringBoot应用打包插件 -->
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

`mvn clean package`把项目打成可执行的jar包

`java -jar demo.jar`启动项目

## 特性小结
