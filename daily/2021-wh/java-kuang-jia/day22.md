---
description: '2021-02-15'
---

# day22

## 完成情况

每一天都学习并记录


每天看至少3个leetcode的题目


## 学习内容

### Spring

* Spring是分层的JavaSE/EE应用 full-stack轻量级开源框架
* 底层是核心容器
  * Beans
  * Core
  * Context
  * SpEL
* 中间层技术
  * AOP
  * Aspects
  * Instrumentation
  * Messaging
* 应用层技术
  * Data Access/Integration
    * JDBC
    * ORM
    * OXM
    * JMS
    * Transactions
  * Web
    * WebSocket
    * Servlet
    * Web
    * Portlet
* Test层
* Spring
  * 方便解耦，简化开发
  * 方便集成各种优秀的框架
  * 方便程序的测试
  * **AOP编程的支持**
  * 声明式事务的支持
  * 降低JavaEE API的使用难度
  * **Java源码是经典学习范例**

#### IoC

耦合：代码书写过程中所使用的技术的结合紧密度，用于衡量软件中各个模块之间的互联程度

内聚：代码书写过程中单个模块内部各组成部分间的联系，用于衡量软件各个功能模块内部的功能联系

程序书写的目标：高内聚，低耦合

* 就是同一个模块的各个元素之间要高度紧密，但是各个模块之间的相互依存不要那么紧密

工厂模式

实现配置和资源的耦合

IoC（Inversion Of Control） 控制反转：Spring反向控制应用程序所需要的外部资源

IoC容器

**IoC入门案例**

* 导入Spring坐标

  ```text
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.1.9.RELEASE</version>
  </dependency>
  ```

* 编写业务层与表现层接口与实现类

  ```text
  public interface UserService {
      public void save();
  }
  ```

  ```text
  public class UserServiceImpl implements UserService {
      @Override
      public void save() {
          System.out.println("user service running...");
      }
  }
  ```

* 建立spring配置文件
* 配置所需资源为spring控制的资源

  ```text
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd">
  ​
  <!--    创建spring控制的资源-->
      <bean id="userService" class="pro.doublez.service.impl.UserServiceImpl">
  ​
      </bean>
  ​
  </beans>
  ```

* 表现层通过spring获取资源

  ```text
  public static void main(String[] args) {
      //加载配置文件
      ApplicationContext ctx=new ClassPathXmlApplicationContext("applicationContext.xml");
      UserService userService = (UserService) ctx.getBean("userService");
      userService.save();
  }
  ```

**IoC配置**

bean

* bean可以定义name，取多个名称

  ```text
  <bean id="userService" name="userService1,userService2" class="pro.doublez.service.impl.UserServiceImpl"/>
  ```

* bean属性scope
  * 控制bean创建出来后的对象是否是单例的
  * singleton：默认就是单例的bean （加载spring容器的时候就创建）
  * prototype：不是单例的bean （在获取对象的时候创建，获取一次创建一次）
* bean的生命周期
  * init-method
    * 单例的bean，加载spring容器的时候执行
    * 不是单例的bean，创建对象的时候执行
  * destroy-method
    * 单例的bean，销毁spring容器的时候执行
    * 不是单例的bean，对象的销毁由垃圾回收机制gc\(\)控制，destroy方法不会执行
* bean对象创建方式
  * 由静态工厂获得
    * factory-method
  * 由实例工厂获得
    * factory-bean
    * factory-method

#### DI

* 依赖注入（Dependency Injection）
* 应用程序运行依赖的资源由spring提供，资源进入应用程序的方式称为注入
* 应用程序依赖于spring提供的资源

**set注入**

```text
private UserDao userDao;
    //对需要进行注入的方法创建set方法
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }
    private int num;
    public void setNum(int num) {
        this.num = num;
    }
    private String version;
​
    public void setVersion(String version) {
        this.version = version;
    }
```

```text
<!--    创建spring控制的资源-->
    <bean id="userService" class="pro.doublez.service.impl.UserServiceImpl">
<!--        将要注入的引用类型的变量通过property属性注入，对应的name是属性名，ref是对应的bean的id-->
        <property name="userDao" ref="userDao"/>
        <property name="num" value="666"/>
        <property name="version" value="spring5"/>
    </bean>
<!--    将要注入的资源声明为bean-->
    <bean id="userDao" class="pro.doublez.dao.impl.UserDaoImpl"/>
```

**构造器注入**

用构造方法注入

```text
<constructor-arg ref="userDao"/>
```

**集合注入**

* array
* list
* set
* map
* props

```text
<bean id="bookDao" class="pro.doublez.dao.impl.BookDaoImpl">
    <property name="al">
        <list>
            <value>doublez</value>
            <value>666</value>
        </list>
    </property>
    <property name="properties">
        <props>
            <prop key="name">doublez666</prop>
            <prop key="value">666</prop>
        </props>
    </property>
    <property name="arr">
        <array>
            <value>666</value>
            <value>6666</value>
        </array>
    </property>
    <property name="hs">
        <set>
            <value>666</value>
            <value>66666</value>
        </set>
    </property>
    <property name="hm">
        <map>
            <entry key="name" value="zhangsan"/>
            <entry key="age" value="18"/>
        </map>
    </property>
</bean>
```

p命名空间：了解

SpEL：了解

**外部properties文件的导入**

```text
<!--加载context命名空间的支持--!>
xmlns:context="http://www.springframework.org/schema/context"
<!--导入--!>
<context:property-placeholder location="classpath:data.properties"/>
```

导入子配置文件

```text
<import resource="applicationContext-user.xml"/>
```

导入Druid

```text
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql:///day23?serverTimezone=Asia/Shanghai"/>
    <property name="username" value="root"/>
    <property name="password" value="aa2217117"/>
</bean>
```

