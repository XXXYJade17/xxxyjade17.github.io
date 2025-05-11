# DQL

## 条件查询:
### select 字段表 from 表名 where 条件列表;
### 条件列表
- and  &&  :  与
- or  ||  :  或
- not  !  :  非
- is null  :  是否空
- <>  !=  :  不等
- between  __  and  __  : 之间
- in( __ , __ , ... ) : 满足条件
- like '_%'  : 模糊匹配   _单个字符   %任意字符
***
## 聚合函数
- count( )  :  统计行数
- avg( )  :  平均值
- max( )  :  最大值
- min( )  :  最小值
- sum( )  :  求和
***
## 分组查询
### select 字段表 from 表名 group by 字段表 having 筛选条件;
### 执行顺序 : where > 聚合函数 > having
***
## 排序查询 ( 默认升序 ) 
### 降序查询 : 
### select 字段表 from 表名 order by 字段表 desc;
### 升序查询 : 
### select 字段表 from 表名 order by 字段表 asc;
***
## 分页查询
### select 字段表 from 表名 limit 起始索引,查询记录数;
### 起始索引默认为0
***
## DQL执行顺序
- select        4      
- from         1        
- where       2        
- group by  ____  having      3       
- order by   5        
- limit          6        