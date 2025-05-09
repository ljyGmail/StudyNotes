# 001 SpringBoot3 课程简介

# 002 SpringBoot3 快速入门 特新介绍

# 003 SpringBoot3 快速入门 实例demo

# 004 SpringBoot3 快速入门 demo小结

## 1. 开发流程

### 1. 创建项目

maven项目
```xml
 <!-- 所有springboot的项目都必须继承自spring-boot-starter-parent -->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.0.5</version>
</parent>
```

### 2. 导入场景

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

### 3. 主程序

```java
@SpringBootApplication
public class MainApplication {

    public static void main(String[] args) {
        SpringApplication.run(MainApplication.class, args);
    }
}
```

### 4. 业务

```java
@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello, Spring Boot 3!!!";
    }
}
```

### 5. 测试
默认启动访问: `localhost:8080`

### 6. 打包

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

## 2. 特性小结

### 1. 简化整合
导入相关的场景，拥有相关的功能。 -> 场景启动器
默认支持的所有场景: [Starters](https://docs.spring.io/spring-boot/reference/using/build-systems.html#using.build-systems.starters)

* 官方提供的场景: 命名为`spring-boot-starter-*`
* 第三方提供场景: 命名为`*-spring-boot-starter`

> 场景一导入，万物皆就绪

### 2. 简化开发
无需编写任何配置，直接开发业务

### 3. 简化配置
`application.properties`
* 集中式管理配置。只需要修改这个文件就行。
* 配置基本都有默认值
* 能写的所有配置都在: [Common Application Properties](https://docs.spring.io/spring-boot/appendix/application-properties/index.html)

### 4. 简化部署
打包为可执行的jar包。
linux服务器上有java环境就行。

### 5. 简化运维
修改配置(外部放一个application.properties文件)、监控、健康检查。