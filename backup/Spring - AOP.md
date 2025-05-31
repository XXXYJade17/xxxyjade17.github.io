## 什么是AOP
Aspect Oriented Programming（面向切面编程、面向方面编程），可简单理解为就是面向特定方法编程。
#### 优点 : 
  1. 减少重复代码
  2. 代码无侵入
  3. 提高开发效率
  4. 维护方便
***
## 导入依赖
```
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```
***
## 核心概念
- #### 连接点：JoinPoint，可以被 AOP 控制的方法（暗含方法执行时的相关信息）
- #### 通知：Advice，指那些重复的逻辑，也就是共性功能（最终体现为一个方法）
- #### 切入点：PointCut，匹配连接点的条件，通知仅会在切入点方法执行时被应用
- #### 切面：Aspect，描述通知与切入点的对应关系（通知 + 切入点）
- #### 目标对象：Target，通知所应用的对象
***
## 通知类型
- #### @Around：环绕通知，此注解标注的通知方法在目标方法前、后都被执行
  ##### 注意1: @Around环绕通知需自行调用 ProceedingJoinPoint.proceed() 来让原始方法执行，其他通知无需考虑目标方法执行 
  ##### 注意2: @Around环绕通知方法的返回值，必须指定为 Object 类型，用于接收原始方法的返回值
- #### @Before：前置通知，此注解标注的通知方法在目标方法前被执行
- #### @After ：后置通知，此注解标注的通知方法在目标方法后被执行，无论是否有异常都会执行
- #### @AfterReturning： 返回后通知，此注解标注的通知方法在目标方法后被执行，有异常不会执行
- #### @AfterThrowing ： 异常后通知，此注解标注的通知方法发生异常后执行
***
## 通知顺序
当有多个切面的切入点都匹配到了目标方法，目标方法运行时，多个通知方法都会被执行。
### 执行顺序：
- #### 不同切面类中，默认按照切面类的类名字母排序 :
  - 目标方法前的通知方法：字母排名靠前的先执行
  - 目标方法后的通知方法：字母排名靠前的后执行
- #### 用@Order(数字)加在切面类上来控制顺序 :
  - 目标方法前的通知方法：数字小的先执行
  - 目标方法后的通知方法：数字小的后执行
***
## 切入点表达式
### 1. execution:
  -  execution主要根据方法的返回值、包名、类名、方法名、方法参数等信息来匹配，语法为:
  execution ( **访问修饰符**  返回值类型  **包名.类名** .方法名(方法参数) **throws 异常** )  
  -  其中加粗的部分可以省略 
    1、访问修饰符：可省略（比如：public、protected） 
    2、包名.类名：可省略 
    3、throws 异常：可省略（注意是方法上声明抛出的异常，不是实际抛出的异常） 
  - 可以使用通配符描述切入点 
    1、* ： 单个独立的任意符号，可以通配任意返回值、包名、类名、方法名、任意类型的一个参数，也可以通配包、类、方法名的一部分
  ``` execution(* com.*.service.*.update*(*)) ``` 
    2、.. ：多个连续的任意符号，可以匹配任意层级的包、或任意类型、任意个数的参数
  ``` execution(* com.xxxyjade17..DeptService.*(..)) ``` 
### 2. @Annotation
  - 定义注解
```
@Target(ElementType.METHOD) // 此注解可用于方法
@Retention(RetentionPolicy.RUNTIME) // 在运行时保留此注解
public @interface LogOperation {
}
```
  - 切面类中标记切入点
```
@Before("@annotation(com.itheima.anno.LogOperation)")
public void before(){
    log.info("MyAspect -> before ...");
}
```
  - 在Service层加上注解
```
@LogOperation
@Override
public void delete(Integer id) {
    deptMapper.delete(id);
}
```
### 3, @Pointcut切入点
```
@Pointcut( 切入点表达式)
public void pt(){}
```
***
## Joinpoint连接点
  - 对于 @Around 通知，获取连接点信息只能使用 ProceedingJoinPoint
```
@Around("execution(* com.itheima.service.DeptService.*(..))")
public Object around(ProceedingJoinPoint joinPoint) throws Throwable {
    String className = joinPoint.getTarget().getClass().getName(); // 获取目标类名
    Signature signature = joinPoint.getSignature(); // 获取目标方法签名
    String methodName = joinPoint.getSign
    Object[] args = joinPoint.getArgs(); 
    Object res = joinPoint.proceed(); // 
    return res;
}
```
  - 对于其它四种通知，获取连接点信息只能使用 JoinPoint ，它是 ProceedingJoinPoint 的父类型
```
@Before("execution(* com.itheima.service.DeptService.*(..))")
public void before(JoinPoint joinPoint) {
    String className = joinPoint.getTarget().getClass().getName(); // 获取目标类名

    Signature signature = joinPoint.getSignature(); // 获取目标方法签名
    String methodName = joinPoint.getSignature().getName(); // 获取目标方法名

    Object[] args = joinPoint.getArgs(); // 获取目标方法运行参数
}
```
