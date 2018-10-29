## Springboot入门

以前需要整合三大框架，配置一大堆文件

springboot：J2EE一站式解决方案

使用嵌入式Servlet容器，无需打包成war包

starters自动依赖与版本控制

大量的自动配置

无需大量的XML，无代码生成，开箱即用；

 微服务：一个应用应该是一组小型服务；

## 配置

1. 设置maven配置文件xml的profile标签

```
	<profile>
	  <id>jdk-1.8</id>
	  <activation>
		<activeByDefault>true</activeByDefault>
		<jdk>1.8</jdk>
	  </activation>
	  <properties>
		<maven.compiler.source>1.8</maven.compiler.source>
		<maven.compiler.target>1.8</maven.compiler.target>
		<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
	  </properties>
	</profile>
```

2. IDEA setting maven 指定maven路径

## 新建一个helloworld

1. 新建一个Maven

```

```

2. 导入Springboot相关的依赖；

```xml
<dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter</artifactId>
</dependency>

<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

3. 编写一个主程序；启动SpringBoot应用

```java
@SpringBootApplication
public class HelloWorldMainApplication {
    public static void main(String[] args) {
        SpringApplication.run(HelloWorldMainApplication.class,args);
    }
}
```

4. 编写相关的Controller、Service

```
@Controller
public class HelloController {
    @ResponseBody
    @RequestMapping("/hello")
    public String hello(){
        return "Hello World!";
    }
}
```

5. 运行主程序测试

6. 简化部署

```
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

## POM文件

#### 1.父项目

```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.6.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
他的父项目：
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>2.0.6.RELEASE</version>
        <relativePath>../../spring-boot-dependencies</relativePath>
    </parent>
他。。。
    <properties>
        <activemq.version>5.15.6</activemq.version>
        <antlr2.version>2.7.7</antlr2.version>
	</properties>
真正管理SpringBoot应用里面的所有依赖版本
```

SpringBoot的版本仲裁中心；

默认是不需要写版本；（没有在dependencies里面管理的依赖自然需要声明版本号）

#### 2.导入的依赖

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
```

spring-boot-starter-web:spring-boot场景启动器，帮我们导入了web模块正常运行所依赖的组件；

SpringBoot将所有的功能场景都抽取出来，做成了一个个的starters（启动器），只需要在项目里面引入这些starter相关场景的所有依赖都会导入进来。要什么功能就导入什么场景的启动器。

如：

```

spring-boot-starter-jdbc   //Starter for using JDBC with the HikariCP connection pool
spring-boot-starter-web   //Starter for building web, including RESTful, applications using Spring MVC. Uses Tomcat as the default embedded container
spring-boot-starter-mail  //Starter for using Java Mail and Spring Framework’s email sending support
...
```

### 3.主程序类，主入口类

```java
@SpringBootApplication
public class HelloWorldMainApplication {
    public static void main(String[] args) {
        SpringApplication.run(HelloWorldMainApplication.class,args);
    }
}
```

@SpringBootApplication:Spring Boot应用标注在某个类上说明这个类时SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用；

它是一个组合类：

```
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
```

@EnableAutoConfiguration:开启自动配置功能；

**将主配置类的所在包及下面的所有子包里面的所有组件扫描到Spring容器；**

## 使用Spring Initializer快速创建SpringBoot项目

直接选择创建一个SpringInitializer项目，在里面勾选需要的模块，会自动联网创建；

默认生成的SpringBoot项目：

- 主程序已经生成好了，我们只需要我们自己的逻辑
- resources文件中目录结构：
  - static：保存所有的静态资源；js css images
  - template：保存所有的模板页面；（Spring Boot默认jar包使用嵌入式的Tomcat，默认不支持jsp页面）；可以使用模板引擎（freemarker、thymeleaf）；
  - application.properties:SpringBoot应用的配置文件 ，如：server.port:8081

## Spring Boot 配置文件

- application.properties

```
server.port=8081
```

- application.yml

```yml
server:
	port:8081
```

#### YAML语法

##### 1.基本语法

k:(空格) v：表示一对键值（空格必须有）

以空格的缩进来控制层级关系；只要左对齐的一列数据，都是同一个层级；大小写敏感；

##### 2.值的写法

##### 字面量：普通的值（数字，字符串，布尔）冒号后面一定要有空格

k：v 字面量直接写；字符串默认不加上引号

“ ”：双引号表示：不会转义字符里面的特殊符号；特殊符号会作为本身想表示的意思：

​     	name：“zhangsan \n lisi"     =>zhangsan  换行 lisi

‘ ’：单引号：会转义特殊字符，特殊字符最终只是一个普通的字符串数据

​     	name：“zhangsan \n lisi"     =>zhangsan  \n lisi

##### 对象、Map（属性和值）（键值对）

​	k: v  对象

```yml
friends:
	lastName: zhangsan 
	age: 20
```

行内写法：

```
firends: {lastName:zhangsan,age:18}
```

数组(List、Set)

用- 值表示数组中的一个元素

```
pets:
 - cat
 - dog
 - pig
```

行内写法：

```
pets: [cat,dog,pig]
```

#### 例子：

1.导入配置文件处理器configuration-processor；

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
```

2.在类文件前面引入注解：

```java
@Component
@ConfigurationProperties(prefix = "person" ) //加载全局组件中的指定部分
public class Person {
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birth;
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
    。。。
    }
```

3. yml文件

```ym
person:
  lastName: zhangsan
  age: 18
  boss: false
  birth: 2018/10/24
  maps: {k1: v1,k2: v2}
  lists:
    - lisi
    - zhaoliu
  dog:
      name: 小狗
      age: 3
```

使用@Value("${person.lastname}") 注解形式：

```
@Component
public class Person {
	@Value("${person.lastname}") //一个一个注入
    private String lastName;
    ...
    private Integer age;
    private Boolean boss;
    。。。
    }
```

|                      | @ConfigurationProperties |    @Value    |
| :------------------- | :----------------------: | :----------: |
| 功能                 | 批量注入配置文件中的属性 | 一个一个指定 |
| 松散绑定（松散语法） |           支持           |    不支持    |
| SpEL                 |          不支持          |     支持     |
| JSR303数据校验       |           支持           |    不支持    |
| 复杂类型封装         |           支持           |    不支持    |

松散绑定：last-name =》lastName

在类前@Validated  ，具体属性前@Email 表示以email格式；



### @PropertySource（value={“classpath:person.properties”}）

可以用来加载指定的配置文件

### @ImportResource（locations={“classpath:beans.xml”}）

导入Spring的配置文件（xml），让配置文件里面的内容生效；



**SpringBoot推荐的给容器中添加组件的方式：全注解的方式。**替代方式：

```java
@Configuration   //替代之前用xml=><bean>的方式
public class MyAppConfig {
    @Bean  //将方法的返回值添加到容器中：容器中这个组件默认的id就是方法名
    public HelloService helloService(){
    	System.out.println("配置类@Bean给容器中添加组件了")；
        return new HelloService();
    }
}
```

### 配置文件占位符

可以在配置文件中使用随机数

```
${random.value}
${random.int}
${random.long}
${random.int(10)}
${random.int[1024,65536]}
```

**可以在配置文件中引用前面配置过的属性**，还有可以用：指定默认值。如${app.name:默认值}

```
app.name=MyApp
app.description=${app.name:lisi} is a SpringBoot Application
```

### Profile

对不同环境提供不同配置功能的支持，可以通过激活、指定参数等方式快速切换环境；

1、多Profile文件

在主文件编写的时候，文件名可以是application-{profile}.properties/yml

例如：

application-dev.properties形式命名开发环境的配置文件

在主配置文件applicaiton.properties里面设置,进行激活配置：

```
spring.profiles.active=dev
```

2、yml文件支持多文档块儿

```yml
server: 
	port: 8080
spring: 
	profiles:
    	active: dev
---
spring: 
	profiles:dev
server:
	port:8081
---
spring: 
	profiles:prod
server:
	port:8082
---

```

### 配置文件记载位置

- file: ./config
- file: ./          //当前项目根目录
- classpath:/config
- classpath:/      //创建项目的resource目录

以上优先级从高到底

### 外部文件加载配置

1.命令行参数

打包后运行时，在命令行指定参数：

```powershell
java -jar spring-boot-hello.jar --server.port=8087 --server.context-path=/abc
```

这样一个一个输入太繁琐

2.在外部建立配置文件

在打包好的jar包外面放置配置文件

## 日志框架

市面上常见的日志框架：

JUL、JCL、Jboss-loggin、logback、log4j、log4j2、slf4j。。。

日志门面：JCL **SLF4J** Jboss-loggin

日志实现：Log4j    JUL   Log4j2  **Logback**

**SpringBoot选用的SLF4J和Logback**

```
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {
  public static void main(String[] args) {
  //记录器
    Logger logger = LoggerFactory.getLogger(HelloWorld.class);
    logger.info("Hello World");
  }
}
```

```
//日志级别trace《debug《info《warn《error  默认info级别
logger.trace("")
logger.debug("")
logger.info("")
logger.warn("")
logger.error("")
```

配置文件里：

```
logging.level.com.coska=trace   //指定某个区域内的日志级别
loggin.path=/spring/log     //当前磁盘根路径,文件名默认
loggin.file=G:/Springboot.log  //指定保存到文件，如果设置了loggin.path就不用设置这一项
//指定输出格式
loggin.pattern.console=%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-Slevel %logger{50} -%msg%n
```



## Spring Boot与web

1、创建SpringInitializer，选相应模块,如:MyBatis(SQL),Redis(NoSQL),Web等

2、Spring默认将这些场景配置好了，只需要在配置文件中指定少量配置就可以运行起来

3、自己编写业务代码

### SpringBoot对静态资源的映射规则

```
@ConfigurationProperties(prefix="spring.resoureces",ignoreUnknownFields=false)
public class ResourceProperties implements ResourceLoaderAware{
//可以设置和静态资源有关的参数、缓存时间等
```

在**WebMvcAutoConfiguration.java**里面可以看到：

1）、所有/webjars/**请求,都去classpath：/META-INF/resources/webjars找资源；

webjars形式引入https://www.webjars.org/；

请求：localhost：8080/webjars/jquery/3.3.1/jquery.js

2）、"/**"访问当前项目的任何资源，

```
"classpath:/META-INF/resources/"
"classpath:/resources/"
"classpath:/static/"
"classpath:/public/"
```

3）、欢迎页设置；静态资源文件夹下所有index.html页面

4）、所有的**/favicon.ico图标都是在静态资源文件下找；

模板引擎：

SpringBoot推荐的Thymeleaf；

1、引入thymeleaf；

```
<dependency>
  <groupId>org.thymeleaf</groupId>
  <artifactId>thymeleaf-spring4</artifactId>
  <version>3.0.0.RELEASE</version>
</dependency>
```

在spring-boot-autoconfigure里面有自动配置规则：

```
@ConfigurationProperties(
    prefix = "spring.thymeleaf"
)
public class ThymeleafProperties {
    private static final Charset DEFAULT_ENCODING;
    public static final String DEFAULT_PREFIX = "classpath:/templates/";  //默认前置
    public static final String DEFAULT_SUFFIX = ".html";   //默认后缀
    private boolean checkTemplate = true;
    private boolean checkTemplateLocation = true;
    private String prefix = "classpath:/templates/";
    private String suffix = ".html";
    private String mode = "HTML";
```

只要我们把HTML页面放在classpath:/templates/，thymeleaf就能自动渲染；

```
Simple expressions:
    Variable Expressions: ${...}
    Selection Variable Expressions: *{...}
    Message Expressions: #{...}
    Link URL Expressions: @{...}
    Fragment Expressions: ~{...}
Literals
Text liter
```

1.导入命名空间

```
<html lang="en" xmlns:th="http://www.thymeleaf.org">
```

2.配置路由，传进去参数

```
    @RequestMapping("/success")
    public String success(Map<String,Object> map) {
        map.put("hello","你好");
        map.put("users",Arrays.asList("zhangsan","lisi","wangwu"));
        return "success";
    }
```

3.html中

```
<body>
    <h1>成功！</h1>
    <div th:text="${hello}"></div>

    <h4 th:text="${user}" th:each="user:${users}"></h4>
</body>
```





### 请求拦截

```
@RequestMapping(value="/user/login",method=RquestMethod.POST)
@PostMapping(value="/user/login")
public String login(@RequestParam("username") String username,
					@RequestParam("password") String password){
                        if(!StringUtils.isEmpty(username)&&"123456".equals(password)){
                        	//登陆成功
                            return "dashborad"
                        }else{
                        	//登录失败
                            return "login"
                        }
					}


@PutMapping
@GetMapping

```

### SpringData

JDBC MyBatis SpringData JPA

