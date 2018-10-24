# Servlet技术

```
ServerletConfig getServerletConfig()
String getServletInfo()
void init(ServerletConfig config)
void service(ServletRequest request, ServletResponse response)
void destory() 
```

- 初始化阶段，调用init()方法实现Servlet的初始化，整个生命周期只调用一次；
- 运行阶段，Servlet容器会为每个访问创建ServletRequest对象和ServletResponse对象，调用service方法来处理这两个对象。
- 销毁阶段，当服务器关闭或Web应用被移除容器时，销毁前会调用destory()访问请求；



## HttpServlet

示例demo：

编写FirstServlet类，通过继承HttpServletRequest类，实现doGet()和doPost()方法的重写；

```java
public class FirstServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html");
        PrintWriter pw = response.getWriter();
        pw.write("<h1> hello first servlet!</h1>");
    }
}
```

在web.xml中配置RequestMehtodServlet的映射路径，配置信息如下：

```xml
<servlet>
    <servlet-name>FirstServlet</servlet-name>   //Servlet名称
    <servlet-class>javademo.FirstServlet</servlet-class>  //完整类名
</servlet>
<servlet-mapping>  //用于映射Servlet虚拟路径
	<servlet-name>FirstServlet</servlet-name> //对应<servlet>里面的servlet-name
    <url-pattern>/demo</url-pattern>  //对应访问该Servlet的虚拟路径
</servlet-mapping>
```



### ServletConfig 和ServletContext

在Servlet运行期间经常需要一些辅助信息，在web.xml中使用一个或多个<init-param>元素进行配置；Tomcat在初始化一个Servlet时会将该Servlet的配置信息封装到一个ServletConfig对象中，调用init(ServletConfig config)方法将ServletConfig对象传递给Servlet。

```xml
<servlet> //位于servlet元素中
<init-param>
	<param-name>encoding</param-name>  //参数名
	<param-value>UTF-8</param-value>   //参数值
</init-param>
</servlet>
```

```
public class FirstServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		ServletConfig config=this.getServletConfig(); //获取ServletConfig对象
		String param=config.getInitParameter("encoding"); //获取name为encoding参数对应的value
    }
  }
```

ServletContext接口，每个Web应用创建唯一一个ServletContex对象，不同的Servlet之间可以共享里面的数据；

```xml
在根元素<web-app>里面：
    <context-param>
        <param-name>companyName</param-name>
        <param-vlaue>COSKA</param-vlaue>
    </context-param>
    <context-param>
        <param-name>address</param-name>
        <param-vlaue>wuhan</param-vlaue>
    </context-param>    
```

```java
public class FirstServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		ServletContext context =this.getServletContext(); //获取ServletContex对象
		Enumeration<String>paramNames=context.getInitParameterNames(); //获取属性名集合
		while(paramNames.hasMoreElements()) {
            String name=paramNames.nextElement(); //遍历获取属性名
            Sting value=context.getInitParameter(name); //根据属性名获取属性值
            ...
		}
    }
  }
```

增加、删除、设置ServletContext域属性的方法

```
Enumeration getAttributeNames()
Object getAttributes(String name)
void removeAttributes(String name)
void setAttributes(String name,Object obj)
```

## 请求和响应

响应体

- getOutputStream() 方法，获取ServletOutputStream输出流，输出二进制格式的响应正文
- getWriter() 方法，获取PrintWriter类型的输出流，直接输出字符串；

使用getOutputStream()方法：

```
public class FirstServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    	String data="itcast";
		OutputStream out=response.getOutputStream(); //获取响应体的输出流
		out.write(data.getBytes()) //向输出流写入字符串信息
		
    }
  }
```

使用getwrite()方法：

```
public class FirstServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    	String data="itcast";
		PrintWriter out=response.getWriter(); //获取响应体的输出流
		out.write(data) //向输出流写入字符串信息
		
    }
  }
```

乱码问题：

```
response.setCharacterEncoding("utf-8"); //设置响应编码
response.setHeader("Content-Type","text/html;charset=utf-8"); //通知浏览器编码形式
```

```
response.setContentType("text/html;charset=utf-8");
```

网页定时刷新并跳转：

```
response.setHeader("Refresh","2;URL=http://www.itcast.cn") //刷新并跳转
response.setHeader("Refresh","3") //定时刷新
```

禁止浏览器缓存（及时更新javaScript）

```
response.setDateHeader("Expires",0)
response.setHeader("Cache-Control","no-cache")
response.setHeader("Pragma","no-cache")
```

请求重定向：

```
response.sendRedirect("xxxx/xxx/xx/index.html")
```

示例：

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        String username=request.getParameter("username");
        String passworld=request.getParameter("passworld");
        if(("itcast").equals(username)&&("123").equals(passworld)){
            response.sendRedirect("/welcome.html");
        }else{
            response.sendRedirect("/login.html");
        }

    }
```





### JSP

java Server Page

当用户通过URL访问Servlet时，Web服务器会根据请求的URL地址在web.xml配置文件里面查找匹配的<servlet-mapping>,然后将请求交给<servlet-mapping>指定的Servlet去执行；

jsp的web.xml配置

```xml
    <servlet>
        <servlet-name>SimpleJspServlet</servlet-name>
        <jsp-file>/simple.jsp</jsp-file>
    </servlet>
    <servlet-mapping>
        <servlet-name>SimpleJspServlet</servlet-name>
        <url-pattern>/itcast</url-pattern>
    </servlet-mapping>
```

```
<%= expression%> //表示输出，不能有分号
<%= new java.util.Date().toLocaleString()%> 
```

```
<% 
	int x=3; //必须有分号,脚本片段；	
%>
...
<% 
	out.println(x); //必须有分号	
%>
```

#### JSP隐式对象：

```
out
request
response
config
session
application //所有用户的共享信息
page  //当前页面转换后的Servlet实例
pageContext //jsp的页面容器
exception //发生的异常
```

out:out对象的类型为JspWriter，相当于一种带缓存功能的PrintWriter();

#### pageContext对象：

```
JspWriter getOut()  //获取out隐式对象
Object getPage()  //用于获取page隐式对象
ServletRequest getRequest()  //用于获取request隐式对象
ServletResponse getResponse() //用于获取response隐式对象
HttpSession getSession() //用于获取session隐式对象
Exception getException() //用于获取exception隐式对象
ServletConfig getServletConfig() //用于获取config隐式对象
ServletContext getServletContext() 
```

```
<%
	HttpServletRequest req=(HttpServletRequest)pageContext.getRequest();
	String ip=request.getRemoteAddr();
	out.println("本机ip地址为:"+ip);
%>
```

pageContext不仅提供了获取隐式对象的方法，还提供了存储数据的功能。

```
void setAttribute(String name,Object value,int scope) //用于设置pageContext对象的属性
Object getAttribute(String name,int scope)
void removeAttribute(String name,int scope)
void removeAttribute(String name)
Object findAttribute(String name)
```



####   < jsp:include>标签

```
<jsp:include page="relativeURL" flush="true|false" />
如：
<jsp:include page="include.jsp" flsu="true" />
```

#### < jsp:forward>标签

```
<jsp:forward page="realtiveURL“/>
```

































