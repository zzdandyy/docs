---
description: '2021-02-14'
---

# day21

## 完成情况

每一天都学习并记录


每天看至少3个leetcode的题目


## 学习内容

### redis

redis是一款高性能的NOSQL系列的非关系型数据库

关系型数据库

* 数据之间有关联关系
* 数据存储在硬盘的文件上

非关系型数据库

* 存键值对，数据之间没有关联关系
* 数据存储在内存中

redis.windows.conf 配置文件

redis-cli.exe 客户端

redis-server.exe 服务器端

* redis-server redis.windows.conf
* redis-server --service-install redis.windows.conf

卸载服务：redis-server --service-uninstall 开启服务：redis-server --service-start 停止服务：redis-server --service-stop

redis数据结构

key,value格式的数据，key是字符串，value有五种不同的数据结构

* 字符串类型：string
  * set key value
  * get key
  * del key
* 哈希类型：hash（HashMap）
  * hset key field vallue
  * hget key field 
  * hgetall key  获取所有
  * hedl key field
* 列表类型：list（LinkedList）（队列）
  * 可以添加一个元素到列表的头部或尾部
  * lpush key value
  * rpush key vlaue
  * lrange key start end 范围获取
  * lpop key 删除列表最左边的元素并返回
  * rpop key 删除列表最右边的元素并返回
* 集合类型：set
  * 不允许重复元素
  * sadd key value
  * smembers key 获取所有元素
  * srem key value 删除某个元素
* 有序集合类型：sortedset
  * zadd key score value 存储数据以及数据的分数
  * zrange key start end
  * zrem key value
* 通用命令
  * keys \*  查询所有的键
  * type key：查找键对应的值的类型
  * del key：删除指定的key value

持久化

* redis是一个内存数据库，当服务器重启，数据会丢失
* 可以进行持久化操作，把数据保存到硬盘的文件中
* 持久化机制
  * RDB：默认方式，不需要配置
    * 在一定的间隔时间中，检测key的变化情况，然后持久化数据
    * 编辑redis.windows.conf文件
    * 
  * AOF（不推荐）
    * 日子记录的方式，可以记录每一条命令的操作，可以每一次命令操作后持久化数据
    * 开启：appendonly no
    * appendfsync always 一直持久化

      appendfsync everysec 每隔一秒持久化

      appendfsync no 不持久化

#### Jedis

* 一款java操作redis数据库的工具
* 字符串类型：string
  * set key value
  * get key
  * del key
* 哈希类型：hash（HashMap）
  * hset key field vallue
  * hget key field 
  * hgetall key  获取所有
  * hedl key field
* 列表类型：list（LinkedList）（队列）
  * 可以添加一个元素到列表的头部或尾部
  * lpush key value
  * rpush key vlaue
  * lrange key start end 范围获取
  * lpop key 删除列表最左边的元素并返回
  * rpop key 删除列表最右边的元素并返回
* 集合类型：set
  * 不允许重复元素
  * sadd key value
  * smembers key 获取所有元素
  * srem key value 删除某个元素
* 有序集合类型：sortedset
  * zadd key score value 存储数据以及数据的分数
  * zrange key start end
  * zrem key value

```java
public class JedisTest {
    //快速入门

    @Test
    public void test1() {
        //获取连接
        Jedis jedis = new Jedis("localhost", 6379);
        //操作
        jedis.set("username", "zhangsan");
        //关闭连接
        jedis.close();
    }

    @Test
    public void test2() {
        //如果是空参数，默认值是"localhost",6379"
        Jedis jedis = new Jedis();
        //存储
        jedis.set("username", "zhangsan");
        //获取
        String username = jedis.get("username");
        System.out.println(username);
        //可以使用setex()方法指定有过期时间的key，value
        //如有时限的激活码
        jedis.setex("active", 20, "hehe");
        jedis.close();
    }

    @Test
    public void test3() {
        //如果是空参数，默认值是"localhost",6379"
        Jedis jedis = new Jedis();
        //存储
        jedis.hset("user", "name", "lisi");
        jedis.hset("user", "age", "23");
        jedis.hset("user", "gender", "male");
        //获取
        String name = jedis.hget("user", "name");
        System.out.println(name);
        Map<String, String> user = jedis.hgetAll("user");

        Set<String> keySet = user.keySet();
        for (String key : keySet) {
            String value = user.get(key);
            System.out.println(key + "," + value);
        }
        jedis.close();
    }

    @Test
    public void test4() {
        //如果是空参数，默认值是"localhost",6379"
        Jedis jedis = new Jedis();
        //存储
        jedis.lpush("mylist", "a", "b", "c");
        jedis.rpush("mylist", "a", "b", "c");
        //获取
        List<String> mylist = jedis.lrange("mylist", 0, -1);
        System.out.println(mylist);
        //list 弹出
        String element1 = jedis.lpop("mylist");
        System.out.println(element1);
        String element2 = jedis.rpop("mylist");
        System.out.println(element2);
        jedis.close();
    }

    @Test
    public void test5() {
        //如果是空参数，默认值是"localhost",6379"
        Jedis jedis = new Jedis();
        //存储
        jedis.sadd("myset", "java", "php", "c++");
        //获取
        Set<String> myset = jedis.smembers("myset");
        System.out.println(myset);
        jedis.close();
    }

    @Test
    public void test6() {
        //如果是空参数，默认值是"localhost",6379"
        Jedis jedis = new Jedis();
        //存储
        jedis.zadd("mysortedset", 60, "shangsan");
        jedis.zadd("mysortedset", 50, "lisi");
        jedis.zadd("mysortedset", 80, "wangwu");

        //获取
        Set<String> mysortedset = jedis.zrange("mysortedset", 0, -1);
        System.out.println(mysortedset);
        jedis.close();
    }
}
```

Jedis连接池:JedisPool

* 创建JedisPool连接池对象
* 调用方法 getResource\(\) 连接

```java
public class JedisPoolUtils {
    private static JedisPool jedisPool;

    static {
        //读取配置文件
        InputStream is = JedisPoolUtils.class.getClassLoader().getResourceAsStream("jedis.properties");
        Properties pro = new Properties();
        try {
            pro.load(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
        //获取数据
        JedisPoolConfig config = new JedisPoolConfig();
        config.setMaxIdle(Integer.parseInt(pro.getProperty("maxIdle")));
        config.setMaxTotal(Integer.parseInt(pro.getProperty("maxTotal")));
        //初始化连接池对象
        jedisPool = new JedisPool(config, pro.getProperty("host"), Integer.parseInt(pro.getProperty("port")));
    }

    public static Jedis getJedis() {
        return jedisPool.getResource();
    }
}
```

### Maven

项目管理工具

* 管理第三方jar包
* 项目生命周期的管理，编译，测试，打包，部署，运行
* 分模块构建，提高开发效率

### MyBatis

ORM 对象关系映射（Object Relational Mapping）

原始的JDBC的问题

* 频繁的创建和销毁数据库的连接会影响系统的性能（连接池）
* sql语句在代码中编码，不容易维护和修改
* 查询操作时，需要手动将结果封装到实体对象中
* 增删改查需要参数的时候，需要手动将实体对象的数据设置到sql的占位符

MyBatis封装了JDBC，通过xml或注解，最终形成sql语句

执行完sql之后可以返回Java对象，采用ORM思想，实现持久化的操作

核心配置

```html
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="mysql">
        <environment id="mysql">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql:///day21?serverTimezone=Asia/Shanghai"/>
                <property name="username" value="root"/>
                <property name="password" value="aa2217117"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="StudentMapper.xml"/>
    </mappers>
</configuration>
```

mapper配置

```html
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="StudentMapper">
    <select id="selectAll" resultType="pro.doublez.bean.Student">
        SELECT *
        FROM day21.student
    </select>
</mapper>
```

```java
@Test
public void selectAll() throws Exception {
    //加载核心配置文件
    InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

    //获取SqlSession工厂对象
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

    //通过SqlSession工厂对象获得SqlSession对象
    SqlSession sqlSession = sqlSessionFactory.openSession();

    //执行映射文件配置中的sql语句，返回结果
    List<Student> list = sqlSession.selectList("StudentMapper.selectAll");

    //处理结果
    for (Student student : list) {
        System.out.println(student);
    }

    //释放资源
    sqlSession.close();
    is.close();
}
```

#### 相关API

Resources 加载核心配置文件

SqlSessionFactoryBuilder

* bulid：通过指定的资源字节输入流获得SqlSession工厂对象

SqlSessionFactory

* openSession：获取SqlSession对象，参数如果为true，就是自动提交事务

SqlSession

* 执行SQL
* 管理事务
* 接口代理
  * selectList
  * selectOne
  * insert
  * update
  * delete
  * commit
  * rollback
  * getMapper
  * close

映射配置文件

* mapper：根标签
  * namespace：名称空间
* select：查询功能的标签
  * id：唯一标识
  * resultType：映射对象类型
  * parameterType：指定参数映射的对象类型

```java
@Test
public void selectById() throws Exception{
    InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
    SqlSession sqlSession = sqlSessionFactory.openSession();
    Student s = sqlSession.selectOne("StudentMapper.selectById", 3);
    System.out.println(s);
    sqlSession.close();
    is.close();
}
@Test
public void insert() throws Exception{
    InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
    SqlSession sqlSession = sqlSessionFactory.openSession();
    Student student=new Student(4,"赵六",26);
    int count = sqlSession.insert("StudentMapper.insert", student);
    System.out.println(count);
    //提交事务
    sqlSession.commit();
    sqlSession.close();
    is.close();
}
@Test
public void update () throws Exception{
    InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
    SqlSession sqlSession = sqlSessionFactory.openSession();
    Student student=new Student(4,"张三",16);
    sqlSession.update("StudentMapper.update",student);
    sqlSession.commit();
    sqlSession.close();
    is.close();
}
@Test
public void delete() throws Exception{
    InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
    SqlSession sqlSession = sqlSessionFactory.openSession();
    sqlSession.delete("StudentMapper.delete",4);
    sqlSession.commit();
    sqlSession.close();
    is.close();
}
```

核心配置文件

* configuration 核心根标签
* environments 配置数据库环境
  * default属性指定使用哪一个
* transactionManager 事务管理
  * type 采用JDBC默认事务
* dataSource 数据源信息
  * type 采用POOLED 连接池
  * property：数据库连接池的配置信息
* mappers 映射文件
* 起别名

  ```html
  <typeAliases>
      <typeAlias type="pro.doublez.bean.Student" alias="Student"/>
  </typeAliases>
  ```

LOG4J

获得真正的sql语句和具体的参数

```html
#ERROR WARN INFO DEBUG
log4j.rootLogger=DEBUG, stdout

log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

#### 接口代理的方式实现Dao层

实现要求

映射配置文件的名称空间必须和Dao层接口全类名（带路径）相同

映射配置文件的id和Dao层接口的方法名相同

映射配置文件增删改查标签的parameterType属性与方法的参数相同

映射配置文件增删改查标签的resultType属性与方法的返回值相同

**动态SQL**

if

```html
<select id="selectCondition" resultType="Student" parameterType="Student">
    SELECT * FROM day21.student
    <where>
        <if test="id!=null">
            id=#{id}
        </if>
        <if test="age!=null">
            AND age=#{age}
        </if>
        <if test="name!=null">
            AND name=#{name}
        </if>
    </where>
</select>
```

foreach

```html
<select id="selectByIds" resultType="Student" parameterType="list">
    SELECT * FROM day21.student
    <where>
        <foreach collection="list" open="id IN(" close=")" item="id" separator=",">
            #{id}
        </foreach>
    </where>
</select>
```

SQL片段抽取

```html
<sql id="select">SELECT * FROM day21.student</sql>

<include refid="select"/>
```

#### 分页插件

```html
<plugins>
    <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
</plugins>
```

```java
@Test
public void selectByPage() throws Exception {
    InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
    SqlSession sqlSession = sqlSessionFactory.openSession();

    //分页插件
    PageHelper.startPage(1, 2);//第一页，每页2行

    List<Student> list = sqlSession.selectList("StudentMapper.selectAll");
    for (Student student : list) {
        System.out.println(student);
    }
    sqlSession.close();
    is.close();
}
```

```java
System.out.println("总条数：" + info.getTotal());
System.out.println("总页数：" + info.getPages());
System.out.println("当前页：" + info.getPageNum());
System.out.println("每页显示条数：" + info.getPageSize());
System.out.println("上一页：" + info.getPrePage());
System.out.println("下一页：" + info.getNextPage());
System.out.println("是否是第一页：" + info.isIsFirstPage());
System.out.println("是否是最后一页：" + info.isIsLastPage());
```

#### 多表查询

一对一

```html
<mapper namespace="pro.doublez.table01.OneToOneMapper">
    <resultMap id="oneToOne" type="card">
        <!--        配置字段和实体对象的映射关系-->
        <id column="cid" property="id"/>
        <result column="number" property="number"/>
        <!--        被包含对象的映射关系-->
        <association property="p" javaType="person">
            <id column="pid" property="id"/>
            <result column="name" property="name"/>
            <result column="age" property="age"/>
        </association>
    </resultMap>
    <select id="selectAll" resultMap="oneToOne">
        select c.id cid, number, pid, name, age
        from day21.card c,
             day21.person p
        where c.pid = p.id;
    </select>
</mapper>
```

一对多

```html
<mapper namespace="pro.doublez.table02.OneToManyMapper">
    <resultMap id="oneToMany" type="classes">
        <id column="cid" property="id"/>
        <result column="cname" property="name"/>
        <collection property="students" ofType="stu">
            <id column="sid" property="id"/>
            <id column="sname" property="name"/>
            <id column="sage" property="age"/>
        </collection>
    </resultMap>
    <select id="selectAll" resultMap="oneToMany">
        select c.id cid, c.name cname, s.id sid, s.name sname, s.age sage
        from day21.classes c,
             day21.stu s
        where c.id = s.cid;
    </select>
</mapper>
```

多对多

```html
<mapper namespace="pro.doublez.table03.ManyToManyMapper">
    <resultMap id="manyToMany" type="stu">
        <id column="sid" property="id"/>
        <result column="sname" property="name"/>
        <result column="sage" property="age"/>
        <collection property="courses" ofType="course">
            <id column="cid" property="id"/>
            <result column="cname" property="name"/>
        </collection>
    </resultMap>
    <select id="selectAll" resultMap="manyToMany">
        select sc.sid, s.name sname, s.age sage, sc.cid, c.name cname
        from day21.stu s,
             day21.course c,
             day21.stu_cr sc
        where sc.sid = s.id
          and sc.cid = c.id;
    </select>
</mapper>
```

#### 注解开发

映射配置文件的弊端

* 名称空间要和接口保持一致
* 接口中的方法参数返回值等等，要和映射配置文件中一致

常用的注解

* @Select\(SQL\)
* @Insert\(SQL\)
* @Update\(SQL\)
* @Delete\(SQL\)

**多表注解查询**

一对一

```java
public interface CardMapper {
    @Select("select * from card")
    @Results({
            @Result(column = "id", property = "id"),
            @Result(column = "number", property = "number"),
            @Result(
                    property = "p", //被包含对象的变量名
                    javaType = Person.class, //被包含对象的实际数据类型
                    column = "pid",  //根据查询出的pid来查询person表
                    one = @One(select = "pro.doublez.mapper.PersonMapper.selectById")

            )
    })
    public abstract List<Card> selectAll();
}
```

一对多

```java
public interface ClassesMapper {

    @Select("select * from classes")
    @Results({
            @Result(column = "id", property = "id"),
            @Result(column = "name", property = "name"),
            @Result(
                    property = "students",
                    javaType = List.class,
                    column = "id",
                    many = @Many(select = "pro.doublez.mapper.StuMapper.selectById")
            )
    })
    public abstract List<Classes> selectAll();
}
```

多对多

```java
@Select("select distinct s.id,s.name,s.age from stu s,stu_cr sc where sc.sid=s.id")
@Results({
        @Result(column = "id", property = "id"),
        @Result(column = "name", property = "name"),
        @Result(column = "age", property = "age"),
        @Result(
                property = "courses",
                javaType = List.class,
                column = "id",
                many = @Many(select = "pro.doublez.mapper.CourseMapper.selectBySid")
        )

})
public abstract List<Stu> selectAll();
```

```java
public interface CourseMapper {
    @Select("select c.id,c.name from stu_cr sc,course c where sc.cid=c.id and sc.sid=#{id}")
    public abstract List<Course> selectBySid(int id);
}
```

**构建SQL**

```java
public class Test01 {
    public static void main(String[] args) {
        System.out.println(getSql());
    }
    public static String getSql() {
        String sql = new SQL() {
            {
                SELECT("*");
                FROM("student");
            }
        }.toString();
        return sql;
    }
}
```

