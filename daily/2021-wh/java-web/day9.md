---
description: '2021-02-02'
---

# day9

## 完成情况

每一天都学习并记录


每天看至少3个leetcode的题目


## 学习内容

### JDBC

Java DataBase Connectivity Java数据库连接

定义了操作所有关系型数据库的规则（接口）

各个关系型数据库厂商去实现这套接口，提供数据库驱动jar包，我们可以使用这套JDBC接口编程，真正执行的代码是jar包中的实现类

```java
public class JDBCDemo {
    public static void main(String[] args) throws Exception {
        //连接数据库，获得Connection对象
        String url = "jdbc:mysql://localhost:3306/school?serverTimezone=UTC";
        String username = "root";
        String password = "aa2217117";
        Connection conn = DriverManager.getConnection(url, username, password);

        //创建执行sql语句的对象
        Statement st = conn.createStatement();
        PreparedStatement pst = conn.prepareStatement("");
        //管理事务
        conn.setAutoCommit(false);  //关闭自动提交，即开启事务
        conn.commit(); //提交事务
        conn.rollback(); //回滚

        //Statement对象
        st.execute("");  //可以执行任意的sql语句
        st.executeUpdate("");  //执行DML语句（增删改表中的数据）和DDL（对数据库和表执行操作），返回值是int（影响的行数）
        st.executeQuery("");  //执行DQL语句（select）
    }
}
```

#### Statement对象

executeUpdate方法

```java
public class StatementDemo01 {
    public static void main(String[] args) {

        String url = "jdbc:mysql://localhost:3306/school?serverTimezone=UTC";
        String username = "root";
        String password = "aa2217117";

        String sql1 = "insert into students values(11,3,'小李','B',91)";
        String sql2 = "update students set class_id=2 where id=11";
        String sql3 = "delete from students where id=10";

        Connection conn = null;
        Statement st = null;

        try {
            conn = DriverManager.getConnection(url, username, password);
            st = conn.createStatement();

            int count1 = st.executeUpdate(sql1);
            if (count1 > 0) {
                System.out.println("添加成功");
            } else {
                System.out.println("添加失败");
            }

            int count2 = st.executeUpdate(sql2);
            if (count2 > 0) {
                System.out.println("修改成功");
            } else {
                System.out.println("修改失败");
            }

            int count3 = st.executeUpdate(sql3);
            if (count3 > 0) {
                System.out.println("删除成功");
            } else {
                System.out.println("删除失败");
            }

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (st != null) {
                try {
                    st.close(); //释放资源
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }

            if (conn != null) {
                try {
                    conn.close(); //释放资源
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
        }

    }
}
```

executeQuery方法，返回的是ResultSet

```java
public class StatementDemo02 {
    public static void main(String[] args) {

        String url = "jdbc:mysql://localhost:3306/school?serverTimezone=UTC";
        String username = "root";
        String password = "aa2217117";
        String sql = "select * from students";
        Connection conn = null;
        Statement st = null;
        ArrayList<Student> students = new ArrayList<>();
        try {
            conn = DriverManager.getConnection(url, username, password);
            st = conn.createStatement();
            ResultSet result = st.executeQuery(sql);

            //遍历查询并存入集合中
            while (result.next()) {
                students.add(new Student(result.getInt(1), result.getInt(2), result.getString(3), result.getString(4), result.getInt(5)));
            }
            //遍历集合并输出
            for (Student s : students) {
                System.out.println(s.getId() + "\t" + s.getClass_id() + "\t" + s.getName() + "\t" + s.getGender() + "\t" + s.getScore());
            }

            result.close();

        } catch (Exception e) {
            e.printStackTrace();
        } finally {


            if (st != null) {
                try {
                    st.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }

            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
        }
    }
}
```

#### JDBC工具类

自己写一个类避免重复操作

```java
/**
 * JDBC工具类
 */
public class MysqlDemo {
    private static String url;
    private static String username;
    private static String password;

    //文件读取
    static {
        try {
            Properties properties = new Properties();
            properties.load(new FileReader("src\\day9\\study2\\jdbc.properties"));
            url = properties.getProperty("url");
            username = properties.getProperty("username");
            password = properties.getProperty("password");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    //返回Connection对象
    public static Connection getConnection() {
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        Connection conn = null;
        try {
            conn = DriverManager.getConnection(url, username, password);
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        return conn;
    }

    //返回Statement对象
    public static Statement getStatement(Connection conn) {
        Statement st = null;
        try {
            st = conn.createStatement();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        return st;
    }

    //返回ResultSet对象
    public static ResultSet getResultSet(Statement st, String sql) {
        ResultSet res = null;
        try {
            res = st.executeQuery(sql);
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        return res;
    }


    //释放资源
    public static void close(Connection conn, Statement st) {
        if (st != null) {
            try {
                st.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }

    //释放资源
    public static void close(Connection conn, Statement st, ResultSet res) {
        if (res != null) {
            try {
                res.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }

        if (st != null) {
            try {
                st.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }
}
```

```java
public class MysqlTest {
    public static void main(String[] args) throws SQLException {

        ArrayList<Student> students = new ArrayList<>();
        String sql = "select * from students";
        Connection conn = MysqlDemo.getConnection();
        Statement st = MysqlDemo.getStatement(conn);
        ResultSet result = MysqlDemo.getResultSet(st, sql);

        //遍历查询并存入集合中
        while (result.next()) {
            students.add(new Student(result.getInt(1), result.getInt(2), result.getString(3), result.getString(4), result.getInt(5)));
        }
        //遍历集合并输出
        for (Student s : students) {
            System.out.println(s.getId() + "\t" + s.getClass_id() + "\t" + s.getName() + "\t" + s.getGender() + "\t" + s.getScore());
        }

        MysqlDemo.close(conn, st, result);
    }
}
```

验证登录

```java
public class LoginDemo01 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入用户名");
        String username = sc.nextLine();
        System.out.println("请输入密码");
        String password = sc.nextLine();
        if (new LogInDemo().login(username, password)) {
            System.out.println("登录成功");
        } else {
            System.out.println("用户名或密码错误");
        }
    }


    public boolean login(String username, String password) {
        if (username == null || password == null) {
            return false;
        } else {
            Connection conn = MysqlDemo.getConnection();
            String sql = "select * from user where username='" + username + "' and password='" + password + "'";
            Statement st = MysqlDemo.getStatement(conn);
            ResultSet res = MysqlDemo.getResultSet(st, sql);
            boolean isTrue = false;
            try {
                isTrue = res.next();
            } catch (SQLException th) {
                th.printStackTrace();
            }
            MysqlDemo.close(conn, st, res);
            return isTrue;
        }
    }
}
```

但是这样子会导致 SQL注入的问题

* 在拼接SQL时，有一些特殊的SQL的特殊关键字参与拼接，会造成安全性的问题
* 解决方法：PreparedStatement对象

PrepareStatement：预编译SQL

* 参数使用？作为占位符
* 如   select \* from user where username=? and password =?;
* 以后都会使用这个，而不使用Statement

```java
public class LoginDemo02 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入用户名");
        String username = sc.nextLine();
        System.out.println("请输入密码");
        String password = sc.nextLine();
        if (new LoginDemo01().login(username, password)) {
            System.out.println("登录成功");
        } else {
            System.out.println("用户名或密码错误");
        }
    }


    public boolean login(String username, String password) {
        if (username == null || password == null) {
            return false;
        } else {
            Connection conn = MysqlDemo.getConnection();
            String sql = "select * from user where username= ? and password= ? ";
            PreparedStatement past = MysqlDemo.getPreparedStatement(conn, sql);
            try {
                past.setString(1, username);
                past.setString(2, password);
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }

            ResultSet res = MysqlDemo.getResultSet(past);
            boolean isTrue = false;
            try {
                isTrue = res.next();
            } catch (SQLException th) {
                th.printStackTrace();
            }
            MysqlDemo.close(conn, past, res);
            return isTrue;
        }
    }
}
```

**事务管理**

转账事务

```sql
create table account(
    id int primary key auto_increment,
    name varchar(10),
    balance double(20,2)
);


insert into account values (null,'张三',1000);
insert into account values (null,'李四',1000);

select * from account;
```

```java
public class TransferAccountsDemo {
    public static void main(String[] args) {
        String sqlSub = "update account set balance = balance- ? where id= ? ";
        String sqlAdd = "update account set balance = balance+ ? where id= ? ";
        Connection conn = MysqlDemo.getConnection();
        PreparedStatement ps1 = MysqlDemo.getPreparedStatement(conn, sqlSub);
        PreparedStatement ps2 = MysqlDemo.getPreparedStatement(conn, sqlAdd);
        try {
            //开启事务
            conn.setAutoCommit(false);

            ps1.setDouble(1, 500);
            ps1.setInt(2, 1);
            ps2.setDouble(1, 500);
            ps2.setInt(2, 2);
            ps1.executeUpdate();
            ps2.executeUpdate();

            //提交事务
            conn.commit();
        } catch (Exception e) {
            e.printStackTrace();
            try {
                //回滚事务
                conn.rollback();
            }
             catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        MysqlDemo.close(conn, ps1);
        MysqlDemo.close(conn, ps2);
    }
}
```

#### 数据库连接池

系统初始化之后，容器被创建，容器中会申请一些连接对象，当数据库访问数据库时，会申请对象，用完之后返还给容器

* 标准接口 DataSource

  获取连接：getConnection\(\);

  归还连接：close\(\),如果Conncetion对象从连接池获取，不再是关闭连接，而是归还

* 一般我们不需要去实现，由数据库厂商实现
* C3P0：数据库连接池技术
* Druid：数据库连接池实现技术，由阿里巴巴提供

**C3P0**

导入jar包

c3p0-0.9.5.2.jar和mchange-commons-java-0.2.12.jar

定义配置文件

c3p0.properties 或 c3p0-config.xml 放在src目录下

```html
<?xml version="1.0" encoding="utf-8"?>
<c3p0-config>
  <default-config>
    <property name="driverClass">com.mysql.cj.jdbc.Driver</property>
    <property name="jdbcUrl">jdbc:mysql://localhost:3306/school?serverTimezone=UTC</property>
    <property name="user">root</property>
    <property name="password">aa2217117</property>

<!--    当连接池池初始化时能建立的物理连接的数目-->
    <property name="initialPoolSize">5</property>
<!--    连接池池能包含的物理连接的最大数目-->
    <property name="maxPoolSize">10</property>
<!--    超时时间-->
    <property name="checkoutTimeout">3000</property>
  </default-config>

<!--  自定义的配置，在创建连接池的时候传入name即可-->
  <named-config name="doublez">
    <property name="driverClass">com.mysql.cj.jdbc.Driver</property>
    <property name="jdbcUrl">jdbc:mysql://localhost:3306/school?serverTimezone=UTC</property>
    <property name="user">doublez</property>
    <property name="password">aa2217117</property>

    <property name="initialPoolSize">6</property>
    <property name="maxPoolSize">66</property>
    <property name="checkoutTimeout">666</property>
  </named-config>
</c3p0-config>
```

```java
public class C3P0Demo {
    public static void main(String[] args) throws SQLException {
        DataSource ds = new ComboPooledDataSource();
        Connection conn = ds.getConnection();
    }
}
```

**Druid**

导入jar包

druid-1.1.22.jar

定义配置文件

properties形式，任意名称，任意目录

```text
url=jdbc:mysql://localhost:3306/school?serverTimezone=UTC
username=root
password=aa2217117
initialSize=10
maxActive=20
maxWait=1000
filters=wall
```

```java
public class DruidDemo {
    public static void main(String[] args) throws Exception {
        Properties pro = new Properties();
        InputStream is = DruidDemo.class.getClassLoader().getResourceAsStream("druid.properties");
        pro.load(is);
        System.out.println(pro);
        DataSource ds = DruidDataSourceFactory.createDataSource(pro);
        Connection conn = ds.getConnection();
        System.out.println(conn);
    }
}
```

```java
public class DruidDemo02 {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement pstmt = null;
        try {
            conn = JDBCUtils.getConnection();
            String sql = "insert into account values(null,?,?)";
            pstmt = conn.prepareStatement(sql);
            pstmt.setString(1, "王五");
            pstmt.setDouble(2, 1000);
            int count = pstmt.executeUpdate();
            System.out.println(count);
            JDBCUtils.close(pstmt, conn);
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
    }
}
```

#### Spring JDBC

Spring框架对JDBC的简单封装

JDBC Template

* 导入jar包
* 创建JdbcTemplate对象，依赖于数据源DataSource

  JdbcTemplate template = new JbdcTemplate\(ds\);

* 调用方法

```java
public class JDBCTemplateDemo01 {
    public static void main(String[] args) {
        JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
        String sql = "update account set balance = 5000 where id= ? ";
        int count = template.update(sql, 3);
        System.out.println(count);
    }
}
```

```java
public class JDBCTemplateDemo02 {
    private final JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());

    @Test
    public void test1() {
        //修改5号学生的成绩为99
        template.update("update school.students set score=? where id=?", 99, 5);
    }

    @Test
    public void test2() {
        //添加一条记录
        template.update("insert into school.students values(null,?,?,?,?)", 2, "小兵", "B", 88);
    }

    @Test
    public void tesT3() {
        //删除刚刚添加的记录
        template.update("delete from school.students where name=?", "小兵");
    }

    @Test
    public void test4() {
        //查询id为1的记录，封装为Map集合
        //查询的结果集长度只能是1
        Map<String, Object> map = template.queryForMap("select * from school.students where id=?", 1);
        System.out.println(map);
    }

    @Test
    public void test5() {
        //查询所有记录，封装为List集合
        List<Map<String, Object>> list = template.queryForList("select * from school.students");
        for (Map<String, Object> stringObjectMap : list) {
            System.out.println(stringObjectMap);
        }
    }

    @Test
    public void test6() {
        //查询所有记录，封装为Students对象的List集合
        // BeanPropertyRowMapper 自动封装，要求类要有默认构造函数
        List<Student> list = template.query("select * from school.students", new BeanPropertyRowMapper<>(Student.class));
        for (Student student : list) {
            System.out.println(student);
        }
    }

    @Test
    public void tesT7() {
        //查询总记录数
        Long total = template.queryForObject("select count(id) from school.students", long.class);
        System.out.println(total);
    }
}
```

