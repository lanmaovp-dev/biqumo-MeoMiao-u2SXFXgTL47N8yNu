
# 二. Spring Boot 中的 “依赖管理和自动配置” 详解透彻到底（附\+详细代码流程）


@

目录* [二. Spring Boot 中的 “依赖管理和自动配置” 详解透彻到底（附\+详细代码流程）](https://github.com)
* [1\. 如何理解 “ 约定优于配置 ”](https://github.com)
* [2\. Spring Boot 依赖管理 和 自动配置](https://github.com)
	+ [2\.1 Spring Boot 的依赖管理](https://github.com)
		- [2\.1\.1 什么是依赖管理](https://github.com)
		- [2\.1\.2 修改自动仲裁 / 默认版本号](https://github.com)
	+ [3\.1 starter 场景启动器](https://github.com)
		- [3\.1\.1 starter 场景启动器的说明介绍](https://github.com)
		- [3\.1\.2 Spring Boot 官方提供的 starter 场景启动器](https://github.com)
		- [3\.1\.3 Spring Boot支持的第三方 starter 场景启动器](https://github.com)
	+ [4\.1 Spring Boot 的自动配置](https://github.com)
		- [4\.1\.1 Spring Boot 自动配置有哪些](https://github.com)
		- [4\.1\.2 如何修改 Spring Boot 中的默认配置](https://github.com)
	+ [5\.1 如何修改默认配置](https://github.com)
		- [5\.1\.1 如何修改默认扫描包结构](https://github.com):[飞数机场](https://ze16.com)
		- [5\.1\.2 Spring Boot中的 resources(类路径下)\\application.properties 配置大全](https://github.com)
		- [5\.1\.3 演示：使用一些常用的配置](https://github.com)
		- [5\.1\.4 Spring Boot 中 application.properties 自定义配置](https://github.com)
	+ [6\.1 Spring Boot 中在哪里配置读取 application.properties 配置文件](https://github.com)
	+ [7\.1 自动配置，遵守按需加载原则](https://github.com)
* [3\. 总结：](https://github.com)
* [4\. 最后：](https://github.com)



---


# 1\. 如何理解 “ 约定优于配置 ”


约定优于配置（Convention over Configuration / CoC）,又称约定编程，是一种软件设计规范，本质上是对系统，类库或框架中一些东西。


一个大众化合理的默认值（缺省值）
2\. 例如在模型中存在一个名为User的类，那么对应到数据库会存在一个名为 user 的表，只有偏离这个约定时才需要做相关的配置（例如
你想将表名命名为：t\_user等非user时才需要写关于这个名字的配置）
3\. 简单来说就是假如你所**期待的配置** 与 **约定的配置** 一致，那么就可以不做任何配置，约定不符合期待时，才需要对约定进行替换配置。
4\. 约定优于配置：为什么要搞一个“约定优于配置”
约定其实就是一个规范，遵循了规范，那么就存在通用性。存在了通用性，那么事情就会变得**相对简单** ，程序员之间的沟通成本就会降低，工作效率会提升，合作也会变得更加简单。
5\. 生活中，这样的情况，大量存在：比如：两个都是男生，你说一起去上厕所去，那么，他们两个人都是（默认，约定好的都是去男生厕所，而特意说明是：去女生厕所。）
6\. 注意：误区：默认是采用约定的，但是你配置了，还是采用你的配置的信息的。


# 2\. Spring Boot 依赖管理 和 自动配置


## 2\.1 Spring Boot 的依赖管理


### 2\.1\.1 什么是依赖管理


1. spring\-boot\-starter\-parent 还有父项目，声明了开发中常用的依赖的版本号
2. 并且进行 **自动版本仲裁** ，即如果程序员没有指定某个 依赖的 `jar` 的版本，则以父项目指定的版本为准。


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908251-1368721013.png)


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908295-2059300553.png)


### 2\.1\.2 修改自动仲裁 / 默认版本号


如果 Spring Boot 当中默认配置的 `jar` 依赖的版本，不是，我们所想要的该如何配置修改成我们自己想要的呢。


如何修改 Spring Boot 当中对应 jar。


**演示：** 将 SpringBoot mysql 驱动修改成 5\.1\.49 版本的。


首先，我们先要学会如何查看到对应我们所需的 `jar` 在 Spring Boot 的默认配置当中使用的是什么版本的。



> 在spring\-boot\-dependencies\\2\.5\.3\\spring\-boot\-dependencies\-2\.5\.3\.pom，中我们可以找到对应的 `jar` 包的依赖版本，如下图所示：这里 Spring Boot 对于 MySQL 的依赖默认配置是 `8.0.26` 的
> 
> 
> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908299-523969497.png)



> 查 看 spring\-boot\-dependencies.pom 里 面 规 定 当前 依 赖 的 版 本 对 应 的 key , 这 里 是 mysql.version
> 
> 
> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908355-198458715.png)



> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908293-2035093079.png)


修改 Spring Boot 当中对应 jar；有两种方式：


* **方式一：** 直接在项目的 `pom.xml` 文件当中直接导入，自己想要的 `jar` 的版本即可。
* **方式二：** 和方式一是一样的，不同的是编写方式，有所不同：“方式二：是将 `jar` 信息和 `jar的版本` 信息分开来编写的了”。


**方式一：** 修改 pom.xml 重写配置 当更新 Maven 时，就依赖到新的 ，mysql 驱动。


直接导入我们自己想要的 依赖的`jar` 的配置，然后，更新 Maven 即可。


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908356-1972537174.png)



```
  
        <dependency>
            <groupId>mysqlgroupId>
            <artifactId>mysql-connector-javaartifactId>
            <version>5.1.49version> 
        dependency>

```

然后，我们在刷新 Maven，重新加载 `jar`


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908261-849966010.png)



> **注意：** 如果配置的依赖`jar` 包时，不指明就是version 版本的话，就会采用 SpringBoot 约定好的(也就是默认的8\.26\)版本的。
> 
> 
> 如下：我们将指明的版本的 version 注释掉，再次更新，看看。
> 
> 
> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908352-653858479.png)


完整的 `pom.xml` 信息。



```
xml version="1.0" encoding="UTF-8"?
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0modelVersion>

    <groupId>com.rainbowseagroupId>
    <artifactId>quickstartBlogartifactId>
    <version>1.0-SNAPSHOTversion>


    
    <parent>
        <groupId>org.springframework.bootgroupId>
        <artifactId>spring-boot-starter-parentartifactId>
        <version>2.5.3version>
    parent>


    
    <dependencies>
        <dependency>
            <groupId>org.springframework.bootgroupId>
            <artifactId>spring-boot-starter-webartifactId>
        dependency>

        
        <dependency>
            <groupId>mysqlgroupId>
            <artifactId>mysql-connector-javaartifactId>
            <version>5.1.49version> 
        dependency>
    dependencies>




project>

```

**方式二：** 将依赖`jar` 包的配置信息，和配置版本分开来编写。



> * 在  标签中，配置 jar 的**版本信息** ，可以配置多个不同的 jar 的版本信息。
> * 在  标签中，配置 jar 的 **配置坐标信息** ，可以配置多个 jar


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908333-997344092.png)



```
    <properties>
        <mysql.version>5.1.49mysql.version>
    properties>


    
    <dependencies>
        
        <dependency>
            <groupId>mysqlgroupId>
            <artifactId>mysql-connector-javaartifactId>
        dependency>
    dependencies>

```

更新 Maven ，重新加载 jar 。


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908270-2085934043.png)


完整的 `pom.xml` 信息。



```
xml version="1.0" encoding="UTF-8"?
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0modelVersion>

    <groupId>com.rainbowseagroupId>
    <artifactId>quickstartBlogartifactId>
    <version>1.0-SNAPSHOTversion>


    
    <parent>
        <groupId>org.springframework.bootgroupId>
        <artifactId>spring-boot-starter-parentartifactId>
        <version>2.5.3version>
    parent>


    <properties>
        <mysql.version>5.1.49mysql.version>
    properties>


    
    <dependencies>
        <dependency>
            <groupId>org.springframework.bootgroupId>
            <artifactId>spring-boot-starter-webartifactId>
        dependency>

        
        <dependency>
            <groupId>mysqlgroupId>
            <artifactId>mysql-connector-javaartifactId>
        dependency>
    dependencies>




project>

```

## 3\.1 starter 场景启动器


### 3\.1\.1 starter 场景启动器的说明介绍


开发中我们引入了相关场景的 starter，这个场景中所有的相关依赖都引入进来了，比如：我们之前学习到的 web 开发引入了，该 starter 将导入与 web 开发相关的所有包。


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908334-776826339.png)


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908230-2139926601.png)


**依赖树** : 可以看到 spring\-boot\-starter\-web ，帮我们引入了 spring\-web mvc，spring\-web 开发模块，还引入了 spring\-boot\-starter\-tomcat 场景，spring\-boot\-starter\-json 场景，这些场景下面又引入了一大堆相关的包，这些依赖项可以快速启动和运行一个项目，提高开发效率.



> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908267-542713312.png)



> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908367-1719082381.png)


所有场景启动器最基本的依赖就是 `spring-boot-starter` , 前面的依赖树分析可以看到,


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908260-1098516480.png)
![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908354-135218428.png)


这个依赖也就是 SpringBoot 自动配置的核心依赖


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908360-1295475652.png)


### 3\.1\.2 Spring Boot 官方提供的 starter 场景启动器


Spring Boot 官方提供的 starter 场景启动器的介绍说明：[https://docs.spring.io/spring\-boot/reference/using/build\-systems.html\#using.build\-systems.starters](https://github.com) 。


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908350-518753093.png)


意思就是说：在开发中我们经常会用到 spring\-boot\-starter\-xxx ，比如 spring\-boot\-starter\-web，该场 景是用作 web 开发，也就是说 xxx 是某种开发场景。


这时候，我们只要引入 starter，这个场景的所有常规需要的依赖我们都自动引入。


Spring Boot3 支持的所有场景如下：官方地址：[https://docs.spring.io/spring\-boot/reference/using/build\-systems.html\#using.build\-systems.starters](https://github.com)


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908482-297140445.png)


### 3\.1\.3 Spring Boot支持的第三方 starter 场景启动器


Spring Boot 也支持第三方starter。


第三方starter不要从spring\-boot 开始，因为这是官方spring\-boot保留的命名方式的。
第三方启动程序通常以项目名称开头，例如：名“XXx”的第三方启动程序项目通常被命名为：“XXx\-spring\-boot\-stater”。


也就是说：xxx\-spring\-boot\-starter是第三方为我们提供的简化开发的场景启动器


## 4\.1 Spring Boot 的自动配置


我们是否还记得，前面学习SSM整合时，需要配置 Tomcat ，配置 Spring MVC，以及配置如何扫描包，配置字符过滤器，配置视图解析器，文件上传等 ✏️✏️✏️ [SSM 整合(Spring \+ MyBatis；Spring \+ Spring MVC)\-CSDN博客](https://github.com) 。非常麻烦。而在 Spring Boot 中，存在**自动配置** 机制，提高开发效率。



> **简单回顾以前 SSM 整合的配置：**
> 
> 
> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908332-2017707448.png)



> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908480-234496871.png)


### 4\.1\.1 Spring Boot 自动配置有哪些


1. 自动配置 Tomcat


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908270-1497464448.png)


2. 自动配置了 Spring MVC


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908363-1717797201.png)


3. 自动配置了 Web 常用功能： 比如 字符过滤器。


我们可以通过获取到的 ioc 容器，查看容器创建的组件来验证。


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908277-538290017.png)



```
package com.rainbowsea.springboot;


import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;


// @SpringBootApplication  表示是一个 springboot 应用
@SpringBootApplication
public class MainApp {
    public static void main(String[] args) {
        // 启动 SpringBoot 应用程序
        ConfigurableApplicationContext ioc = SpringApplication.run(MainApp.class, args);

        // 查看容器里面的组件
        String[] beanDefinitionNames = ioc.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            System.out.println("beanDefinitionName ---" + beanDefinitionName);
        }


    }
}


```

![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908361-1879383911.png)


更加直接查看的方式：我们可以通过 Debug 的方式，在 ioc 打上断点，从而查看 ioc 容器中有哪些 Bean 对象被创建了。



> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908341-1770810131.png)



> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908364-416103314.png)



> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908338-823906811.png)



> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908211-1032497494.png)



> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908266-1975858051.png)



> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908300-1073795741.png)



在Spring Boot 的自动配置中，我们不需要像Spring MVC 中一定要配置**扫描包** 。Spring Boot 有默认的扫描包结构。


如下是来自 Spring Boot 官方文档说明 【[https://docs.spring.io/spring\-boot/reference/using/structuring\-your\-code.html\#using.structuring\-your\-code.using\-the\-default\-package】的：自动扫描的包结构。你按照该“约定/自动扫描包结构”，开发项目，就不需要配置扫描包了，直接用](https://github.com) Spring Boot 约定的/默认的即可。


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908229-1711180426.png)



```
com
 +- example
     +- myapplication
         +- MyApplication.java
         |
         +- customer
         |   +- Customer.java
         |   +- CustomerController.java
         |   +- CustomerService.java
         |   +- CustomerRepository.java
         |
         +- order
             +- Order.java
             +- OrderController.java
             +- OrderService.java
             +- OrderRepository.java

```


> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908267-1834976516.png)


### 4\.1\.2 如何修改 Spring Boot 中的默认配置


**演示：** 要求能扫描 com.rainbowsea 包下的 HiController.java 应该如何处理 ?



> 首先我们先测试一下，不按照 Spring Boot 默认的包结构创建的，主程序类。会怎么样。
> 
> 
> 在 quickstartBlog\\src\\main\\java\\com\\rainbowsea\\目录/包下创建一个，名为 HiController.java主程序类。 并测试， 这时是访问不到的。
> 
> 
> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908345-650832580.png)



> 运行主程序，打开浏览器输入：[http://localhost:8080/hi](https://github.com) 测试。
> 
> 
> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908335-1659186267.png)



> **原因是：hello 配置的 HelloController 是在 Spring Boot 的默认自动配置的包结构下的，而我们的 hi 配置的 HiController 并不是在，Spring Boot 的自动配置打包结构下的，而我们自己有没有配置包扫描，Spring Boot 自然找不到了。**
> 
> 
> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908481-1136918569.png)


## 5\.1 如何修改默认配置


### 5\.1\.1 如何修改默认扫描包结构


上面我们测试了，在 com.rianbowsea 包下的 HiController.java 是扫描不到了，扫描不到，也就无法加入到 ioc 容器当中被 Spring Boot管理起来了，也就无法被访问到了。


所以想要，让 HiController.java 可以被扫描到，就解决了这个，无法访问找不到的问题了。


**方法：**


我们修改 MainApp.java, 增加扫描的包 。


使用 `@SpringBootApplication` 注解当中的 `scanBasePackages` 注解属性，该属性的值：就是让Spring Boot 扫描的，包路径。


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908368-1815888584.png)


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908339-2033026683.png)



```
package com.rainbowsea.springboot;


import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;


// @SpringBootApplication  表示是一个 springboot 应用
// 直接在    SpringBootApplication 注解后指定,让 Spring Boot 要扫描到的包路径
 @SpringBootApplication(scanBasePackages = {"com.rainbowsea"})
public class MainApp {
    public static void main(String[] args) {
        // 启动 SpringBoot 应用程序
        ConfigurableApplicationContext ioc = SpringApplication.run(MainApp.class, args);

        // 查看容器里面的组件
        String[] beanDefinitionNames = ioc.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            System.out.println("beanDefinitionName ---" + beanDefinitionName);
        }


    }
}


```

配置好之后，我们再次重启 ，运行试试。


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908362-1053534176.png)


### 5\.1\.2 Spring Boot中的 resources(类路径下)\\application.properties 配置大全


SpringBoot 项目最重要也是最核心的配置文件就是 application.properties，所有的框架配 置都可以在这个配置文件中说明 ：[Spring Boot 框架中配置文件 application.properties 当中的所有配置大全\-CSDN博客](https://github.com)


### 5\.1\.3 演示：使用一些常用的配置


在Spring Boot中各种配置都有默认，而想要修改配置的话， 可 以 在 resources\\application.properties文件中修改。至于`application.properties` 文件是需要我们手动创建的。



> **特别注意：该文件名必须是 `application.properties`，后缀也不可以修改。强烈建议将其放到 类的根路径下(也就是`resources` 目录下 )**
> 
> 
> 其次是，注意：在`.properties` 后缀的配置文件，当中编写，不要有空格，尽量不要有空格。


如下：


* 演示一：我们设置该服务器 Tomcat ，我们项目启动的端口，默认是 8080，这里我们设置为 9090。


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908197-605826527.png)


编写配置：设置修改 Server 的监听窗口为 9090



```
# 修改server的监听窗口为 9090
# Tomcat started on port(s): 9090 (http) with context path ''
server.port=9090


```

![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908359-1333279419.png)


编写配置好后，运行主程序，重新加载程序，看看。


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908369-1731640724.png)


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908521-761495404.png)


* **演示2：** 修改文件上传的大小；


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908500-372934413.png)



```
# 修改server的监听窗口为 9090
# Tomcat started on port(s): 9090 (http) with context path ''
server.port=9090


# 修改文件上传的大小
# 解读一下这个配置是在哪里读取！
#multipart.max-file-size，属性可以指定SpringBoot 上传文件的大小限制（体现“约定优于配置”）
# 默认骗子hi最总都是映射到某个类上，比如：multipart.max-file-size
# 会映射/关联到MultipartProperties 类
# 把光标放在该属性，输入 ctrl + b,或者是按住 ctrl + 点击，就可以定位这个属性是管理到哪个类(属性)
spring.servlet.multipart.max-file-size=10MB


```

这里，我们想要看到效果的话，需要Debug，打上断点，进行 Debug 调试才可以比较清除的看到效果。



> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908365-1122410488.png)



> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908331-1430361026.png)



> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908353-872028481.png)


* **演示三：** 配置开始的项目根的映射路径 .


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908333-1504377901.png)



```
# 修改server的监听窗口为 9090
# Tomcat started on port(s): 9090 (http) with context path ''
server.port=9090


# 修改文件上传的大小
# 解读一下这个配置是在哪里读取！
#multipart.max-file-size，属性可以指定SpringBoot 上传文件的大小限制（体现“约定优于配置”）
# 默认骗子hi最总都是映射到某个类上，比如：multipart.max-file-size
# 会映射/关联到MultipartProperties 类
# 把光标放在该属性，输入 ctrl + b,或者是按住 ctrl + 点击，就可以定位这个属性是管理到哪个类(属性)
spring.servlet.multipart.max-file-size=10MB




# 配置开始的项目根的映射路径
server.servlet.context-path=/rainbowsea


```

**重新启动程序，测试：**


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908482-974974123.png)


**特别说明：讲解**



> 上述的配置文件时在哪里读取的
> 
> 
> 比如：multipart.max\-file\-size，属性可以指定SpringBoot 上传文件的大小限制（体现“约定优于配置”）
> 
> 
> 默认 Spring Boot 都会将其映射到某个类上，比如：multipart.max\-file\-size，会映射/关联到MultipartProperties 类。配置都会被映射到相对应的类上的。
> 
> 
> 而我们将光标放在该属性，输入 ctrl \+ b,或者是按住 ctrl \+ 点击，就可以定位这个属性是管理到哪个类(属性)
> 
> 
> 
> > ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908336-1865400064.png)


### 5\.1\.4 Spring Boot 中 application.properties 自定义配置


在 Spring Boot 中，我们还可以还在 `.properties` 文件中自定义配置，通过 `@Value("${}")` 的方式来获取对应属性值。


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908307-1985667685.png)



```
# 修改server的监听窗口为 9090
# Tomcat started on port(s): 9090 (http) with context path ''
server.port=9090


# 修改文件上传的大小
# 解读一下这个配置是在哪里读取！
#multipart.max-file-size，属性可以指定SpringBoot 上传文件的大小限制（体现“约定优于配置”）
# 默认骗子hi最总都是映射到某个类上，比如：multipart.max-file-size
# 会映射/关联到MultipartProperties 类
# 把光标放在该属性，输入 ctrl + b,或者是按住 ctrl + 点击，就可以定位这个属性是管理到哪个类(属性)
spring.servlet.multipart.max-file-size=10MB




# 配置开始的项目根的映射路径
server.servlet.context-path=/rainbowsea


# 自定义配置属性
my.website=https://www.baidu.com


```

![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908289-840157547.png)



```
package com.rainbowsea;


import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller // 注入到 ioc 容器当中
public class HiController {
    @RequestMapping("/hi")  // 设置请求映射路径
    @ResponseBody   // 表示返回的是一个 json/字符串，而不是一个网页
    public String hi(){
        System.out.println("website" + website);
        return "hi~, spring boot";
    }

    // 需求website，属性值从application.properties 的 k-v
    @Value("${my.website}")
    private String website;
}


```

运行测试：


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908286-1695315477.png)



> 当然，我们也时可以 将其 配置到 Bean 对象上的，只不过，配置到 Bean 对象上的话，有更好的方式，可以用 Spring Boot 的容器管理。


## 6\.1 Spring Boot 中在哪里配置读取 application.properties 配置文件


打开 ConfigFileApplicationListener.java , 看一下源码；


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908304-1784371969.png)


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908481-1073900760.png)



```
// 指明: application.properties 可以存放的位置在哪里，Spring Boot 可以成功读取到 
private static final String DEFAULT_SEARCH_LOCATIONS = "classpath:/,classpath:/config/,file:./,file:./config/*/,file:./config/";

// 指明: 配置类，要是为：application，  
private static final String DEFAULT_NAMES = "application";

```

## 7\.1 自动配置，遵守按需加载原则


自动配置遵守按需加载原则：也就是说，引入了哪个场景 `starter启动器` 就会加载该场景关联，的 jar 包，没有引入的 starter 则不会加载其关联 jar 。


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908372-1390760800.png)


SpringBoot 所有的自动配置功能都在 spring\-boot\-autoconfigure 包里面；


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908336-1068640375.png)


在Spring Boot 中的自动配置包，一般是 XXxAutoConfiguration.java 对应 XXxProperties.java，如图


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908337-641145912.png)




---


# 3\. 总结：


1. Spring Boot 理解其中的 “约定优于配置”，又称约定编程，是一种软件设计规范，本质上是对系统，类库或框架中一些东西。减少不必要的配置，使用约定(默认的配置)吗。
2. Spring Boot 当中的 jar 版本的查看，以及 修改 Spring Boot 当中的 jar 版本的为字节所需。
	1. **方式一：** 直接在项目的 `pom.xml` 文件当中直接导入，自己想要的 `jar` 的版本即可。
	2. **方式二：** 和方式一是一样的，不同的是编写方式，有所不同：“方式二：是将 `jar` 信息和 `jar的版本` 信息分开来编写的了”。
3. Spring Boot 当中的 starter 场景启动器，Spring Boot 官方提供的 starter 场景启动器的介绍说明：[https://docs.spring.io/spring\-boot/reference/using/build\-systems.html\#using.build\-systems.starters](https://github.com) 。
4. Spring Boot 自动配置，Spring Boot 默认的扫描包结构，修改配置扫描包
5. Spring Boot当中核心配置文件 `application.properties` 的理解和使用。 `application.properties` 的每个配置属性，都对应这 Spring Boot 当中的某个类当中的某个属性，并且加入到了 ioc 容器当中。
6. 在Spring Boot 中的自动配置包，一般是 XXxAutoConfiguration.java 对应 XXxProperties.java


# 4\. 最后：



> “在这个最后的篇章中，我要表达我对每一位读者的感激之情。你们的关注和回复是我创作的动力源泉，我从你们身上吸取了无尽的灵感与勇气。我会将你们的鼓励留在心底，继续在其他的领域奋斗。感谢你们，我们总会在某个时刻再次相遇。”
> 
> 
> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202408/3084824-20240829223908475-989009559.gif)


