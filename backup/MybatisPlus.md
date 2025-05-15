## 使用 MybatisPlus 的基本步骤：
- ### 引入 MybatisPlus 依赖，代替 Mybatis 依赖
  ```
  <dependency>
      <groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus-boot-starter</artifactId>
      <version>3.5.3.1</version>
  </dependency>
   ```
- ### 定义 Mapper 接口并继承 BaseMapper
  ```
  public interface UserMapper extends BaseMapper<User> {
  }
  ```
***
## MybatisPlus 是如何获取实现 CRUD 的数据库表信息的？
  - 默认以类名驼峰转下划线作为表名
  - 默认把名为 id 的字段作为主键
  - 默认把变量名驼峰转下划线作为表的字段名
***
## MybatisPlus 的常用注解有哪些？
  - @TableName : 指定表名称及全局配置
  - @TableId : 指定 id 字段及相关配置
  - @TableField : 指定普通字段及相关配置
***
## IdType 的常见类型有哪些？
  - AUTO : 数据库自增长
  - INPUT : 通过 set 方法自行输入
  - ASSIGN_ID : 分配 ID，接口 IdentifierGenerator 的方法 nextId 来生成 id，默认实现类为 DefaultIdentifierGenerator 雪花算法
***
## 使用 @TableField 的常见场景是？
  - 成员变量名与数据库字段名不一致
  - 成员变量名以 is 开头，且是布尔值
  - 成员变量名与数据库关键字冲突
  - 成员变量不是数据库字段