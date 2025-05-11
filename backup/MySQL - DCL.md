# DCL
## 管理用户
- ### 查询用户
```
use mysql;
select * from user;
```
- ### 创建用户
```
create user '用户名@主机名' identified by '密码';
```
- ### 修改用户密码
```
alter user '用户名@主机名' identified with '原密码' by '新密码';
```
- ### 删除用户
```
drop user '用户名@主机名';
```
***
## 权限控制
- ### 查询权限
```
show grants for '用户名@主机名';
```
- ### 授予权限
```
grant 权限 on 数据库名  表名 to '用户名@主机名';
```
- ### 撤销权限
```
revoke 权限 on 数据库名  表名 from '用户名@主机名';
```
- ### 权限
  **1. 所有权限  :  all, all privileges**
  **2. 查询数据  :  select**
  **3.  插入数据  :  insert**
  **4. 修改数据  :  update**
  **5. 删除数据  :  delete**
  **6. 修改表  :  alter**
  **7. 删除数据库 / 表 / 视图  :  drop**
  **8. 创建数据库 / 表  :  create**
***
## 函数
- ### 字符串函数
  **1. 字符串拼接 :**  ```concat(s1,s2,...); ```
  **2. 转小写 :**  ```lower(str)```
  **3. 转大写 :**  ```upper(str)```
  **4. 在str左侧以pad填充，至长度为n :**  ```lpad(str,n,pad)```
  **5. 在str右侧以pad填充，至长度为n :**  ```rpad(str,n,pad)```
  **6. 去除两侧空格 :**  ```trim(str)```
  **7. 从字符串的start开始截取len个字符 :**  ```substring(str,start,len)```
- ### 数值函数
  **1. 向上取整 :**  ```ceil(num)```
  **2. 向下取整  :**  ```floor( num )```
  **3. x/y的模 :**  ```mod(x/y)```
  **4. 0~1随机数 :**  ```rand( )```
  **5. 四舍五入,保留y位小数 :**  ```round(x,y)```
- ### 日期函数
  **1. 当前日期 :**  ```curdate( )```
  **2. 当前时间 :**  ```curtime( )```
  **3. 当前日期和时间 :**  ```now()```
  **4. 当前年份 :**  ```year(date)```
  **5. 当前月份 :**  ```month(date)```
  **6. 当前日 :**  ```day(date)```
  **7. 在当前日期的type(年月日)加value :**  ```date_add(date,interval value type)```
  **8. 两个之间差的天数 :**  ```detediff(date1,date2)```
- ### 流程控制函数
  1. ```if(value,t,f)```
解释  :  value真返t假返f
  2. ```ifnull(value1,value2)```
解释  :  value1不为null返value1，为null返value2
  3. ```case value when value1 then ans1 when value2 then ans2 ... else default end```
解释  :  value等于value1返回ans1,等于value2返回ans2,...,否则返回default
***
## 约束
- ### 示例
```
create table user(
    id int primary key  auto_increment comment,
    name varchar(10) not null unique ,
    age int check ( age>0 && age<=120 ),
    status char(1) default 1,
    gender char(1)
)
```
- ### 约束种类
  **not null : 非空约束**
  **unique : 唯一约束**
  **primary key : 主键约束**
  **default : 默认约束**
  **check(条件) : 检查约束**
  **foreign key : 外键约束**
***
## 约束外键
- ### 加入外键
```
create table 表名(
    字段名 数据类型
     ......
     constraint 外键名 foreign key(外键字段名) references 主表(列名)
);
alter table 表名 
add constraint 外键名 
foreign key(外键字段名) 
references 主表(列名)
on delete 删除行为
on update 更新行为;
```
- ### 删除外键
```
alter table 表名 drop  foreign key 外键名
```
- ### 外键删除/更新的行为及其含义
  **no action  :  当在父表中删除 / 更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除 / 更新。(与 restrict 一致)**
  **restrict  :  同no action**
  **cascde  :  当在父表中删除 / 更新对应记录时，首先检查该记录是否有对应外键，如果有，则也删除 / 更新外键在子表中的记录**
  **set null  :  当在父表中删除对应记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为 null（这就要求该外键允许取 null）**
  **set default  :  父表有变更时，子表将外键列设置成一个默认的值 (Innodb 不支持)**
***
## 多表查询
- ### 内连接(交集)
  **隐式  :** 
  ```select 字段名,... from 表名,... where 条件```
  **显式  :**
  ```
  select 字段名,... from 表名1 inner join 表名2 on 条件
  select 字段名,... from 表名1 join 表名2 on 条件
  ```
- ### 外连接(交集+本体)
  **左外连接 :** 
  ``` 
  select 字段名,... from 表名1 left outer join 表名2 on 条件
  select 字段名,... from 表名1 left join 表名2 on 条件
  ```
  **右外连接 :** 
  ```
  select 字段名,... from 表名1 right outer join 表名2 on 条件;
  select 字段名,... from 表名1 right join 表名2 on 条件
  ```
- ### 自连接 :
  ```
  select 字段名,... from 表名 别名1,表名 别名2 where 条件
  select 字段名,... from 表名 别名1 left join 表名 别名2 on 条件
  ```
- ### 联合查询
  **不去重 :**	
  ```
  select 字段名 from 表名 where 条件1
  union all
  select 字段名 from 表名 where 条件2
  ```
  **去重 :** 	
  ```
  select * from emp where 条件1
  union
  select * from emp where 条件2;
  ```
- ### 子查询(条件嵌套)
  **标量子查询  :**  ```select 字段名 from 表名 where 条件[select 字段名 from 表名 where 条件]```
  - **例  :**  ```select * from emp where entrydate > (select entrydate from emp where name = '方东白')```
 
  **列子查询  :**  ```select 字段名 from 表名 where 字段名 操作符 (select 字段名 from 表名 where 条件)```
  - **例  :**  
  ```
  select * from emp where dept_id in (select id from dept where name = '销售部' or name = '市场部')
  select * from emp where salary > all ( select salary from emp where dept_id = (select id from dept where name = '财务部') )
  select * from emp where salary > some ( select salary from emp where dept_id = (select id from dept where name = '研发部') )
  ```
  **操作符  :**
  - **in  :  在指定的集合范围之内，多选一**
  - **not in  :  不在指定的集合范围之内**
  - **any  :  子查询返回列表中，有任意一个满足即可**
  - **some  :  与 any 等同，使用 some 的地方都可以使用 any**
  - **all  :  子查询返回列表的所有值都必须满足**
  
  **行子查询  :**  ```select 字段名,... from 表名 where (字段名1,字段名2) = (select 字段名1, 字段名2 from 表名 where 条件)```
  - **例  :**  ```select * from emp where (salary,managerid) = (select salary, managerid from emp where name = '张无忌')```
 
  **表子查询:**
  - **列方式  :**  ```select 字段名,... from 表名 where (字段名1,字段名2) 列子查询操作符 (select 字段名1, 字段名2 from 表名 where 条件)```
    - **例  :**  ```select * from emp where (job,salary) in ( select job, salary from emp where name = '鹿杖客' or name = '宋远桥' )```
  - **内外连接方式 :**  ```select 字段名1,字段名2 from (select 字段名 from 表名 where 条件) 别名 left join 表名 on 条件[使用别名]```
    - **例 :**  ```select e.*, d.* from (select * from emp where entrydate > '2006-01-01') e left join dept d on e.dept_id = d.id```
***
## 事务
  - **开启事务 :**  ```start transaction``` 
  - **提交 / 回滚事务 :**  ```commit /rollback```
  - **事务隔离级别 :** 
    **1. read uncommitted**
    **2. read committed**
    **3. repeatable read** 
    **4. serializable**
## 引擎
  - **在创建表时，指定存储引擎 :**
  ```
  create table 表名 (
       字段 1 字段 1 类型,
	......,
	字段 n 字段 n 类型
  ) engine = 引擎类型
  ```
  - **查看当前数据库支持的存储引擎 :**  ```show engines```
## 索引
  - **创建索引 :**  ```create [unique|fulltext] index 索引名 on 表名(字段名)```
  -**查看索引 :**  ```show index from 表名```
  -**删除索引 :**  ```drop index 索引名 on 表名```
## 性能分析
  - **查看执行频次 : MySQL 客户端连接成功后 , 通过**  ```show [session|global] status```  **命令可以提供服务器状态信息**
  ```show global status like 'Com______';```
 - **慢查询日志 : 记录了所有执行时间超过指定参数（long_query_time，单位：秒，默认 10 秒）的所有 SQL 语句的日志。
	    MySQL 的慢查询日志默认没有开启，需要在 MySQL 的配置文件（/etc/my.cnf）中配置如下信息 :**
  ```	    
# 开启 MySQL 慢日志查询开关
slow_query_log=1
# 设置慢日志的时间为 2 秒，SQL 语句执行时间超过 2 秒，就会视为慢查询，记录慢查询日志
long_query_time=2
```
## Profiles :
  - **执行一系列的业务 SQL 的操作，然后通过如下指令查看指令的执行耗时 :**
  ```
  # 查看每一条 SQL 的耗时基本情况
  show profiles;
  # 查看指定 query_id 的 SQL 语句各个阶段的耗时情况
  show profile for query query_id;
  # 查看指定 query_id 的 SQL 语句 CPU 的使用情况
  show profile cpu for query query_id;
  ```
## explain 执行计划 :
  - **语法 :** ```explain select 字段名 from 表名 where 条件```
  - **explain执行计划各字段含义 :**
    **id：select 查询的序列号，表示查询中执行 select 子句或者是操作表的顺序。id 相同，执行顺序从上到下；id 不同，值越大，越先执行**
    **select_type：表示 SELECT 的类型，常见取值有 SIMPLE（简单表，即不使用表连接或者子查询 ）、PRIMARY（主查询，即外层的查询 ）、UNION（UNION 中的第二个		或者后面的查询语句 ）、SUBQUERY（SELECT/WHERE 之后包含了子查询 ）等**
    **type：表示连接类型，性能由好到差的连接类型为 NULL、system、const、eq_ref、ref、range、index、all **
    **possible_key：显示可能应用在这张表上的索引，一个或多个**
    **key：实际使用的索引，如果为 NULL，则没有使用索引**
    **key_len：表示索引中使用的字节数，该值为索引字段最大可能长度，并非实际使用长度，在不损失精确性的前提下，长度越短越好 **
    **rows：MySQL 认为必须要执行查询的行数，在 innodb 引擎的表中，是一个估计值，可能并不总是准确的**
    **filtered：表示返回结果的行数占需读取行数的百分比，filtered 的值越大越好**