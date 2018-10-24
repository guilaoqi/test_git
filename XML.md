# 1.XML基础

## DTD语法

### 1.元素定义

```dtd
<!ELEMENT 元素名称 元素内容>
<!ElEMENT 书名 (#PCDATA)> //普通文本字符串
<!ELEMENT 书（书名，作者，售价）> //()嵌套一组子元素
<!ElEMENT 书（#PCDATA|书名)* > //混合内容
<!ELEMENT br EMPTY>  //空元素
<!ELEMENT 联系人 ANY>  //表示可以包含任意内容，除了根元素外，其他元素使用ANY都将失去DTD对XML文档的约束效果。
```

### 2.属性定义

```
<! ATTLIST 元素名 
	属性名1 属性类型 设置说明
	属性名2 属性类型 设置说明
	。。。
!>
例如：
<! ATTLIST 肉 品种 （鸡肉|牛肉|猪肉|鱼肉）“鸡肉”>  //<肉 品种=“鱼肉”/> <肉/>不设置时标签默认属性为"鸡肉"；

```

#### IDREF 和 IDREFS

IDREF建立一对一，IDREFS建立一对多；

```dtd
<!ATTLIST 联系人
	编号 ID #REQUIRED
	上司 IDREF #IMPLIED //#IMPLIED表示该属性可有可无
>
```

#### NMTOKEN 和 NMTOKENS

Name Token，一个元素的NMTOKENS属性设置值可以是另外多个NMTOKEN类型属性的设置值；

#### NOTATION

对于图像、声音、影像等XML无法直接处理的数据，可以通过设置NOTATION类型的属性来让一个外部应用程序处理。

```
<!NOTATION 符号名 SYSTEM "MIME类型“>
<!NOTATION 符号名 SYSTEM "URL路径名“>
```

```
<? xml version="1.0" encoding="GB2312" standalone="yes"?>
<! DOCTYPE 文件[
    <!NOTATION mp SYSTEM "movPlayer.exe">
    <!NOTATION gif SYSTEM "Image/gif">
    <!ELEMENT 文件 ANY>
    <!ELEMENT 电影 EMPTY>
    <!ATTLIST 电影 演示设备 NOTATION (mp|gif) #REQUIRED>
]>
<文件>
	<电影 演示设备="mp">
</文件>
```

#### ENTITY和ENTITYS

实体

```
<!ENTITY itcast "传智播客，www.itcast.cn">
<!ELEMENT 电影 EMPTY>
<!ATTLIST 电影 来源 ENTITY #REQUIRED>
```

```
<电影 来源="&itcast;"/>  //通过&实体名的方式引用
```

参数实体(DTD文件自身使用)

```dtd
<!ENTITY % TAG_NAME "姓名|EMAIL|电话|地址">
<!ENTITY 个人信息 (% TAG_NAME;|生日)>
<!ENTITY 客户信息 （% TAG_NAME;|公司名)>
```

## Schema约束

名称空间

```
<元素名 xmlns:prefixname="URI">
```

元素定义

```xs
<xs:element name="xxx" type="yyy"/>
<xs:element name="age" type="xs:integer"/>  //<age>28</age>
```

属性定义

```
<xs:attribute name="xxx" type="yyy"/>
```

简单类型

```xml
<xs:element name="age">
<xs:simpleType>
	<xs:restriction base="xs:integer">
		<xs:minInclusive value="18"/>
		<xs:maxInclusive value="58"/>
	</xs:restriction>
</xs:simpleType>
</xs:element>
```

