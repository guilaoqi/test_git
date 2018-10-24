# java基础



```java
class HelloWorld {
    public static void main(String[] args) {
        System.out.println("你好");
    }
}
```



## 标识符

- 不能以数字开头

- 不能是关键字

- 区分大小写

  $ _ 是合法，#等不合法，不能有空格 .；

包：驼峰命名；

## 常量

-字面值常量、自定义常量

字符串常量、整数常量、小数常量、字符常量、布尔常量、空常量

原反补：(反+补=0)

#### 原：

第一位 0 表示正值，1表示负值；

#### 反：

正数相同，负数取反；

#### 补：

正相同，负在反码末位加1;

## 变量

#### 整数

byte（一个字节），short（两个字节），int （四个字节），**long（八个字节 long x=8888888L）**;

#### 浮点

小数默认double(八个字节)，**float( 四个字节float f = 12.3F)**

#### 字符

字符占两个字节

#### 数据类型转换

小转大不报错（例如：byte=》int 不报错，char转int，整数=》浮点，其他转dobule）

强制转换：`b=(byte)(x+b);`

```
- byte b1 = 3;
- byte b2 = 4;
- byte b3 = b1 + b2;
//byte类型的变量在进行运算的时候,会自动类型提升为int类型
```

#### 引用数据类型：

- 类（class）
- 接口（interface）
- 数组
- 枚举（enum）
- 注解（annotation）

## 方法

```java
修饰符 返回值类型 方法名（[ 参数类型 参数名1，参数类型 参数名2，。。。]）{
	执行语句
	。。。
	return 返回值；
}

public static int getArea（int x , int y）{
    int temp=x*y;
    return temp;
}
```

## 数组

```
int[] x= new int[100];
int[] x;
x=new int[100];
```

数组默认值：

- byte，short，int，long 0、
- float，dobule 0.0
- char `\u0000`
- boolean flase
- 引用 null

多维数组：

```
int[][] arr=new int[3][4];
arr[0]=new int[]{1,2,3,4};
```

## 类

#### 构造方法（类初始化的时候调用）：

- 方法名与类名相同
- 方法前面没有返回值类型的声明
- 方法中不能使用return语句返回一个值

**一个类中可以定义多个构造方法，只要每个方法的参数类型或参数个数不同即可；**

#### 垃圾回收

**System.gc();**

**finalize()方法**会在垃圾回收前被调用；

例子：

```java
class Person {
    public void finalize(){
        System.out.println("对象将要被回收");
    }
}
public class Example {
    public static void main(String[] args) {
        Person p1=new Person();
        Person p1=new Person();
        p1=null;
        p2=null;
        System.gc();  //调用方法进行垃圾回收，对空对象会回收
        for(int i=0;i<100000000000;i++){
            //为了延时。。。
        }
    }
}
```

#### Static 关键字

- 静态变量

static关键是声明的变量是静态变量，静态变量被所有实例共享；

- 静态方法

类本身的方法

```
class Person {
    public static void sayHello() {
        System.out.println("hello");
    }
}
public class Example {
    public static void main(String[] args) {
       Person.sayHello(); //直接用类.方法名 调用
        }
    }
}
```

- 静态代码块

当类被加载时，静态代码块会执行，由于类只加载一次，静态代码块只加载一次；

#### 单例模式

- 类的构造方法使用private修饰，声明为私有；这样就不能在类的外部使用new关键字创建实例对象了。

- 在内部创建一个该类的实例对象，并使用静态变量INSTANCE引用该对象；

- 为了让外部获得该变量，创建一个静态方法getInstance()用于返回该类的实例INSTANCE；

  ```java
  class Single {
      private static Single INSTANCE = new Single();
      private Single(){};
      public static Single getInstance(){
          return INSTANCE;
      }
  }
  ```

#### 内部类

在一个类的内部定义类

访问方式：`外部类名.内部类名 变量名=new 外部类名（）.new内部类名（）；`

**静态内部类**

```java
class Outer {
    private static int num=6;
    static class Inner {   //类内部静态类
        void show(){
            System.out.println("num="+num);
        }
    }
}

class Example {
    public static void main(String[] args) {
        Outer.Inner inner = new Outer.Inner(); // 用new 外部类.内部类（） 创建
        inner.show();
    }
}
```

#### 类的继承

- java中，类只支持单继承，不允许多重继承；

重写父类方法；

super关键字：

```java
class Animal {
    String name="动物";
    void shout(){
        System.out.println('动物发出声音')；
    }
}
class Dog extends Animal {   //定义Dog类继承动物类
    String name=“犬类"；
    void shout(){
        super.shout();//用super关键字调用父类的成员方法；
    }
    void printName(){
        System.out.println("name="+super.name);//用super关键字访问父类的成员变量；
    }
}
//测试：
public class Example {
    public static void main（String[] args）{
        
    }
}
```

#### final关键字

被final 关键字修饰的类不能被继承；

被final关键字修饰的方法将不能被子类重写；

被final关键字修饰的变量将变为常量，值不能再改变；

#### 抽象类和接口

抽象方法：定义方法时不写方法体；

**当一个类包含了抽象方法，该类必须使用abstract关键字修饰，该类为抽象类**。

```java
abstract class Animal {
    abstract int shout();
}
```

如果一个抽象类的所有方法都是抽象的，则可以用interface关键字来声明

```java
interface Animal {
    int ID=1;
    void breathe();
    void run();
}
```

- 接口中的方法都是抽象的，**不能实例化对象**；
- **抽象类的子类必须覆盖所有的抽象方法后才能被实例化，否则这个子类还是个抽象类**
- 当一个类实现接口时，如果这个类是抽象类，则实现接口中 的部分方法即可，否则需要实现接口中的所有方法；
- 一个类通过implements关键字实现接口时，可以实现多个接口，被实现的多个接口之间用，隔开；

```
interface Run {
    ...
}
interface Fly {
    ...
}
class Bird implements Run,Fly {
    ...
}
```

#### 多态

对实例强制类型转换：

`Cat cat=(Cat)animal ` 

instanceof关键字，判断一个对象是否为*某个类（或接口）的实例或子类实例*；

`对象 instanceof 类（或接口）`

#### 异常

```java
try {
    
} catch(Exception e) {
    System.out.println(e.getMessage());
}

修饰符 返回值类型 方法名（[参数1，参数2...]） throws ExceptionType1[,ExceptionType2...]{
    
}
```

## 包

专门用来存放类，包的声明只能放在java源文件的第一行；

```java
package cn.itcast;  // 用package关键字声明包，目录为cn/itcast
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

编译时：`javac -d D:\cn\itcast\HelloWorld.java`

运行：`java cn.itcast.HelloWorld`

#### import 包的引用(import 包名.类名)

`import cn.itcast.Student;`

JDK中，不同的功能类放在不同的包中,常用包：

- java.lang ：核心类，如：String，Math，System和T和Thread类，系统会自动导入；
- java.util: 工具类，集合类；
- java.net: 网络编程相关接口；
- java.io: 输入输出接口；
- java.awt: 构建GUI界面相关接口；

#### 打包

把cn目录下的全部内容生成一个helloworld.jar文件

`jar -cvf helloworld.jar cn`

-c 代表创建归档文件

-v 代表在标准输出中生成详细输出。

-f 代表指定归档文件名

运行：

`java -jar helloworld.jar`



## API



### 1.1 String类

`String str1='abc';`

常用方法：

```
indexOf(字符) lastIndexOf(字符) charAt(索引) endsAt(字符串) length() equals()
isEmpty() startsWith(字符串) contains() toLowerCase() toUpperCase() valueOf()//int转字符串 
toCharArray() replace(oldstr,newstr) split() sustring(beginIndex,endIndex) trim()
```

### 1.2 StringBuffer 类

内容长度可变，对其进行进行添加或删除字符时，并不会产生新的StringBuffer对象；针对StringBuffer有一系列方法；

```
append(char); //尾部插入字符
isnert(int,String); //指定位置插入字符串
deleteCharAt(int);//移除次序列指定位置的字符
delete（start，end）；//删除指定范围
replace（start，end，String）；//替换指定范围
setCharAt（index，char）；//修改指定位置字符
toString（）；//缓冲区中的字符串
reverse（）；//反转替代
```

### 2.1 System类

```
getProperties()
currentTimeMillis() //返回毫秒时间戳
arraycopy(源数组，源数组起始位置，目标数组，目标起始位，拷贝长度)
```

### 2.2 Runtime类（虚拟机运行时的状态）

```java
Runtime run=Runtime.getRuntime();
run.availableProcessors(); // 处理器个数
run.freeMemory(); //空闲内存（字节为单位）
maxMemory(); //最大可用内存（字节为单位）
```

```
public class Example {
    public static void main(String[] args) throws Exception {
        Runtime rt = Runtime.getRuntime();
        Process process = rt.exec("notpad.exe"); //执行notpad.exe并得到process对象
        Thread.sleep(3000); //延时
        process.destroy();  //杀进程
    }
}
```

### 2.3 Math类和Random类

```
Math.abs(-1);    1
Math.ceil(5.6);   6.0
Math.floor(-4.2);  -5.0
Math.round(-4.6);  -5  //四舍五入
Math.max(2.1,-2.1);
Math.min(2.1,-2.1);
Math.random();大于等于0小于1的随机数

Random r1=new Random( seed );
r1.nextFloat();
r1.nextInt( int n);生成0-n的随机整数；
r1.nextDouble();
r1.nextBoolean();
r1.nextLong();
```

### 2.4 包装类

基本数据类型对应的包装类：（**基本数据类型不能直接调用系统类的方法，先包装成对应的包装类**）

```
byte===》Byte
char===》Character
int===》Integer
short===》Short
long===》Long
float===》Float
double===》Double
boolean===》Boolean
```

例：

```
public class Example {
    public static void main(String[] args) {
        int a=20;
        Integer in=new Interger(a);
        System.out.println(a.toString()); //调用了Object类的toString()方法
    }
}
```

Integer基本方法：

```
toBinaryString(); //转二进制，字符串
toHexString（）；//转16进制，字符串
toOctalString（）；//转8进制，字符串
ValueOf(int i); //将int转为Interger
valueOf(String s); //将String转为Integer
parseInt（string); // 字符串转成十进制整数
intValue(); //以int类型返回（拆箱）
```

例：

```java
public class Example {
    public static void main(String[] args) {
        int w=Integer.parseInt(args[0]); //接受传进来的参数并转化为int类型
        int h=Integer.parseint(args[1]); //接受传进来的参数并转化为int类型
        for(int i=0;i<h;i++){
            StringBuffer sb=new StringBuffer();
            for(int j=0;j<w;j++){
                sb.append("*");
            }
            System.out.println(sb.toString());
        }
    }
}
```

```
Integer i=Integer.valueOf("123") //合法
Integer i=Integer。valueOf("12a") //不合法
```

##### JDK5.0以后自动拆箱装箱

```
int num=20;
Integer number=num;   //自动装箱 new Integer(num)
```

```
Integer num=new Integer(18);
int number=num;  //自动拆箱 num.intValue();
```

### 2.5 Date类、Calendar类、DateFormat类

```
Date date1=new Date(); //当前时间的Date对象
Date date2=new Date(966666666666L); //创建时间戳的Date对象
```

```
Calendar类是一个抽象类，不能实例化，需要调用其静态方法getInstance()获取对象;
Calendar calendar= Calendar.getInstance();
int year=calendar.get(Calendar.YEAR); //获取年
int month=calendar.get(Calendar.MONTH)+1; //获取月
int date=calendar.get(Calendar.DATE);  //获取日
...
calender.set(2008,7,8);//设置
calender.add(Calender.DATE,100);//加
```

```
DateFormat 格式化日期，抽象类，不能实例化，直接访问静态方法获取对象
DateFormat.getDateInstance();//创建默认风格日期格式
DateFormat.getDateInstance(int Style);//创建指定风格日期格式
DateFormat.getDateTimeInstance();//创建默认风格日期/时间格式
DateFormat.getDateTimeInstance(int dateStyle，int timeStyle);//创建指定风格日期/时间格式
DateFormat.format();//将Date对象解析成指定字符串格式；
DateFormat.parse(); //将对应格式字符串解析成Date对象；
```

## 集合类

java中为了保存数目不确定的对象，JDK提供了一系列特殊的类，可以存储任意类型对象，并且长度可变，统称为集合；这些类都位于java.util包中； 

### collection接口

所有单列集合的父接口，有两个重要子接口List（有序可重复）和Set（无序不可重复）；

```
add(Object o)
addAll(Collection c)
clear()
remove()
removeAll()
isEmpty()
contains()
containsAll()
iterator()
size()
```

#### List接口

有序可重复

```
add( int index,Object element)
addAll(int index,Collection c)
get(int index)
remove(int index)
set(int index,Object element) //替换指定位置对象，返回值为该对象
indexOf（Object o）
lastIndexOf(Object o)
subList(int fomIndex, int toIndex)

```

##### ArrayList

List接口的一个实现类，内部封装了一个长度可变的数组对象；
```
import java.util.*;
public class Example {
    public static void main(String[] args) {
        ArrayList list=new ArrayList();
        list.add("stu1");
        list.add("stu2");
        list.add("stu3");
        list.add("stu4");
        System.out.println("集合的长度：“+list.size());
        System.out.println("第二个元素是："+list.get(1));
    }
}
```
##### LinkdList集合

快速增删，内部维护了一个双向循环链表，每一个元素使用引用方式记住前一个和后一个元素；

##### Iterator接口

主要用于访问（遍历）Collection中的元素
```java
import java.util.*;
public class example {
    public static void main(String[] args) {
        ArrayList list=new ArrayList();
        list.add("add_1");
        list.add("add_2");
        list.add("add_3");
        list.add("add_4");
        list.add("add_5");
        Iterator it = list.iterator();
        while(it.hasNext()) {  //判断是否存在下一个
            Object obj=it.next();
            System.out.println(obj);
        }
    }
}
```

##### foreach循环
注意，这种写法可读但是不能对元素进行修改。

```java
import java.util.*;
public class example {
    public static void main(String[] args) {
        ArrayList list=new ArrayList();
        list.add("add_1");
        list.add("add_2");
        list.add("add_3");
        list.add("add_4");
        for(Object obj:list) { //使用for（容器中元素的类型 临时变量：容器变量）
            System.out.println(obj);
        }
    }
}
```

##### ListIterator 接口
##### Enumeration 接口

#### set接口
无序不可重复，主要有两个实现类HashSet和TreeSet
- HashSet根据对象的哈希值来确定元素在集合中的存储位置；
- TreeSet根据二叉树的方式来存储元素；
##### HashSet集合
为了保证不重复，在存入时，先根据对象的哈希值（调用hashCode（）方法）计算出一个存储位置，如果已经有了该哈希值，则调用equals（）方法进行比较，如果不同则存入；

**对于Sting这样的类，String类中已经重写了hashCode（）和equals（）方法；对于自定义的类，需要自己重写hashCode（）和equals（）方法**
例：
```java
import java.util.*;
class Student {
    private String id;
    private String name;
    public Student(String id, String name){
        this.id=id;
        this.name=name;
    }
    public String toString() {
        return id+":"+name;
    }
    public int hashCode() {
        return id.hashCode();
    }
    public boolean equals(Object obj){
        if(this==obj){
            return true;
        }
        if(!(obj instanceof Student)) {
            return false;
        }
        Student stu=(Student) obj;
        boolean b= this.id.equals(stu.id);
        return b;
    }

}
public class HelloWorld {
    public static void main(String[] args) {
        HashSet hs =new HashSet();
        Student stu1=new Student("1","jack");
        Student stu2=new Student("2","Rose");
        Student stu3=new Student("3","haha");
        Student stu4=new Student("1","jack");
        hs.add(stu1);
        hs.add(stu2);
        hs.add(stu3);
        hs.add(stu4);
        System.out.println(hs);

    }
}

```

##### TreeSet集合
存入一个新的元素时，会将该元素与其他元素进行比较，最后将它插入到有序的对象序列中；
集合在进行比较时会调用compareTo（）方法，该方法在Comparable接口中定义，JDK大部分类都实现了Comparable接口如Integer，Double，String
**自定义的类，需要实现Comparable接口,并重写compareTo方法**：
```
import java.util.*;
class Student implements Comparable {
    private int age;
    private String name;
    public Student(String name, int age){
        this.age=age;
        this.name=name;
    }
    public String toString() {
        return name+":"+age;
    }
    public int compareTo(Object obj) {
        Student s=(Student) obj;
        if(this.age - s.age>0){
            return 1;
        }
        if(this.age - s.age==0) {
            return this.name.compareTo(s.name); 
            //由于Sting类实现了Comparable接口，直接调用；
        }
        return -1;
    }

}
public class HelloWorld {
    public static void main(String[] args) {
        TreeSet ts =new TreeSet();
        Student stu1=new Student("jack",19);
        Student stu2=new Student("Rose",18);
        Student stu3=new Student("haha",19);
        Student stu4=new Student("jack",19);
        ts.add(stu1);
        ts.add(stu2);
        ts.add(stu3);
        ts.add(stu4);
        Iterator it=ts.iterator();
        while(it.hasNext()){
            System.out.println(it.next());
        }
        System.out.println(ts);

    }
}
```



### MAP接口

双例集合，它的每一个元素都包含一个key和value；
```
put(Object key,Object value) //将指定值与此映射中的指定键关联
get(Object key) //返回指定键所映射的值
containsKey(Object key) //是否包含指定键
containsValue(Object value) //是否包含指定值
keySet() //返回包含的键的Set视图
values() //返回包含值的Collection视图
entrySet() //返回包含映射关系的set视图
```
Map接口最常用的实现类，HashMap和TreeMap

#### HashMap
**键相同，值覆盖；**
```
import java.util.*;
public class HelloWorld {
    public static void main(String[] args) {
        Map map=new HashMap();
        map.put("1","Jack");
        map.put("2","Rose");
        map.put("3","haha");
        map.put("3","lala");
        Set keySet=map.keySet();
        Iterator it=keySet.iterator();
        while (it.hasNext()) {
            Object key=it.next();
            Object value=map.get(key);
            System.out.println(key+":"+value);
        }

    }
}
...
1:Jack
2:Rose
3:lala
```

遍历hashMap的另一种方式
```
import java.util.*;
public class HelloWorld {
    public static void main(String[] args) {
        Map map=new HashMap();
        map.put("1","Jack");
        map.put("2","Rose");
        map.put("3","haha");
        map.put("3","lala");
        Set entrySet=map.entrySet();
        Iterator it=entrySet.iterator();
        while (it.hasNext()) {
            Map.Entry entry=(Map.Entry) (it.next());
            Object key = entry.getKey();
            Object value=entry.getValue();
            System.out.println(key+":"+value);
        }
    }
}
```
#### TreeMap
一样不允许有重复的键，通过二叉树保证键的唯一性；
在使用TreeMap时，也可以通过自定义比较器的方式对所有的键进行排序；
```
import java.util.*;
public class HelloWorld {
    public static void main(String[] args) {
        Map map=new TreeMap(new MyComparator()); //在构造TreeMap实例时传入
        map.put("1","Jack");
        map.put("2","Rose");
        map.put("3","haha");
        map.put("3","lala");
        Set entrySet=map.entrySet();
        Iterator it=entrySet.iterator();
        while (it.hasNext()) {
            Map.Entry entry=(Map.Entry) (it.next());
            Object key = entry.getKey();
            Object value=entry.getValue();
            System.out.println(key+":"+value);
        }
    }
}
class MyComparator implements Comparator {
    public int compare(Object obj1,Object obj2) {
        String id1=(String) obj1;
        String id2=(String) obj2;
        return id2.compareTo(id1);
    }
}
```

### 泛型
对象从集合中取出时，这个对象的编译类型会变成Objectle类型；
`ArrayList<参数化类型>list=new ArrayList<参数化类型>()`
```
import java.util.ArrayList;
public class HelloWorld {
    public static void main(String[] args) {
    	//使用泛型创建ArrayList集合
        ArrayList<String> list=new ArrayList<String> (); 
        list.add("String");
        list.add("Collection");
        for(String str : list) {
            System.out.println(str);
        }
    }
}
```
#### 自定义泛型
```
import java.util.*;
public class HelloWorld {
    public static void main(String[] args) {
        cachepPool<Integer> pool=new cachepPool<Integer>();
        pool.save(new Integer(1));
        Integer temp=pool.get();
        System.out.println(temp);
    }
}
class cachepPool<T> {
    T temp;
    public void save(T temp) {
        this.temp=temp;
    }
    public T get() {
        return this.temp;
    }
}
```

### Collections类常见方法
```
addAll()
reverse(list)
shuffle(list)
sort(list)
swap(list,i,j) //i,j互换
```



## 文件io

### 字节流
#### InputStream
```
int read(); //从输入流读取一个8位的字节，转换成整数，并返回
int read(byte[] b) //从输入流读取若干字节，并保存到参数b指定的字节数组中，返回整数表示读取的字节数(长度)
int read(byte[] b,int off,int len) //off表示指定字节数组开始保存的起始下标，len表示读取的字节的长度；
void close() ；关闭输入流，并释放资源；
```


#### OutputStream

```
void write(int b);
void write(byte[] b);
void write(byte[] b,int off,int len)
void flush(); //刷新并前置写出所有缓冲的输出字节
void close();
```













## 网络编程

#### InetAddress
```
IntAddress getByName(String host) //根据主机名返回主机的IP地址
IntAddress getLocalHost()  //创建一个表示本地主机的IntAddress对象
String getHostName()  //
boolean isReachable(int timeout) //判断指定时间是否能到达
String getHostAddress() //得到字符串格式原始ip
```
```java
import java.net.*;
public class HelloWorld {
    public static void main(String[] args) throws Exception {
        InetAddress localAddress= InetAddress.getLocalHost();
        InetAddress remoteAddress=InetAddress.getByName("www.baidu.com");
        System.out.println("本机ip地址:"+localAddress.getHostAddress());
        System.out.println("baidu的ip地址:"+remoteAddress.getHostAddress());
        System.out.println("3秒是否可以到达:"+remoteAddress.isReachable(3000));
        System.out.println("baidu的主机名："+remoteAddress.getHostName());
    }
}
```

### UDP通信
面向无连接协议。
#### DatagramPacket
封装UDP通信中发送或者接受的数据(集装箱)
```
DatagramPacket(byte[] buf,int length) //用于接收端，无需指定地址及端口
DatagramPacket(byte[] buf,int length,InetAddress addr,int port) //发送端
DatagramPacket(byte[] buf,int offset,int length)
DatagramPacket(byte[] buf,int offset,int length,InetAddress addr,int port)
```
```
InetAddress getAddress() //返回数据包ip
int getPort() //
byte[] getData() //返回数据包的数据
int getLength() //返回数据包的长度
```
#### DatagramSocket 
发送和接受DatagramPacket数据包(码头)
```
DatagramSocket(); //当前主机随机分配一个端口
DatagramSocket(int port); //
DatagramSocket(int port,InetAdress addr); //有多块网卡时，分配指定ip
```

```
void receive(DatagramPacket p) //用于接受数据填充到p中，在接受到数据前会一直处于阻塞状态；
void send(DatagramPacket p) //发送数据包p
void close() //关闭当前socket，释放资源
```

示例：
接收端：
```
import java.net.*;
public class UDPRecieve {
    public static void main(String[] args) throws Exception {
        byte[] buf=new byte[1024];
        DatagramSocket ds=new DatagramSocket(8954);
        DatagramPacket dp=new DatagramPacket(buf,1024);
        System.out.println("等待接受数据。。。");
        ds.receive(dp);
        String str=new String(dp.getData(),0,dp.getLength())+"from"+dp.getAddress().getHostAddress()+":"+dp.getPort(); //String接受（数组，起始位，长度，编码格式）
        System.out.println(str);
        ds.close();
    }
}
```
发送端：
```
import java.net.*;
public class UDPSend {
    public static void main(String[] args)throws Exception {
        DatagramSocket ds=new DatagramSocket(3000);
        String str="你好啊，收到了么？";
        DatagramPacket dp=new DatagramPacket(str.getBytes(),str.length(),InetAddress.getByName("localhost"),8954);
        ds.send(dp);
        ds.close();
    }
}
```

### TCP通信
严格区分客户端和服务端，必须先由客户端去连接服务端才能实现通信；
#### ServerSocket
```
ServerSocket() //这种形式创建后还需调用bind方法进行端口号绑定
ServerSocket(int port) //
ServerSocket(int port,int backlog) //backlog指定在服务器忙时可以与之保持连接请求的等待客户数量，不指定则为50；
ServerSocket(int port,int backlog,InertAddress bindAddr) //多块网卡时
```
```
Socket accept() //等待客户连接，接受客户端数据
InetAddress getInetAddress() //返回ServerSocket绑定的ip地址
boolean isClosed() //判断是否为关闭状态
void bind(SocketAddress endpoint) //绑定ip地址和端口号
```
#### Socket
```
Socket()
Socket(String host,int port) //连接到服务器的IP和端口
Socket(InetAddress address,int port)
```
```
int getPort() //
InetAddress getLocalAddress()
void close()
InputStream getInputStream() //获取对方的输入流或自己的输出流
OutputStream getOutputStream() //获取对方的输出流或自己的输入流
```
示例
```java
import java.io.*;
import java.net.*;
public class TCPServer {
    public static void main(String[] args) throws Exception {
        new TCPServers().listen();
    }
}
class TCPServers {
    private static final int PORT=7788;
    public void listen() throws Exception {
        ServerSocket serverSocket=new ServerSocket(PORT);
        Socket client=serverSocket.accept();
        OutputStream os =client.getOutputStream();
        System.out.println("开始于客户端交互数据");
        os.write(("哈哈，这是一条tcp测试").getBytes());
        os.write(("哈哈，这是第二条tcp测试").getBytes());
        Thread.sleep(5000);
        System.out.println("结束与客户端交互数据");
        os.close();
        client.close();
    }
}
```
```java
import java.io.*;
import java.net.*;
public class TCPClient {
    public static void main(String[] args) throws Exception {
        new TCPClients().connect();
    }
}
class TCPClients {
    private static final int PORT=7788;
    public void connect() throws Exception {
        Socket client = new Socket(InetAddress.getLocalHost(),PORT);
        InputStream is=client.getInputStream();
        byte [] buf=new byte[1024];
        int len=0;
        while((len=is.read(buf))!=-1){
            System.out.println(new String(buf,0,len));
        }
        client.close();
    }
}
```

文件上传
```
import java.io.*;
import java.net.*;
public class FileUploadTCPServer {
    public static void main(String[] args) throws Exception {
        ServerSocket serverSocket=new ServerSocket(10001);
        while(true) {
            Socket s=serverSocket.accept();
            //每当和客户建立Socket连接之后，单独开启一个线程处理和客户端的交互;
            new Thread(new ServerThread(s)).start();
        }
    }
}
class ServerThread implements Runnable {
    private Socket socket;
    public ServerThread(Socket socket) {
        this.socket=socket;
    }
    public void run() {
        String ip=socket.getInetAddress().getHostAddress();
        int count =1;
        try {
            InputStream in =socket.getInputStream(); //读取客户端发送的数据；
            File parentFile=new File("D:\\GJF\\JavaTest\\upload");
            if(!parentFile.exists()) {
                parentFile.mkdir();
            }
            File file=new File(parentFile,ip+"("+count+").jpg");
            while (file.exists()) {
                file=new File(parentFile,ip+"("+(count++)+").jpg");
            }
            FileOutputStream fos=new FileOutputStream(file);
            byte[] buf =new byte[1024];
            int len;
            while ((len=in.read(buf))!=-1) {
                fos.write(buf,0,len);    //将客户端的数据流写入到fos文件中；
            }
            OutputStream out =socket.getOutputStream();
            out.write("上传成功".getBytes());
            fos.close();
            socket.close();
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```
```
import java.io.*;
import java.net.*;

public class FileUploadTCPClient {
    public static void main(String[] args) throws Exception{
        Socket socket=new Socket("127.0.0.1",10001);
        OutputStream out=socket.getOutputStream();
        FileInputStream fis=new FileInputStream("D:\\GJF\\JavaTest\\1.jpg");
        byte[] buf=new byte[1024];
        int len;
        while((len=fis.read(buf))!=-1) {
            out.write(buf,0,len);
        }
        socket.shutdownOutput();
        InputStream in=socket.getInputStream();
        byte[] bufMsg=new byte[1024];
        int num=in.read(bufMsg);
        System.out.println(new String(bufMsg,0,num));
        fis.close();
        socket.close();
    }
}

```









编辑器：
psvm===》public static void main（String[] args） {};
sout===》System.out.println(obj);
​                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 























