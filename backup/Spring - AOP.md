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
