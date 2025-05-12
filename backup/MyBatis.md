# 什么是MyBatis
**[MyBatis](https://mybatis.net.cn/getting-started.html)** 是一款优秀的持久层框架，它支持自定义 SQL、存储过程以及高级映射。MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。
***
# Maven依赖
```
<dependency>
         <groupId>org.mybatis.spring.boot</groupId>
         <artifactId>mybatis-spring-boot-starter-test</artifactId>
         <version>3.0.4</version>
         <scope>test</scope>
</dependency>
```
***
# 基本使用
- ### MySQL语句在代码里
```
@Mapper
public interface TestMapper {
    @Select(select * from user where id = #{id})
    public Result list(Pojo pojo);
}
```
  方法上, 加上mysql语句对应的注解, 括号内写上执行语句, 函数参数内封装的值 ( 例如 : id ), 会自动传入语句, 然后在数据库执行
  **注** : 封装的参数名**必须**与语句的参数占位符的参数名一致
- ### MySQL语句在.xml文件里
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.mybatis.example.BlogMapper">
  <select id="selectBlog" resultType="Blog">
    select * from Blog where id = #{id}
  </select>
</mapper>
```
```
@Mapper
public interface TestMapper {
    public Result list(Pojo pojo);
}
```
**[官方示例](https://mybatis.net.cn/getting-started.html)** : 
  **namespace :**  全限定名, 就是类路径  **(注 : 要与.xml文件同路径名)**  
  **id :**  方法名
  **resultType :**  方法返回结果
  <**select**> :  语句类型标签