# 什么是PageHelper
**PageHelper是第三方提供在 Mybatis 框架中实现分页的插件, 用来简化分页操作, 提高开发效率**
***
# 导入依赖
```
<!--分页插件PageHelper-->
<dependency>
       <groupId>com.github.pagehelper</groupId>
       <artifactId>pagehelper-spring-boot-starter</artifactId>
       <version>1.4.7</version>
</dependency>
```
# 实现代码
- ### Mapper接口
  ```
  @Mapper
  public interface DemoMapper {
      @Select("select emp.*,dept.name deptName from emp left join dept on emp.dept_id=dept.id order by emp.update_time desc")
      public List<Emp> list();
   } 
  ```
  **在实现分页查找的MySQL语句省略limit部分**
- ### Service实现类
  ```
    @Override
    public PageResult<Emp> page(Integer page, Integer pageSize) {

            /**
               *设置分页参数
               *@param page 页码
               *@param pageSize 单页记录数
               */
            PageHelper.startPage(page,pageSize);

            //执行查询
            List<Emp> empList = empMapper.list();

            //结果封装
            Page<Emp> p = (Page)empList;

            return new PageResult<Emp>(p.getTotal(),p.getResult());
    }
  ```
# 总结
  - MySQL语句少写了limit部分
  - Mapper接口可以不用再写page和pageSize参数
  - 不用重新去计算page的起始索引