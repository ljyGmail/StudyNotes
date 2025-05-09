# 006 SpringBoot3 细节分析 依赖管理机制

## 3. 应用分析

### 1. 依赖管理机制
思考:

1. 为什么导入`starter-web`所有相关依赖都会导入进来？
* 开发什么场景，导入什么**场景启动器**。
* Maven的**依赖传递原则**。A-B-C: A就拥有B和C。
* 导入场景启动器，就会自动把这个场景的所有核心依赖全部导入进来。

2. 为什么版本号都不用写？
* 每个boot项目都有一个父项目`spring-boot-starter-parent`
* parent的父项目是`spring-boot-dependencies`
* 父项目: 版本仲裁中心，把所有常见的jar的依赖版本都声明好了。
比如: `mysql-connector-j`

3. 自定义版本号
* 利用Maven的就近原则
    * 直接在`properties`标签中声明父项目用的版本属性的key
    * 直接在导入依赖的时候声明版本

4. 第三方的jar包
* boot父项目没有管理的需要自行声明好
```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.16</version>
</dependency>
```

### 2. 自动配置机制

1. 初步理解

* 自动配置的Tomcat、SpringMVC等

    * 导入场景，容器中就会自动配置好这个场景的核心组件
    * 以前: `DispatcherServlet`、`ViewResolver`、`CharacterEncodingFilter`
    * 现在: 自动配置好的这些组件
    * 验证: **容器中有了什么组件，就具有什么功能**
```java
    public static void main(String[] args) {
        // Java10: 局部变量类型的自动推断
        var ioc = SpringApplication.run(MainApplication.class, args);

        // 1. 获取容器中所有组件的名字
        String[] names = ioc.getBeanDefinitionNames();

        // 2. 挨个遍历: dispatcherServlet、beanNameResolver、characterEncodingFilter、multipartResolver
        // SpringBoot把以前配置的核心组件现在都给我们自动给配置好了
        for (String name : names) {
            System.out.println(name);
        }
    }
```
* 默认的包扫描规则
    * @SpringBootApplication标注的类就是主程序类
    * **SpringBoot只会扫描主程序所在的包及其下面的子包，自动的component-scan功能**
    * **自定义扫描路径**
        * @SpringBootApplication(scanBasePackages = "com.atguigu")
        * `@ComponentScan("com.atguigu")`直接指定扫描的路径

* 配置默认值
    * 配置文件的所有配置项是和某个**类的对象**值进行一一绑定的。
    * 绑定了配置文件中每一项值的类: **配置属性类**
    * 比如: 
        * `ServerProperties`绑定了所有Tomcat服务器有关的配置
        * `MultipartProperties`绑定了所有文件上传相关的配置
        * ...参照官方文档: 或者参照绑定的**属性类**
* 按需加载自动配置
    * 导入场景`spring-boot-starter-web`
    * 场景启动器除了会导入相关功能依赖，还会导入一个`spring-boot-starter`，是所有`starter`的`starter`，基础核心starter
    * `spring-boot-starter`导入了一个包`spring-boot-autoconfigure`。包里面都是各种场景的`AutoConfiguration`**自动配置类**
    * 虽然全场景的自动配置都在`spring-boot-autoconfigure`这个包，但是不是全都开启的。
        * 导入哪个场景就开启哪个自动配置

> 总结: 导入场景启动器、触发`spring-boot-autoconfigure`这个包的自动配置生效、容器中就会具有相关场景的功能。