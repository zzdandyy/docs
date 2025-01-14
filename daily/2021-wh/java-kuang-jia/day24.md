---
description: '2021-02-17'
---

# day24

## 完成情况

每一天都学习并记录

每天看至少3个leetcode的题目

## 学习内容

### AOP

动态代理技术

* JDK代理，基于接口的动态代理技术

  ```java
  public class proxy {
      public static void main(String[] args) {

          Target target = new Target();

          Advice advice=new Advice();

          TargetInterface proxy = (TargetInterface) Proxy.newProxyInstance(target.getClass().getClassLoader(), target.getClass().getInterfaces(), new InvocationHandler() {
              @Override
              public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                  advice.before();
                  Object invoke = method.invoke(target, args);
                  advice.afterReturning();
                  return invoke;
              }
          });

          proxy.show();
      }
  }
  ```

* cglib代理，基于父类的动态代理技术

  ```java
  public class proxy {
      public static void main(String[] args) {

          Target target = new Target();

          Advice advice = new Advice();

          //基于cglib的动态代理
          //1.创建增强器
          Enhancer enhancer = new Enhancer();
          //2.设置父类（目标）
          enhancer.setSuperclass(Target.class);
          //3.设置回调
          enhancer.setCallback(new MethodInterceptor() {
              @Override
              public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
                  //执行前置
                  advice.before();
                  //执行目标
  //                Object invoke = method.invoke(target, args);
                  Object invoke = methodProxy.invokeSuper(proxy, args);
                  //执行后置
                  advice.afterReturning();
                  return invoke;
              }
          });
          //4.创建代理对象
          Target proxy = (Target) enhancer.create();
          //5.测试
  //        System.out.println(proxy);
          proxy.show();
      }
  }
  ```

AOP相关概念

* Target（目标对象）：代理的目标对象
* Proxy（代理）：结果代理类
* Joinpoint（连接点）：可以被增强的方法，即被拦截到的点
* Point（切入点）：要对那些Joinpoint进行拦截的定义（要增强那些方法）
* Advice（通知/增强）：拦截到的Joinpoint之后要做的事情
* Aspect（切面）：切点与通知的结合叫切面
* Weaving（织入）：把增强应用到目标对象来创建新的代理对象的过程，spring采用动态代理织入

AOP的开发过程

* 编写核心业务代码
* 编写切面类，切面类中有通知
* 在配置文件中，配置织入关系，即哪些通知与哪些连接点进行结合

AOP实现过程

* spring监控切入点方法的执行
* 监控到切入点的方法被执行，使用代理机制动态创建，将通知织入

AOP使用哪种代理方式

* JDK：目标对象有接口默认使用JDK
* cglib：目标对象没有接口使用cglib

#### XML配置

```html
<!--    配置织入-->
    <aop:config>
        <aop:aspect ref="myAspect">
<!--            <aop:before method="before()" pointcut="execution(public void pro.doublez.aop.Target.show())"/>-->
            <aop:before method="before()" pointcut="execution(* pro.doublez.aop.*.*(..))"/>
        </aop:aspect>
    </aop:config>
```

* 前置通知：before
* 后置通知：after-returning
* 环绕通知：arount
* 异常抛出通知：after-throwing
* 最终通知：after

#### 注解配置

```java
@Configuration
@ComponentScan(value = "pro.doublez")
@PropertySource("classpath:jdbc.properties")
@EnableAspectJAutoProxy  //aop自动代理
@Import({JDBCConfig.class,
        MyBatisConfig.class,
//        CustomerImportSelector.class,
//        MyBeanFactory.class,
//        MyBean.class
        })
public class SpringConfig {

}
```

```java
@Component("myAspect")
@Aspect //标注当前MyAspect是一个切面类
public class MyAspect {

    @Pointcut("execution(* pro.doublez.aop.Target.*(..))")
    public void pointcut() {
    }

    @Before("pointcut()")
    public void before() {
        System.out.println("前置增强");
    }

    @AfterReturning("pointcut()")
    public void afterReturning() {
        System.out.println("后置增强");
    }


    //Proceeding JoinPoint 正在执行的连接点（切点）
    @Around("pointcut()")
    public Object around(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("环绕前增强");
        Object proceed = pjp.proceed();
        System.out.println("环绕后增强");
        return proceed;
    }

    @AfterThrowing("pointcut()")
    public void afterThrowing() {
        System.out.println("异常抛出后增强");
    }

    @After("pointcut()")
    public void after() {
        System.out.println("最终增强");
    }
}
```

**AOP底层原理**

* 静态代理
  * 装饰者模式：在不惊动原始设计的基础上。为其添加功能
* JDK代理
  * Proxy
  * 依赖接口
* cglib代理
  * Code Generation Library，Code生成类库
  * 不依赖接口
* 织入形式
  * 织入时机
    * 编译期：速度快，灵活性差，编译即锁定
    * 加载期：速度快，灵活性中，多次加载可变更实现
    * 运行期：运行速度慢，灵活性强，每次运行均可改变实现（spring）

### 事务

* 保障数据库即使在异常状态下也能保持一致
* 并发访问数据库时，在多个访问相互隔离，防止互相干扰
* 事务特征
  * 原子性：事务是一个不可分的整体
  * 一致性：事务前后的数据的完整性必须保持一致
  * 隔离性：数据库为每一个用户开启的事务，不能被其他事务干扰，并发事务相互隔离
  * 持久性：事务一旦提交，对数据的改变就是永久性的
* 事务隔离级
  * 脏读：运行读取未提交的数据
    * 原因：Read uncommitted
    * 解决方案：Read committed（表级读锁）
  * 不可重复读：读取过程中单个数据发生了变化
    * 解决方案：Repeatable read（行级写锁）
  * 幻读：读取过程中数据条目发生了变化
    * 解决方案：Serializable（表级写锁）

#### Spring事务核心对象

* PlatFormTransactionManager 平台事务管理器
  * DataSourceTransactionManager
* TransactionDefinition  事务定义
* TransactionStatus 事务状态

**事务管理**

* 编程式事务

  ```java
  @Override
      public void transfer(String outName, String inName, double money) {
          //开启事务
          PlatformTransactionManager ptm = new DataSourceTransactionManager(dataSource);
          //事务定义
          TransactionDefinition td = new DefaultTransactionDefinition();
          //事务状态
          TransactionStatus ts = ptm.getTransaction(td);

          accountDao.inMoney(inName, money);
  //        int i=1/0;
          accountDao.outMoney(outName, money);

          //提交事务
          ptm.commit(ts);
      }
  ```

* 编程使用aop处理事务

  ```java
  @Around("execution(void pro.doublez.service.impl.AccountServiceImpl.transfer(..))")
  public Object transactionManager(ProceedingJoinPoint pjp) throws Throwable {
      //开启事务
      PlatformTransactionManager ptm = new DataSourceTransactionManager(dataSource);
      //事务定义
      TransactionDefinition td = new DefaultTransactionDefinition();
      //事务状态
      TransactionStatus ts = ptm.getTransaction(td);
      //执行事务
      Object proceed = pjp.proceed(pjp.getArgs());
      //提交事务
      ptm.commit(ts);
      return proceed;
  }
  ```

* 声明式事务（xml）
* 事务传播行为
  * 事务管理员
  * 事务协调员
  * 事务协调员对管理员所携带的事务的态度
    * REQUIRED：如果管理员有事务，就加入事务，无就新建事务
    * REQUIRES\_NEW：无论管理员是否有事务，都新建事务
    * SUPPORTS：如果管理员有事务，就加入事务，无就没有事务
    * NOT\_SUPPORTED：无论管理员有没有事务都没有事务
    * MANDATORY：如果管理员有事务就加入，无就报错
    * NEVER：如果管理员有事务就报错，无就没有事务
    * NESTED：设置回滚点
* 声明式事务（注解）

  ```java
  @EnableTransactionManagement()
  ```

  ```java
  @Bean(name = "txManager1")
  public PlatformTransactionManager getTransactionManager(DataSource dataSource){
      return new DataSourceTransactionManager(dataSource);
  }
  ```

  ```java
  @Transactional
  ```

  **模板对象**

  jdbcTemplate

  RedisTemplate

  * 客户端基本操作

    ```java
    redisTemplate.type();
    redisTemplate.persist();
    redisTemplate.move();
    redisTemplate.hasKey();
    redisTemplate.getExpire();
    redisTemplate.expire();
    redisTemplate.delete();
    redisTemplate.rename();
    ```

  * Operation对象

    ```java
    redisTemplate.opsForValue();
    redisTemplate.opsForHash();
    redisTemplate.opsForList();
    redisTemplate.opsForSet();
    redisTemplate.opsForZSet();
    ```

  * Bound Operation 阻塞式对象
  * 其他
    * 主从
    * 集群

      ```java
      redisTemplate.slaveOf(, );
      redisTemplate.slaveOfNoOne();

      redisTemplate.opsForCluster();
      ```

**事务的底层原理**

* 策略模式：使用不同策略的对象实现不同的行为方式，策略对象的变化导致行为的变化
* 装饰模式：？传参数，name传参数

