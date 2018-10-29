# JDBC

- 1.注册驱动
- 2.获取连接
- 3.创建执行sql语句的对象
- 4.书写sql语句
- 5.执行sql语句
- 6、对结果进行处理

```java

public class TestLogin {
    public void login(String name,String password) throws ClassNotFoundException,SQLException {
        //1.注册驱动
        Class.forName("com.mysql.jdbc.Driver");
        //2.获取连接
        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/db_test", "root", "12345");
        //3.创建执行sql语句的对象
        Statement stmt = conn.createStatement();
        //4.书写sql语句
        String sql="select * from test where "+"name='"+name+"' and password='"+password+"'";
        //5.执行sql语句
        ResultSet rs = stmt.executeQuery(sql);
        //6、对结果进行处理
        if(rs.next()){
            System.out.println("恭喜你，登录成功！"+name);
        }else{
            System.out.println("账号或密码错误！");
        }
        if(rs!=null)rs.close();
        if(stmt!=null)stmt.close();
        if(conn!=null)conn.close();
    }
    @Test
    public void testLogin(){
        try {
            login("root","12345");
        }catch(Exception e){
            e.printStackTrace();
        }
    }
}
```

预编译声明

```
public class TestLogin {
    public void login(String name,String password) throws ClassNotFoundException,SQLException {
        //1.注册驱动
        Class.forName("com.mysql.jdbc.Driver");
        //2.获取连接
        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/db_test", "root", "12345");
        //3.书写sql语句
        String sql="select * from test where name=? and password=? ";
        //4.创建预处理对象
        PreparedStatement pstmt = conn.prepareStatement(sql);
        //5.设置参数
        pstmt.setString(1,name);
        pstmt.setString(2,password);
        //6、执行查询操作
        ResultSet rs = pstmt.executeQuery();
        //6、对结果进行处理
        if(rs.next()){
            System.out.println("恭喜你，登录成功！"+name);
        }else{
            System.out.println("账号或密码错误！");
        }
        if(rs!=null)rs.close();
        if(stmt!=null)stmt.close();
        if(conn!=null)conn.close();
    }
}
```



int executeUpdate();  执行insert update delete语句

ResultSet executeQuery(); 执行select语句

boolean execute();  执行select返回true 执行其他语句返回false；



## limit 

xxx  limit 6,3  //起始位置6，查3条



## 外键

比如商品有很多分类；

分类表category称为主表，cid为主键。商品表products称为从表，category_id称为外键；

从表外键的值是对主表主键的引用；

声明外键约束：

```sql
alter table 从表 add [constraint][外键名称] foreign key (从表字段名) references 主表(主表的主键)
```

```
ALTER TABLE product add FOREIGN KEY(category_id) REFERENCES category(cid);
```

删除外键约束：

```
alter table 从表 drop foreign key 外键名称
```

使用外键：保证数据的完整性；

从表中不能添加主表中不存在的记录；

**多对多**的表中，需要创建第三张表，中间表中至少两个字段，这两个字段分别作为外键指向各自一方的主键；



## C3P0连接池

导入：c3p0-x.x.x-pre5.jar包

配置文件，名称固定：c3p0-config.xml

```
ComboPooleDateSource dataSource = new ComboPooledDataSource(); //加载默认的配置

ComboPooleDateSource dataSource = new ComboPooledDataSource(); //加载有名称的配置
```

