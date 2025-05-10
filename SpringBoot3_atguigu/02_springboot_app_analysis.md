# 006 SpringBoot3 细节分析 依赖管理机制

## 3. 应用分析

### 1. 依赖管理机制

思考:

1. 为什么导入`starter-web`所有相关依赖都会导入进来？

- 开发什么场景，导入什么**场景启动器**。
- Maven 的**依赖传递原则**。A-B-C: A 就拥有 B 和 C。
- 导入场景启动器，就会自动把这个场景的所有核心依赖全部导入进来。

2. 为什么版本号都不用写？

- 每个 boot 项目都有一个父项目`spring-boot-starter-parent`
- parent 的父项目是`spring-boot-dependencies`
- 父项目: 版本仲裁中心，把所有常见的 jar 的依赖版本都声明好了。
  比如: `mysql-connector-j`

3. 自定义版本号

- 利用 Maven 的就近原则
  - 直接在`properties`标签中声明父项目用的版本属性的 key
  - 直接在导入依赖的时候声明版本

4. 第三方的 jar 包

- boot 父项目没有管理的需要自行声明好

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.16</version>
</dependency>
```

# 007 SpringBoot3 细节分析 自动配置机制

### 2. 自动配置机制

1. 初步理解

- 自动配置的 Tomcat、SpringMVC 等

  - 导入场景，容器中就会自动配置好这个场景的核心组件
  - 以前: `DispatcherServlet`、`ViewResolver`、`CharacterEncodingFilter`
  - 现在: 自动配置好的这些组件
  - 验证: **容器中有了什么组件，就具有什么功能**

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

- 默认的包扫描规则

  - @SpringBootApplication 标注的类就是主程序类
  - **SpringBoot 只会扫描主程序所在的包及其下面的子包，自动的 component-scan 功能**
  - **自定义扫描路径**
    - @SpringBootApplication(scanBasePackages = "com.atguigu")
    - `@ComponentScan("com.atguigu")`直接指定扫描的路径

- 配置默认值
  - 配置文件的所有配置项是和某个**类的对象**值进行一一绑定的。
  - 绑定了配置文件中每一项值的类: **配置属性类**
  - 比如:
    - `ServerProperties`绑定了所有 Tomcat 服务器有关的配置
    - `MultipartProperties`绑定了所有文件上传相关的配置
    - ...参照官方文档: 或者参照绑定的**属性类**
- 按需加载自动配置
  - 导入场景`spring-boot-starter-web`
  - 场景启动器除了会导入相关功能依赖，还会导入一个`spring-boot-starter`，是所有`starter`的`starter`，基础核心 starter
  - `spring-boot-starter`导入了一个包`spring-boot-autoconfigure`。包里面都是各种场景的`AutoConfiguration`**自动配置类**
  - 虽然全场景的自动配置都在`spring-boot-autoconfigure`这个包，但是不是全都开启的。
    - 导入哪个场景就开启哪个自动配置

> 总结: 导入场景启动器、触发`spring-boot-autoconfigure`这个包的自动配置生效、容器中就会具有相关场景的功能。

# 008 SpringBoot3 常用注解 组件注册

## 4. 核心技能

### 1. 常用注解

SpringBoot 摒弃了 XML 配置方式，改为全注解驱动。

#### 1. 组件注册

`@Configuration`、`@SpringBootConfiguration`
`@Bean`、`@Scope`
`@Controller`、`@Service`、`@Repository`、`@Component`
`@Import`
`@ComponentScan`

步骤

1. `@Configuration`编写一个配置类
1. 在配置类中，自定义方法给容器中注册组件、配合@Bean
1. 或使用`@Import`导入第三方的组件

#### 2. 条件注解

> 如果注解指定的条件成立，则触发指定行为

<span style="color: red">@ConditionOnXxx</span>
<span style="color: purple; font-weight: bold">@ConditionOnClass: 如果类路径中存在这个类，则触发指定行为</span>
<span style="color: blue; font-weight: bold">@ConditionOnMissingClass: 如果类路径中不存在这个类，则触发指定行为</span>
<span style="color: purple; font-weight: bold">@ConditionOnBean: 如果容器中存在这个 Bean(组件)，则触发指定行为</span>
<span style="color: blue; font-weight: bold">@ConditionOnMissingBean: 如果容器中不存在这个 Bean(组件)，则触发指定行为</span>

> 场景:

- 如果存在`FastsqlException`这个类，给容器中放一个`Cat`组件，名为`cato01`
- 否则，就给容器中放一个`Dog`组件，名为`dog01`
- 如果系统中有`dog01`这个组件，就给容器中放一个`User`组件，名为`zhangsan`
- 否则，就放一个`User`组件，名为`lisi`
