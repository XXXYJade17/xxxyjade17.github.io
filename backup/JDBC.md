# JDBC是什么
JDBC（Java DataBase Connectivity）使用 Java 语言操作关系型数据库的一套 API
# 导入依赖
```xml
<dependency>
        <groupId>com.mysql</groupId>
        <artifactId>mysql-connector-j</artifactId>
        <version>8.0.33</version>
</dependency>
```
# 简单示例
```java
[1.注册驱动]
Class.forName("com.mysql.cj.jdbc.Driver");

[2.获取数据库连接]
//数据库地址
String url="jdbc:mysql://localhost:3306/web";     
//数据库用户名
String username="root";
//密码
String password="xxxyjade17";
//创建连接对象
Connection connection=DriverManager.getConnection(url,username,password);

[3.获取SQL语句执行]
//创建预执行对象
Statement statement=connection.createStatement();

[4.执行sql]
//接收返回值
int i=statement.executeUpdate("update user set age = 30 where id = 1");

[5.释放资源]
statement.close();
connection.close();
```
# 预编译示例
```java
[1.数据库配置]
String url="jdbc:mysql://localhost:3306/web";
String username="root";
String password="xxxyjade17";
//数据库操作语句
String sql="SELECT id,username,password,name,age from user where username=? and password=?";

[2.创建对象]
//连接对象
Connection connection = null;
//预编译对象
PreparedStatement stmt = null;
//查询返回的结果
ResultSet rs = null;  

[3.执行]
try{
    //创建驱动
    Class.forName("com.mysql.cj.jdbc.Driver");
    //获取连接
    connection=DriverManager.getConnection(url,username,password);
    //获取预编译对象
    stmt=connection.prepareStatement(sql);
    //赋值
    stmt.setString(1,"daqiao");
    stmt.setString(2,"123456");
    //执行操作
    rs=stmt.executeQuery();
    while(rs.next()){
        User user=new User(
            rs.getInt("id"),
            rs.getString("username"),
            rs.getString("password"),
            rs.getString("name"),
            rs.getInt("age")
        );
        System.out.println(user);
    }
} catch (ClassNotFoundException e) {
    throw new RuntimeException(e);
} catch (SQLException e) {
    throw new RuntimeException(e);
} finally {
    try{
        if(rs!=null)rs.close();
        if(stmt!=null)stmt.close();
        if(connection!=null)connection.close();
    } catch (SQLException e) {
        throw new RuntimeException(e);
    }
}
```
