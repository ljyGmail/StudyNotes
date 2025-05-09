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