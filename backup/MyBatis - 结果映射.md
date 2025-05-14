### 查询部分
```
<select id="getById" resultMap="empResultMap">
        select
            e.* ,
            ee.id ee_id ,
            ee.emp_id ee_empid ,
            ee.begin ee_begin ,
            ee.end ee_end ,
            ee.company ee_company ,
            ee.job ee_job
        from emp e left join emp_expr ee on e.id = ee.emp_id
        where e.id = #{id}
</select>
```
  **查询结果将通过empResultMap进行映射处理**
***
### 结果映射部分
```
<resultMap id="empResultMap" type="com.xxxyjade17.pojo.Emp">
    <id column="id" property="id" />
    <result column="username" property="username" />
    <result column="password" property="password" />
    <result column="name" property="name" />
    <result column="gender" property="gender" />
    <result column="phone" property="phone" />
    <result column="job" property="job" />
    <result column="salary" property="salary" />
    <result column="image" property="image" />
    <result column="entryDate" property="entryDate" />
    <result column="deptId" property="deptId" />
    <result column="createTime" property="createTime" />
    <result column="updateTime" property="updateTime" />
    <collection property="exprList" ofType="com.xxxyjade17.pojo.EmpExpr">
        <id column="ee_id" property="id" />
        <result column="ee_company" property="company" />
        <result column="ee_job" property="job" />
        <result column="ee_begin" property="begin" />
        <result column="ee_end" property="end" />
        <result column="ee_empId" property="empId" />
    </collection>
</resultMap>
```
**主映射配置：**
  - type="" : 指定映射的目标 Java 类
  - column="" , property="" : 将查询结果的列映射到Emp类的属性
  - collection标签 : 用于处理一对多关系，将多条映射到类的属性上
  - ofType="" : 指定集合元素的类型
  - id标签 : 映射表的主键列, 提高缓存效率，确保对象唯一性
  - result标签 : 映射普通数据列, 无特殊需求，只需简单映射