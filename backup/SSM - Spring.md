## IoC(Inversion of Control)
- 控制反转 : 使用对象时,由**主动new产生对象**转换为**外部提供对象**
- Spring中提供了一个IoC容器充当外部，统一管理对象的创建和初始化
- 在IoC容器中被管理的对象统称为Bean
- 示例 :
  获取IoC容器 : ``` ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml"); ```
  获取bean : ``` BookDao bookDao = (BookDao) ctx.getBean("bookDao"); ```
***
## Bean
- #### 一、Bean 基本配置
  1. 别名（name）
    可定义多个别名，用逗号 (,)、分号 (;) 或空格分隔。
    通过 id 或 name 未找到 Bean 时，抛出 NoSuchBeanDefinitionException。
  2. 作用范围（scope）
    singleton（默认）：单例模式，容器中仅存在一个实例。
    prototype：每次请求创建新实例。
  3. 唯一标识（id）
    Bean 在容器中的唯一标识符，不可重复。
  4. 实现类（class）
    指定 Bean 的全限定类名（包名 + 类名），容器通过反射创建实例。
  5. 依赖注入（ref/value）
    ref：引用其他 Bean（如 <property name="userDao" ref="userDao"/>）。
    value：注入基本数据类型值（如 <property name="price" value="10"/>）。
  6. 自动装配（autowire）
    byType：按类型匹配注入。
    byName：按属性名与 Bean 的 id/name 匹配注入。
- #### 二、Bean 实例化方式
  1. 构造方法实例化 ``` <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/> ```
  bean的本质就是对象，如果没有对应的无参构造，将抛出异常BeanCreationException
  2. 静态工厂方法 ```<bean id="bookDao" class="com.itheima.factory.BookDaoFactory" factory-method="createBookDao"/> ```
  class : 指定工厂类
  factory-method : 指定静态创建方法。
  3. 实例工厂方法
      ```xml 
      <bean id="userFactory" class="com.itheima.factory.UserDaoFactory"/>
      <bean id="userDao" factory-bean="userFactory" factory-method="getUserDao"/> 
      ```  
      <blockquote> 先实例化工厂 Bean，再通过 factory-bean 引用工厂实例并调用方法。 </blockquote>
  4. FactoryBean 接口
      ```java
      public class UserDaoFactoryBean implements FactoryBean<UserDao> {
          @Override
          public UserDao getObject() { return new UserDaoImpl(); }
          @Override
          public Class<?> getObjectType() { return UserDao.class; }
      }
      ```
     ```xml
     <bean id="userDao" class="com.itheima.factory.UserDaoFactoryBean"/>
     ```
- #### 三、Bean生命周期  
  核心顺序：创建对象 → 构造方法 → 属性注入 → 初始化方法 → 业务操作 → 销毁方法。
  1. 初始化容器阶段（容器启动时执行）
    容器启动后，依次完成 Bean 的创建与初始化，
    创建对象（内存分配）-> 执行构造方法 -> 执行属性注入（set 操作）-> 执行 Bean 初始化方法
  2. 使用 Bean 阶段（容器运行时）
    Bean 准备就绪后，供应用调用： 执行业务操作
  3. 关闭 / 销毁容器阶段（容器关闭时执行）： 执行 Bean 销毁方法
    1.手工关闭容器 : ConfigurableApplicationContext 接口 close() 操作
       ```java       
       ConfigurableApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
       context.close(); 
       ```
     2.注册关闭钩子， 虚拟机退出前自动关闭容器
     ```java
     context.registerShutdownHook();
     ```
- #### 四、控制 Bean 生命周期
  1. 自定义方法 + XML 配置
     ```java
     public class BookDaoImpl {
         public void init() { System.out.println("初始化..."); }
         public void destroy() { System.out.println("销毁..."); }
     }
     ```
     ```xml
     <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl" 
           init-method="init" destroy-method="destroy"/>
     ```

  2. 实现 InitializingBean, DisposableBean 接口
     ```java
     public class BookServiceImpl implements InitializingBean, DisposableBean {
         @Override
         public void afterPropertiesSet() { System.out.println("属性注入后初始化..."); }
         @Override
         public void destroy() { System.out.println("容器销毁前执行..."); }
     }
     ```
***
## DI(Dependecy Injection)
- 依赖注入：在IoC中建立bean和bean之间的依赖关系的过程
- 注入方式
  #### Setter 注入
    1. 定义变量
        ```java
        private BookDao bookDao;
        private int price;
        ```
    2. 提供 Set 方法
        ```java
        public void setBookDao(BookDao bookDao) {
            this.bookDao = bookDao;
        }
        public void setPrice(int price) {
            this.price = price;
        }
        ```
    3. XML 配置
        ```xml
        <bean id="bookService" class="com.xxxyjade17.service.impl.BookServiceImpl">
            <property name="bookDao" ref="bookDao"/>  <!-- 引用其他Bean -->
            <property name="price" value="10"/>       <!-- 注入基本值 -->
        </bean>
        ```
  #### 构造器注入
    1. 定义带参构造器
        ```java
        public BookServiceImpl(BookDao bookDao, int price) {
            this.bookDao = bookDao;
            this.price = price;
        }
        ```
    2. XML 配置（三种方式）
        ```xml
        <bean id="bookService" class="com.xxxyjade17.service.impl.BookServiceImpl">
            <!-- 方式1：按参数名匹配 -->
            <constructor-arg name="bookDao" ref="bookDao"/>
            <constructor-arg name="price" value="10"/>
    
            <!-- 方式2：按参数索引匹配（从0开始） -->
            <constructor-arg index="0" ref="bookDao"/>
            <constructor-arg index="1" value="10"/>
    
            <!-- 方式3：按参数类型匹配 -->
            <constructor-arg type="com.xxxyjade17.dao.BookDao" ref="bookDao"/>
            <constructor-arg type="int" value="10"/>
        </bean>
        ```
- 集合注入
  1. 数组（Array）
      ```xml
      <property name="array">
          <array>
              <value>100</value>
              <value>200</value>
              <value>300</value>
          </array>
      </property>
      ```
  2. List 集合
      ```xml
      <property name="list">
          <list>
              <value>itcast</value>
              <value>itheima</value>
              <value>boxuegu</value>
          </list>
      </property>
      ```
  3. Set 集合
      ```xml
      <property name="set">
          <set>
              <value>itcast</value>
              <value>itheima</value>
              <value>boxuegu</value> <!-- 重复元素会被自动去重 -->
          </set>
      </property>
      ```
  4. Map 集合
      ```xml
      <property name="map">
          <map>
              <entry key="country" value="china"/>
              <entry key="province" value="henan"/>
              <entry key="city" value="kaifeng"/>
          </map>
      </property>
      ```
  5. Properties 集合
      ```xml
      <property name="properties">
          <props>
              <prop key="country">china</prop>
              <prop key="province">henan</prop>
              <prop key="city">kaifeng</prop>
          </props>
      </property>
      ```
- 复杂对象注入
  若集合元素是其他 Bean，可替换``` <value> ```为``` <ref bean="beanId"> ```
    ```xml
    <list>
        <ref bean="userService"/> <!-- 引用容器中 id 为 userService 的 Bean -->
    </list>
    ```
