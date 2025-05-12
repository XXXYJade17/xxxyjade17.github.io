# 动态SQL的概念与应用
动态SQL是一种根据不同的条件动态生成SQL语句的技术。在MyBatis等ORM框架中，动态SQL能够根据传入的参数值的不同，拼接出不同的SQL语句，从而实现更加灵活的数据库操作。这种技术主要用于处理复杂的查询条件，避免了硬编码SQL语句的不便，同时也减少了因手动拼接SQL语句而导致的错误。
***
# IF
```
<if test="gender != null">
      and e.gender = #{gender}
</if>
```
  test后面加条件判断, 如果满足就会拼接标签中的语句
***
# WHERE
```
<where>
       <if test="id != null">
              id=#{id}
       </if>
              and deleteFlag=0;
</where>
```
  根据查询条件, 来生成where关键字, 并会自动去除条件前面多余的and或or