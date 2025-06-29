# 一、Redis 基础操作
## 1. 启动与退出
```
服务端
启动: redis-server.exe redis.windows.conf
退出: ctrl + c

客户端
启动: redis-cli.exe -h localhost -p 端口号(默认6379) -a 密码
退出: exit
```
## 2. 数据类型
Redis 存储的是 key-value 结构的数据，其中 key 是字符串类型，value 有 5 种常用的数据类型：
- 字符串 string
- 哈希 hash
- 列表 list
- 集合 set
- 有序集合 sorted set/zset
# 二、常用命令
## 1. 字符串常用命令
```set key value``` 设置指定key的值
```get key``` 获取指定key的值
```setex key seconds value``` 设置指定key的值，并将key的过期时间设为seconds秒
```setnx key value``` 只有key不存在时设置key的值 
## 2. 哈希操作常用命令
```hset key field value``` 将哈希表key中的字段field的值设为value
```hget key field``` 获取存储在哈希表中指定字段的值
```hdel key field``` 删除存储在哈希表中的指定字段
```hkeys key``` 获取哈希表中所有的字段
```hvals key``` 获取哈希表中所有的值
## 3. 列表操作常用命令(类似队列)
```lpush key value1 [value2]``` 将一个或多个值插入到列表头部
```lrange key start stop``` 获取列表指定范围内的元素
```rdrop key``` 移除并获取列表最后一个元素
```llen key``` 获取列表长度
## 4. 集合操作命令
```sadd key member1 [member2]``` 向集合添加一个或多个成员
```smembers key``` 返回集合中的所有成员
```scard key``` 获取集合的成员数
```sinter key1 [key2]``` 返回给定所有集合的交集
```sunion key1 [key2]``` 返回所有给定集合的并集
```srem key member1 [member2]``` 删除集合中一个或多个成员
## 5. 有序集合操作命令
```zadd key score1 member1 [score2 member2]``` 向有序集合添加一个或多个成员
```zrange key start stop [WITHSCORES]``` 通过索引区间返回有序集合中指定区间内的成员
```zincrby key increment member``` 有序集合中对指定成员的分数加上增量 increment
```zrem key member [member ...]``` 移除有序集合中的一个或多个成员
## 6. 通用命令
```keys pattern``` 查找所有符合给定模式 (pattern) 的 key
```exists key``` 检查给定 key 是否存在
```type key``` 返回 key 所储存的值的类型
```del key``` 该命令用于在 key 存在时删除 key 

# 三、Spring Data Redis 使用方式
## 1. 导入依赖
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```
## 2. 配置 Redis 数据源
```yml
redis:
    host: localhost
    port: 6379
    password: 123456
    database: 0
```
## 3. 编写配置类
```java
@Configuration
@Slf4j
public class RedisConfiguration {
    @Bean
    public RedisTemplate redisTemplate(RedisConnectionFactory redisConnectionFactory){
        log.info("开始创建redis模板对象");
        RedisTemplate redisTemplate = new RedisTemplate();
        //设置redis连接工厂对象
        redisTemplate.setConnectionFactory(redisConnectionFactory);
        //设置redis key的序列化器
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        return redisTemplate;
    }
}
```
## 4. RedisTemplate 操作示例
- 字符串操作
  ```java
  @Test
  public void testString(){
      // set
      redisTemplate.opsForValue().set("city","北京");
      // get
      String city = (String) redisTemplate.opsForValue().get("city");
      System.out.println(city);
      // setex
      redisTemplate.opsForValue().set("code","1234",3, TimeUnit.MINUTES);
      // setnx
      redisTemplate.opsForValue().setIfAbsent("lock","1");
      redisTemplate.opsForValue().setIfAbsent("lock","2");
  }
  ```
- 哈希操作
  ```java
  @Test
  public void testHash(){
      HashOperations hashOperations = redisTemplate.opsForHash();
      //hset
      hashOperations.put("100","name","tom ");
      hashOperations.put("100","age","20 ");
      //hget
      String name = (String) hashOperations.get("100","name");
      System.out.println(name);
      //hkeys
      Set keys = hashOperations.keys("100");
      System.out.println(keys);
      //hvals
      List values = hashOperations.values("100");
      System.out.println(values);
      //hdel
      hashOperations.delete("100","age");
  }
  ```
- 集合操作
  ```java
  @Test
  public void testSet(){
      SetOperations setOperations = redisTemplate.opsForSet();
      //sadd
      setOperations.add("set1","a","b","c","d");
      setOperations.add("set2","a","b","x","y");
      //smembers
      Set members = setOperations.members("set1");
      System.out.println(members);
      //scard
      Long size = setOperations.size("set1");
      System.out.println(size);
      //sinter
      Set intersect = setOperations.intersect("set1","set2");
      System.out.println(intersect);
      //sunion
      Set union = setOperations.union("set1","set2");
      System.out.println(union);
      //srem
      setOperations.remove("set1","a","b");
  }
  ```
- 有序集合操作
  ```java
  @Test
  public void testZset(){
      ZSetOperations zSetOperations = redisTemplate.opsForZSet();
      //zadd
      zSetOperations.add("zset1","a",10);
      zSetOperations.add("zset1","b",12);
      zSetOperations.add("zset1","c",9);
      //zrange
      Set zset1 = zSetOperations.range("zset1",0,-1);
      System.out.println(zset1);
      //zincrby
      zSetOperations.incrementScore("zset1","c",10);
      //zrem
      zSetOperations.remove("zset1","a","b");
  }
  ```
- 通用操作
  ```java
  @Test
  public void testCommon(){
      //keys
      Set keys = redisTemplate.keys("*");
      System.out.println(keys);
      //exists
      Boolean name = redisTemplate.hasKey("name");
      Boolean set1 = redisTemplate.hasKey("set1");
      //type
      for (Object key : keys) {
          DataType type = redisTemplate.type(key);
          System.out.println(type.name());
      }
      //del
      redisTemplate.delete("mylist");
  }
  ```