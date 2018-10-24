Spring基础：



示例：

一般java写hello world,定义对象，再在主函数内引入对象创建实例，调用实例方法：

```
public class HelloWorld {   //定义Helloworld对象
    private String name;
    public void setName(String name) {
        this.name = name;
    }
    public void hello(){
        System.out.println("hello:"+name);
    }
}
```

```
public class main {    //引用并创建实例
    public static void main(String[] args) {
        HelloWorld helloWorld = new HelloWorld();
        helloWorld.setName("coska");
        helloWorld.hello();
    }
}
```



采用spring的形式：

```java
public class HelloWorld {   //定义Helloworld对象
    private String name;
    public void setName(String name) {
        this.name = name;
    }
    public void hello(){
        System.out.println("hello:"+name);
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>  //配置applicationContext.xml的bean文件
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="helloWorld" class="com.coska.spring.beans.HelloWorld">
        <property name="name" value="Spring"></property>
    </bean>
</beans>
```

```java
package com.coska.spring.beans;    //main.java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class main {
    public static void main(String[] args) {
//        1.创建Spring的IOC容器对象
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
//        2.从IOC容器中获取Bean实例
        HelloWorld helloWorld =(HelloWorld) ctx.getBean("helloWorld");
//        3.调用hello方法
        helloWorld.hello();
    }
}
```

采用spring在创建IOC容器对象时，容器对象会自动加载HelloWorld的构造方法，并且自动调用set方设置属性；

- 分离接口与实现
- 采用工厂设计模式
- 采用反转控制

## IOC反转控制

反转资源获取的方向，容器主动把资源推送给它所管理的组件，组件所要做的仅仅是选择一种合适的方式来接受资源；

## 配置Bean

1.通过Class：bean的全类名，通过反射的方式在IOC容器中创建Bean，所以**要求Bean中必须有无参数的构造器**；通过id来获取容器中的bean，id唯一；

```
HelloWorld helloWorld =(HelloWorld) ctx.getBean("helloWorld");
```

2.ApplicationContext是BeanFactory的子接口，ApplicationContext面向使用Spring框架的开发者，几乎所有的应用场合都直接使用ApplicationContext而非底层的BeanFactory；

主要实现类：

ClassPathXmlApplicationContext，从类的路径下加载配置文件：

```
 ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
```

FileSystemXmlApplicationContext, 从文件系统中加载配置文件：

```

```

**ApplicationContext在初始化上下文时就实例化多有的单例的Bean。**

WebApplicationContext是专门为WEB应用而准备的；



### Spring支持3种依赖注入方式：

- 属性注入
- 构造器注入
- 工厂方法注入（很少使用，不推荐）

#### 1.属性注入(最常用方法)：

通过setter方法注入Bean的属性值或依赖的对象

(该方法在构建对象时只能用setter方法，**不能用带参的constructor形式**；)

```xml
    <bean id="helloWorld" class="com.coska.spring.beans.HelloWorld">
        <property name="name" value="Spring"></property>
    </bean>
```

ok：

```java
public class Person {
    private String name;
    private int age;
    private Car car;

    public void setName(String name) {
        this.name = name;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public void setCar(Car car) {
        this.car = car;
    }
}
```

不ok：

```java
public class Person {
    private String name;
    private int age;
    private Car car;

    public Person(String name, int age, Car car) { //使用属性注入的方式不能使用带参构造器
        this.name = name;
        this.age = age;
        this.car = car;
    }

    @Override
    public String toString() {
        return "Person [name="+name+",age="+age+",car="+car+"]";
    }
}
```

#### 2. 通过构造方法来配置bean属性：

可以指定参数的位置和参数的类型！以区分重载的构造器。

```java
public class Car
{
    private String brand;
    private String corp;
    private double price;
    private int maxSpeed;
    public Car(String brand, String corp, double price) {
        this.brand = brand;
        this.corp = corp;
        this.price = price;
    }
    public Car(String brand, String corp, int maxSpeed) {
        this.brand = brand;
        this.corp = corp;
        this.maxSpeed = maxSpeed;
    }
}
```

```
    <bean id="car" class="com.coska.spring.beans.Car">
        <constructor-arg value="Audi" index="0"></constructor-arg>
        <constructor-arg value="dasauto" index="1"></constructor-arg>
        <constructor-arg value="300000" type="double"></constructor-arg>
    </bean>
    <bean id="car2" class="com.coska.spring.beans.Car">
        <constructor-arg value="Audi" index="0"></constructor-arg>
        <constructor-arg value="dasauto" index="1"></constructor-arg>
        <constructor-arg value="30" type="int"></constructor-arg>
    </bean>
```

特殊字符可以用  <![CDATA[XXX]]> 包裹起来

#### 3.通过ref建立bean之间的引用关系

- 属性注入方式：

```java
public class Person {
    private String name;
    private int age;
    private Car car;
    public void setName(String name) {
        this.name = name;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public void setCar(Car car) {
        this.car = car;
    }
    @Override
    public String toString() {
        return "Person [name="+name+",age="+age+",car="+car+"]";
    }
}
```

在bean里：

```xml
   <bean id="car2" class="com.coska.spring.beans.Car">
        <constructor-arg value="Audi" index="0"></constructor-arg>
        <constructor-arg value="dasauto" index="1"></constructor-arg>
        <constructor-arg value="30" type="int"></constructor-arg>
    </bean>
    <bean id="person" class="com.coska.spring.beans.Person">
        <property name="name" value="Tom"></property>
        <property name="age" value="18"></property>
        <property name="car" ref="car2"></property>  //通过ref属性建立bean之间的引用
        //另一种写法<property name="car"><ref bean="car2"/></property>
    </bean>
```

属性注入方式，内部bean写法（内部bean只能在内部使用，外部无法引用）：

```xml
<property name="car">
    <bean  class="com.coska.spring.beans.Car">  //只在内部使用id可以不写；
        <constructor-arg value="Audi" index="0"></constructor-arg>
        <constructor-arg value="dasauto" index="1"></constructor-arg>
        <constructor-arg value="30" type="int"></constructor-arg>
    </bean>
</property>
```

- 构造器方式：

```java
package com.coska.spring.beans;

public class Person {
    private String name;
    private int age;
    private Car car;
    
    public Person(String name, int age, Car car) {
        this.name = name;
        this.age = age;
        this.car = car;
    }
    @Override
    public String toString() {
        return "Person [name="+name+",age="+age+",car="+car+"]";
    }
}
```

```xml
    <bean id="person" class="com.coska.spring.beans.Person">
        <constructor-arg  value="Tom"></constructor-arg>
        <constructor-arg  value="18"></constructor-arg>
        <constructor-arg  ref="car2"></constructor-arg>
    </bean>
```



##### null值和级联属性

```xml
<constructor-arg><null/></constructor-arg> //测试时赋空值
```

```xml
    <bean id="person" class="com.coska.spring.beans.Person">
        <constructor-arg  value="Tom"></constructor-arg>
        <constructor-arg  value="18"></constructor-arg>
        <constructor-arg  ref="car2"></constructor-arg>
        <property name="car2.price" value="30000"></property>  //级联属性方式赋值？？？？
        //妈蛋？？？我照着敲不行？？？
    </bean>
```

#### 集合属性：

通过一组内置的xml标签（例如：<list>，<set>或<map>）来配置集合属性。

##### 对java.util.list类型：

通过<value>指定简单的常量值，<ref>指定Bean的引用，<bean>内置Bean，<null/>空

数组和set的定义和list一样，都使用<list>

一个人多辆车:

```
public class Person {
    private String name;
    private int age;
    private List<Car> cars;
    public void setName(String name) {
        this.name = name;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public void setCars(List<Car> cars) {
        this.cars = cars;
    }
    @Override
    public String toString() {
        return "Person [name="+name+",age="+age+",cars="+cars+"]";
    }
}
```

```
    <bean id="person3" class="com.coska.spring.beans.Person">
    <property name="name" value="Tom"></property>
    <property name="age" value="18"></property>
    <property name="cars" >
        <list>
            <ref bean="car"/>
            <ref bean="car2"/>
        </list>
    </property>
    </bean>
```

##### 对于map类型：

Java.util.Map通过<map>标签定义，<map>标签里可以使用多个<entry>作为子标签，每个条目包含一个键和一个值；

必须在<key>标签里定义键



示例：

```xml
    <bean id="mapPerson" class="com.coska.spring.beans.MapPerson">
        <property name="name" value="Rose"></property>
        <property name="age" value="18"></property>
        <property name="cars" >
            <map>
                <entry key="AA" value-ref="car"></entry>
                <entry key="BB" value-ref="car2"></entry>
            </map>
        </property>
    </bean>
```

```java
package com.coska.spring.beans;
import java.util.Map;
public class MapPerson {
    private String name;
    private int age;
    private Map<String, Car> cars;
    public void setName(String name) {
        this.name = name;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public void setCars(Map<String, Car> cars) {
        this.cars = cars;
    }
    @Override
    public String toString() {
        return "Person [name="+name+",age="+age+",cars="+cars+"]";
    }
}

```

Java.util.Properties示例(使用<prope>标签)：

```java
package com.coska.spring.beans;
import jdk.nashorn.internal.objects.annotations.Property;
import java.util.Properties;
public class DataSource {
    private Properties properties;
    public Properties getProperties() {
        return properties;
    }
    public void setProperties(Properties properties) {
        this.properties = properties;
    }
    @Override
    public String toString() {
        return "DataSource [properties=]"+properties+"]";
    }
}
```

```xml
    <bean id="dataSource" class="com.coska.spring.beans.DataSource">
        <property name="properties">
            <props>
                <prop key="user">root</prop>
                <prop key="password">123456</prop>
                <prop key="jdbcUrl">jdbc:mysql:///test</prop>
                <prop key="driverClass">com.mysql.jdbc.Driver</prop>
            </props>
        </property>
    </bean>
```

使用  < utils:list> 标签：

```xml
    <util:list id="cars">
        <ref bean="car" />
        <ref bean="car2" />
    </util:list>
    <bean id="person4" class="com.coska.spring.beans.Person">
        <property name="name" value="李四"></property>
        <property name="age" value="18"></property>
        <property name="cars" ref="cars"></property>
    </bean>
```

p命名空间形式：

```
<bean id="person5" class="com.coska.spring.beans.Person" p:age="30" p:name="王5" p:cars-ref="cars" ></bean>
```

### 自动装配（实际开发少用）

 通过名字：autowire="byName"; 通过类型：autowire="byType"

```xml
<bean id="address" class="xxxx" p:city="beijin" p:street="huilongguan"></bean>
<bean id="car" class="xxxx" p:brand="Audi" p:price="300000"></bena>
<bean id="person" class="xxx" p:name="zhangsan" autowire="byName"></bean>
```

#### 配置的继承

父bean结尾加上^,子配置parent="address"；

```xml
<bean id="address" class="xxx" p:city="Beijin^" p:street="WuDaoKou"></bean>
<bean id="address2" p:street="DaZhongSi" parent="address"></bean>
```

通过depends-on属性设定Bean的前置依赖；

#### bean作用域

使用bean的scope属性来配置bean的作用域

singleton：默认值，容器初始话时创建bean实例，在整个容器的生命周期内只创建这一个bean；

prototype：原型的，容器初始化时不创建bean的实例，而是在每次请求时创建一个新的Bean实例；

```
ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
Car car1=(Car) ctx.getBean("car");
Car car2=(Car) ctx.getBean("car");
//如果car的bean不是单例的，则car1不等于car2；
```

#### 使用外部属性文件

Spring提供了一个PropertyPlaceholderConfigurer的BeanFactory后置处理器，这个处理器允许用户将Bean配置文件的部分内容外移到**属性文件**中，可以在Bean配置文件里使用形式为**${var}**的变量；PropertyPlaceholderConfigurer从属性文件里加载属性，并使用这些属性来替换变量；

Spring还允许在属性文件中使用${propName},以实现属性之间的相互引用；

```properties
user=root
password=1230
jdbcurl=jdbc:mysql:///test
driverclass=com.mysql.jdbc.Driver
```

```
   <bean id="dataSource" class="com.coska.spring.beans.DataSource">
        <property name="properties">
            <props>
                <prop key="user">${user}</prop>
                <prop key="password">${password}</prop>
                <prop key="jdbcUrl">${jdbcurl}</prop>
                <prop key="driverClass">${driverclass}</prop>
            </props>
        </property>
    </bean>
```

#### SpEL

Spring表达式语言，支持运行时查询和操作对象图的强大的表达式语言。

使用#{...}作为定界符    如：`#{car.price>30000?"金领":"白领"}`

为bean属性动态赋值

逻辑：and or not |

if-else：XXX?(XXX):(XXX)

正则表达式：matches

调用静态属性或方法：T();如`T(java.lang.Math).PI*80`

#### Bean的生命周期：

xml里配置：init-method和destroy-method这两个方法：

```java
    public void init(){   //这个名字随便取的，只是为了在xml里指定init-method和destory-method
        System.out.println("初始化。。。。");
    }
    public void destory(){
        System.out.println("销毁。。。。");
    }
```

``` xml
    <bean id="car" class="com.coska.spring.beans.Car" 
    init-method="init" destroy-method="destory">
        <constructor-arg value="Audi" index="0"></constructor-arg>
        <constructor-arg value="dasauto" index="1"></constructor-arg>
        <constructor-arg value="300000" type="double"></constructor-arg>
    </bean>
```

##### 创建Bean后置处理器

通过重写BeanPostProcessor的两个方法：（用这个甚至可以偷梁换柱。。。）

```java
import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;
public class MyBeanPostProcessor implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("BeforeInit..."+bean+","+beanName);
        return bean;
    }
    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("AfterInit..."+bean+","+beanName);
        return bean;
    }
}
```

```
<bean class="com.coska.spring.beans.MyBeanPostProcessor"></bean>
```

## 通过注解的方式配置Bean



Spring组件扫描:Spring能够从classpath下自动扫描，侦测和实例化具有特定注解的组件；

- @Component:基本注解，标识了一个受Spring管理的组件
- @Service("user")：标识服务层（业务层）组件
- @Controller("user")：标识表现层组件
- @Repository("user")：标识持久层组件

其实也是可以混用，只是这样约束下清楚点；

```java
@Repository("userRepository")  //不传参会自动根据类型搜索，但是有两个的话就需要名字来指定哪个
public class UserRepositoryImpl implements UserRepository {
    @Override
    public void save() {
        System.out.println("UserRepositoryImpl save()...");
    }
}
```

```xml
<context:component-scan 
        base-package="com.spring.beans"
        resource-pattern="repostory/*.class">  //过滤扫描,只扫描子包
</context:component-scan>
```

```
<context:include-filter> 子节点表示要包含的目标类
<context:exclude-filter> 子节点标识要排除在外的目标类
```

##### @Autowired自动装配Bean组件

对于互相引用的组件：

```java
@Controller
public class UserController {
    @Autowired
    private UserService userService;
    public void execute(){
        System.out.println("UserController execute()...");
        userService.add();
    }
}
```

```java
@Autowired(required=false) //即使这个属性没装配也可以，自动设为空
@Qualifier("userRepositoryImpl") //装配指定的bean
@Autowired 应用在数组类型的属性上，此时Spring会把所有匹配的Bean进行自动装配
@Autowired 注解也可以应用在集合属性上，此时Spring读取该集合的类型信息，然后自动装配所有与之兼容的Bean.
@Autowired 注解用在Map上时，该Map的键值为Spring，那么Spring将自动装配与之Map值类型兼容的Bean.
```

### 泛型依赖注入：

Spring4.x中可以为子类注入对应的泛型类型的成员变量的引用。

```java
public class BaseService<T> {
    @Autowired
    protected BaseRepository<T> repository;
    public void add(){
        System.out.println("add...");
        System.out.println(repository);
    }
}
```

```java
public class BaseRepository<T> {
}
```

```java
@Service
public class UserService extends BaseService<User>{
}
```

```java
@Repository
public class UserRepository extends BaseRepository<User> {
}
```

```
<context:component-scan base-package="com.spring.beans"></context:component-scan>
```

```
public class Main {
    public static void main(String[] args) {
        ApplicationContext ctx=new ClassPathXmlApplicationContext("beans-generic.xml");
        UserService userService=(UserService)ctx.getBean("userService");
        userService.add();
    }
}
```

#### 面向切面编程  AOP

越来越多的非业务需求（日志和验证等）加入后，原有的业务方法急剧膨胀，每个方法在处理核心逻辑的同事还必须兼顾吉他的多个关注点。在多个模块里多次重复相同的日志代码，如果日志需求发生变化，必须修改所有模块。

动态代理方式解决：

```java
public class ArithmeticCalculatorLoggingProxy {
    private ArithmeticCalculator target;
    public ArithmeticCalculatorLoggingProxy(ArithmeticCalculator target){
        this.target=target;
    }
    public ArithmeticCalculator getLoggingProxy(){
        ArithmeticCalculator proxy = null;
        //代理对象由哪一个类加载器负责加载
        ClassLoader loader=target.getClass().getClassLoader();
        //代理对象的类型，即其中有哪些方法
        Class [] interfaces=new Class[]{ArithmeticCalculator.class};
        //当调用代理对象其中的方法时，该执行的代码
        InvocationHandler h=new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                String methodName = method.getName();
                //日志
                System.out.println("The method "+methodName+" begins with"+ Arrays.asList(args));
                //执行方法
                Object result =method.invoke(target,args);
                System.out.println("The method "+methodName+" ends with "+ result);
                return result;
            }
        };
        proxy =(ArithmeticCalculator) Proxy.newProxyInstance(loader,interfaces,h);
        return proxy;
    }
}
```

SpringAOP方式：

通知（Advice）、目标（Target）、代理（Proxy）、连接点（Jionpoint）、切点（pointcut）

AspectJ：java社区里最完整最流行的AOP框架。

```xml
        <context:component-scan base-package="com.spring.aop.calculator2"></context:component-scan>
        <aop:aspectj-autoproxy></aop:aspectj-autoproxy> //切面配置
```

切面首先加入到ioc容器中@Component，然后还需要加入@Aspect注解

```java
@Aspect
@Component
public class LoggingAspect {
    @Before("execution(public int com.spring.aop.calculator2.ArithmeticCalculator.add(int,int))") //把add换成*则匹配所有方法
    public void beforeMethod(JoinPoint joinPoint){
        String methodName=joinPoint.getSignature().getName(); //获取方法名称
        List<Object> args= Arrays.asList(joinPoint.getArgs()); //获取方法参数
        System.out.println("The method"+methodName+" begins ..."+args);
    }
}
```

```
@Before:前置通知
@After:后置通知  在目标方法后执行（无论是否发生异常）
@AfterRunning:在方法返回结果之后执行
@AfterThrowing：异常通知，在方法抛出异常之后
@Around:环绕通知，围绕着方法执行；
```

用@order（int）指定优先级，值越小，优先级越高

@order(1)   @order(2)

重用切点表达式：

```
    @Pointcut("execution(public int com.spring.aop.calculator2.ArithmeticCalculator.*(int,int))")
    public void declareJointPointExpression(){}
```













使用注解方式配置：

- 1.为主配置文件引入新的命名空间（约束）
- 2.开启使用注解代理配置文件
- 3.在类中使用注解完成配置



修改对象作用范围：

@Scope(scopeName="prototype")

@Value(value="tom")

对象的注解：

@Autowired //自动装配

@Qualifier("car2")//指定装配对象的名字

@Resource(name="car")  //手动注入

```
@PostConstruct
public void init(){
    System.out.println("我是初始化方法！")
}

@PreDestroy //在销毁前调用
public void destory(){
    System.out.println("我是销毁方法！")
}
```



核心容器

Beans 

Core 

Context 

spEL















