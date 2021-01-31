---
description: '2021-01-31'
---

# day7

## 完成情况

{% hint style="success" %}
每一天都学习并记录
{% endhint %}

{% hint style="warning" %}
每天看至少3个leetcode的题目
{% endhint %}

## 学习内容

#### Stream流

```java
public class StreamDemo01 {
    public static void main(String[] args) {
        ArrayList<String> array = new ArrayList<>();
        array.add("大大怪");
        array.add("小小怪");
        array.add("小明");
        array.add("小王儿");
        array.add("大铭");
        array.add("大黎明");

        ArrayList<String> daArray = new ArrayList<>();
        for (String s : array) {
            if (s.startsWith("大")) {
                daArray.add(s);
            }
        }

        ArrayList<String> threeArray = new ArrayList<>();
        for (String s : daArray) {
            if (s.length() == 3) {
                threeArray.add(s);
            }
        }

        for (String s : threeArray) {
            System.out.println(s);
        }
        System.out.println("--------");
        //Stream流改写
        array.stream().filter(s -> s.startsWith("大")).filter(s -> s.length() == 3).forEach(System.out::println);
    }
}
```

**生成方式**

Collection体系的集合可以使用默认的方法Stream\(\)生成流

Map体系需要间接的生成流

数组可以通过Stream接口的静态方法of\(T...values\)生成流

```java
public class StreamDemo02 {
    public static void main(String[] args) {
//        Collection体系的集合可以使用默认的方法Stream()生成流
        List<String> list = new ArrayList<>();
        Stream<String> listStream = list.stream();

        Set<String> set = new HashSet<>();
        Stream<String> setStream = set.stream();
//        Map体系需要间接的生成流
        Map<Integer, String> map = new HashMap<>();
        Stream<Integer> keyStream = map.keySet().stream();
        Stream<String> valueStream = map.values().stream();
        Stream<Map.Entry<Integer, String>> entryStream = map.entrySet().stream();
//        数组可以通过Stream接口的静态方法of(T...values)生成流
        String[] strArray = {"hello", "world", "java"};
        Stream<String> strArrayStream = Stream.of(strArray);
        Stream<String> stringStream = Stream.of("hello", "world", "java");
        Stream<Integer> integerStream = Stream.of(10, 20, 30);
    }
}
```

**中间操作**

filter 对流中的数据进行过滤

```java
public class StreamDemo03 {
    public static void main(String[] args) {
        ArrayList<String> array = new ArrayList<>();
        array.add("大大怪");
        array.add("小小怪");
        array.add("小明");
        array.add("小王儿");
        array.add("大铭");
        array.add("大黎明");
        //集合中以“大”开头的元素在控制台输出
        array.stream().filter(s -> s.startsWith("大")).forEach(System.out::println);
        System.out.println("--------");
        //集合中名字长度为3的元素在控制台输出
        array.stream().filter(s -> s.length() == 3).forEach(System.out::println);
        System.out.println("--------");
        //集合中以大开头且长度为3的元素在控制台输出
        array.stream().filter(s -> s.startsWith("大")).filter(s -> s.length() == 3).forEach(System.out::println);
    }
}
```

limit 截取前n个数据

skip 跳过前n个数据

```java
public class StreamDemo04 {
    public static void main(String[] args) {
        ArrayList<String> array = new ArrayList<>();
        array.add("大大怪");
        array.add("小小怪");
        array.add("小明");
        array.add("小王儿");
        array.add("大铭");
        array.add("大黎明");
        //取前三个元素在控制台输出
        array.stream().limit(3).forEach(System.out::println);
        //跳过三个元素，把剩下的元素在控制台输出
        array.stream().skip(3).forEach(System.out::println);
        //跳过两个元素，把剩下的元素的前两个在控制台输出
        array.stream().skip(2).limit(2).forEach(System.out::println);
    }
}
```

concat 合并两个流

distinct 返回流中的不同元素（根据equals判断）

```java
public class StreamDemo05 {
    public static void main(String[] args) {
        ArrayList<String> array = new ArrayList<>();
        array.add("大大怪");
        array.add("小小怪");
        array.add("小明");
        array.add("小王儿");
        array.add("大铭");
        array.add("大黎明");
        //取前四个数据组成一个流
        Stream<String> s1 = array.stream().limit(4);
        //跳过两个数据组成一个流
        Stream<String> s2 = array.stream().skip(2);
        //合并两个流并的控制台输出
        //Stream.concat(s1,s2).forEach(System.out::println);
        //合并两个流并在控制台输出，但是输出的元素不能重复
        Stream.concat(s1, s2).distinct().forEach(System.out::println);
    }
}
```

sorted 返回自然排序后的流

可以加Comparator，自己定义排序方式

```java
public class StreamDemo06 {
    public static void main(String[] args) {
        ArrayList<String> array = new ArrayList<>();
        array.add("dadaguai");
        array.add("xiaoxiaoguai");
        array.add("xiaoming");
        array.add("xiaowanger");
        array.add("daming");
        array.add("daliming");
        //按照字母顺序把数据在控制台输出
        array.stream().sorted().forEach(System.out::println);
        System.out.println("--------");
        //按照字符串长度把数据在控制台输出
        array.stream().sorted((s1, s2) -> s1.length() - s2.length()).forEach(System.out::println);
    }
}
```

map 返回给定函数作用于流中元素之后的流

mapToInt 返回一个IntStream，其中包含给定函数作用于此流元素的结果

```java
public class StreamDemo07 {
    public static void main(String[] args) {
        ArrayList<String> array = new ArrayList<>();
        array.add("10");
        array.add("20");
        array.add("30");
        array.add("40");
        array.add("50");
        //把集合中的数据转换为整数之后输出
        array.stream().map(Integer::parseInt).forEach(System.out::println);
        System.out.println("--------");
        //利用mapToInt
        array.stream().mapToInt(Integer::parseInt).forEach(System.out::println);
        System.out.println("--------");
        //int sum 返回Int流中元素的和并输出
        System.out.println(array.stream().mapToInt(Integer::parseInt).sum());
    }
}
```

**终结操作**

forEach 对流中每个元素进行操作

count 返回流中元素个数

```java
public class StreamDemo08 {
    public static void main(String[] args) {
        ArrayList<String> array = new ArrayList<>();
        array.add("大大怪");
        array.add("小小怪");
        array.add("小明");
        array.add("小王儿");
        array.add("大铭");
        array.add("大黎明");
        //把集合中的元素在控制台输出
        array.stream().forEach(System.out::println);
        //统计集合中有几个以“大”开头，把统计结果输出
        System.out.println(array.stream().filter(s -> s.startsWith("大")).count());
    }
}
```

**Stream流的收集**

```java
public class StreamDemo09 {
    public static void main(String[] args) {
        List<String> array = new ArrayList<>();
        array.add("大大怪");
        array.add("小小怪");
        array.add("小明");
        array.add("小王儿");
        array.add("大铭");
        array.add("大黎明");
        //得到名字为3个子的流
        Stream<String> listStream = array.stream().filter(s -> s.length() == 3);
        //储存在集合中并遍历
        List<String> names = listStream.collect(Collectors.toList());
        for (String name : names) {
            System.out.println(name);
        }

        Set<Integer> set = new HashSet<>();
        set.add(10);
        set.add(20);
        set.add(30);
        set.add(40);
        set.add(50);
        set.add(60);
        //得到年龄大于25的流
        Stream<Integer> setStream = set.stream().filter(age -> age > 25);
        //储存在集合中并遍历
        Set<Integer> ages = setStream.collect(Collectors.toSet());
        for (Integer age : ages) {
            System.out.println(age);
        }

        //字符串数组，每个字符串数据由姓名和年龄组成
        String[] strArray = {"小王,20", "小青,26", "小明,33", "小刚,28", "小李,32", "小白,22"};
        //取字符串中年龄大于28的数据并存放到map中，姓名作键，年龄作值
        Stream<String> ageStream = Stream.of(strArray).filter(s -> Integer.parseInt(s.split(",")[1]) > 28);
        Map<String, Integer> map = ageStream.collect(Collectors.toMap(s -> s.split(",")[0], s -> Integer.parseInt(s.split(",")[1])));
        Set<String> key = map.keySet();
        for (String s : key) {
            Integer value = map.get(s);
            System.out.println(s + "," + value);
        }
    }
}
```

**Stream流练习**

```java
public class StreamTest {
    public static void main(String[] args) {
        ArrayList<String> manList = new ArrayList<>();
        manList.add("周润发");
        manList.add("成龙");
        manList.add("刘德华");
        manList.add("吴京");
        manList.add("周星驰");
        manList.add("李连杰");
        ArrayList<String> womanList = new ArrayList<>();
        womanList.add("林心如");
        womanList.add("张曼玉");
        womanList.add("林青霞");
        womanList.add("柳岩");
        womanList.add("林志玲");
        womanList.add("王祖贤");

        //男演员主要名字是三个字的前三个
        Stream<String> manStream = manList.stream().filter(s -> s.length() == 3).limit(3);
        //女演员只要姓林的，而且不要第一个
        Stream<String> womanStream = womanList.stream().filter(s -> s.startsWith("林")).skip(1);
        //合并流
        Stream<String> stream = Stream.concat(manStream, womanStream);
        //根据流中的元素作为构造方法创建演员对象，变量数据
        stream.map(Actor::new).forEach(p -> System.out.println(p.getName()));
    }
}
```

#### 类加载器

**类加载**

类的加载

* 将calss文件读入内存，创建一个java.lang.Class对象

类的连接

* 验证阶段
* 准备阶段
* 解析阶段

类的初始化

* 对类变量进行初始化
* 加载和连接，初始化父类，执行初始化语句

初始化时机

* 创建类的实例
* 调用类的类方法（静态方法）
* 访问类或接口的类变量
* 使用反射方式
* 初始化某个类的子类
* 直接使用java.exe命令运行某个主类

**类加载器**

负责将.class文件加载到内存中，并为之生成对应的java.lang.Class对象

类加载机制

* 全盘负责
* 父类委托
* 缓冲机制

ClassLoader:负责加载类的对象

内置加载器

* Bootstrap class：内置类加载器
* Platform class loader：平台类加载器
* System class loader：应用程序类加载器
* 继承关系：System的父加载器为Platform，PlatForm的父加载器为Bootstrap

```java
public class ClassLoaderDemo {
    public static void main(String[] args) {
        ClassLoader c1 = ClassLoader.getSystemClassLoader();
        System.out.println(c1); //AppClassLoader

        ClassLoader c2 = c1.getParent();
        System.out.println(c2); //PlatformClassLoader

        ClassLoader c3 = c2.getParent();
        System.out.println(c3); //null
    }
}
```

#### 反射

Student类

```java
public class Student {
    //成员变量，一个私有。一个默认，一个公共
    private String name;
    int age;
    public String address;

    //构造方法，一个私有，一个默认，两个公共
    public Student() {
    }

    private Student(String name) {
        this.name = name;
    }

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public Student(String name, int age, String address) {
        this.name = name;
        this.age = age;
        this.address = address;
    }

    //成员方法，一个私有，四个公共
    private void function() {
        System.out.println("function");
    }

    public void method1() {
        System.out.println("method");
    }

    public void method2(String s) {
        System.out.println("method:" + s);
    }

    public String method3(String s, int i) {
        return s + "," + i;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", address='" + address + '\'' +
                '}';
    }
}
```

```java
public class ReflectDemo01 {
    public static void main(String[] args) throws ClassNotFoundException {
        //类的class属性
        Class<Student> c1 = Student.class;
        System.out.println(c1); //class day7.study2.Student
        Class<Student> c2 = Student.class;
        System.out.println(c1 == c2);
        System.out.println("--------");

        //对象的getClass方法
        Student s = new Student();
        Class<? extends Student> c3 = s.getClass();
        System.out.println(c3); //class day7.study2.Student

        //Class类中的静态方法forName
        Class<?> c4 = Class.forName("day7.study2.Student"); //class day7.study2.Student
        System.out.println(c4);
    }
}
```

```java
public class ReflectDemo02 {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        Class<?> c = Class.forName("day7.study2.Student");   //得到字节码文件
        //获得所有的公共构造方法的对象数组
        Constructor<?>[] cons1 = c.getConstructors();
        for (Constructor<?> con : cons1) {
            System.out.println(con);
        }
        //所有的构造方法的对象数组
        Constructor<?>[] cons2 = c.getDeclaredConstructors();
        for (Constructor<?> con : cons2) {
            System.out.println(con);
        }
        //单个公共的构造方法对象
        Constructor<?> con = c.getConstructor();
        Object obj = con.newInstance();  //创建学生类对象
        System.out.println(obj);
        //单个构造方法对象
        c.getDeclaredConstructor();
    }
}
```

```java
public class ReflectDemo03 {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        Class<?> c = Class.forName("day7.study2.Student");
        Constructor<?> con1 = c.getConstructor(String.class, int.class, String.class); //基本类型也有.class
        Object obj1 = con1.newInstance("小王", 20, "广州");
        System.out.println(obj1);  //Student{name='小王', age=20, address='广州'}

        Constructor<?> con2 = c.getDeclaredConstructor(String.class);
        //暴力反射，使用私有构造方法
        con2.setAccessible(true);
        Object obj2 = con2.newInstance("小李");
        System.out.println(obj2);
    }
}
```

```java
public class ReflectDemo04 {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException, NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        Class<?> c = Class.forName("day7.study2.Student");
        //获得无参构造方法创建对象
        Constructor<?> con = c.getConstructor();
        Object obj = con.newInstance();

        System.out.println(obj);
        System.out.println("--------");
        //获得成员变量的对象
        Field addressField = c.getField("address");
        //给obj的成员变量addressField赋值为 广州
        addressField.set(obj, "广州");

        Field nameField = c.getDeclaredField("name");
        nameField.setAccessible(true); //取消访问检查
        nameField.set(obj, "小王");

        Field ageField = c.getDeclaredField("age");
        ageField.set(obj, 22);

        System.out.println(obj);
    }
}
```

```java
public class ReflectDemo05 {
    public static void main(String[] args) throws ClassNotFoundException {
        Class<?> c = Class.forName("day7.study2.Student");
        Method[] m1 = c.getMethods();  //公共方法，这里包括了继承的方法
        Method[] m2 = c.getDeclaredMethods(); //所有方法，但是不包括继承的方法
        for (Method method : m1) {
            System.out.println(method
            );
        }
        System.out.println("--------");
        for (Method method : m2) {
            System.out.println(method);
        }
    }
}
```

```java
public class ReflectDemo06 {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        Class<?> c = Class.forName("day7.study2.Student");
        Constructor<?> con = c.getConstructor();
        Object obj = con.newInstance();

        Method m1 = c.getMethod("method1");
        m1.invoke(obj);

        Method m2 = c.getMethod("method2", String.class);
        m2.invoke(obj, "小王");

        Method m3 = c.getMethod("method3", String.class, int.class);
        Object o = m3.invoke(obj, "小李", 23);
        System.out.println(o);

        Method m4 = c.getDeclaredMethod("function");
        m4.setAccessible(true);
        m4.invoke(obj);
    }
}
```

```java
public class ReflectTest01 {
    public static void main(String[] args) throws NoSuchMethodException, InvocationTargetException, IllegalAccessException {
        ArrayList<Integer> array = new ArrayList<>();
        array.add(10);
        array.add(20);
        //array.add("30");  肯定会报错

        Class<? extends ArrayList> c = array.getClass();
        Method m = c.getMethod("add", Object.class);
        m.invoke(array, "hello");
        m.invoke(array, "world");
        m.invoke(array, "java");
        for (Object i : array) {
            System.out.println(i);
        }
    }
}
```

配置文件

```java
public class ReflectTest02 {
    public static void main(String[] args) throws IOException, ClassNotFoundException, NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        /*
        class.txt
        className=×××
        methodName=×××
         */

        //加载数据
        Properties prop = new Properties();
        FileReader fr = new FileReader("src\\day7\\study2\\class.txt");
        prop.load(fr);
        fr.close();
        String className = prop.getProperty("className");
        String methodName = prop.getProperty("methodName");

        Class<?> c = Class.forName(className);
        Constructor<?> con = c.getConstructor();
        Object obj = con.newInstance();

        Method m = c.getMethod(methodName);
        m.invoke(obj);
    }
}
```

**模块化**

模块中的src文件夹新建文件 module-info.java

```java
//让模块中的包能被其他模块使用
module Java {
    exports day7.study2;
}
```

```java
//导入其他模块依赖
module myTwo {
    requires Java学习;
}
```

#### 基础加强

**Junit单元测试（白盒测试）**

* 步骤

  1.test包

  2.定义测试类（测试用例）类名Test

  3.定义测试方法：可以独立运行

  方法名：test方法名 （testAdd（））

  ​ 返回值：void

  ​ 参数列表：空参数

  4.给方法加注解 @Test

  5.导入Junit寒假依赖

* 判断结果

  红色：成功

  绿色：失败

  断言：我断言是这个结果，不是就会失败

* 补充
  1. @Before：修饰init方法，会在测试方法之前被自动执行
  2. @After：修饰close方法，会在测试方法之后被自动执行

```java
public class CalculatorTest {
    /**
     * 初始化方法
     * 用于资源申请，所有执行方法之前都会执行此方法
     */
    @Before
    public void init() {
        System.out.println("初始化成功...");
    }

    /**
     * 释放资源的方法
     * 所有的方法执行之后，都会执行此方法
     */
    @After
    public void close() {
        System.out.println("释放资源成功...");
    }

    @Test
    public void testAdd() {
        System.out.println("测试中");
        Calculator c = new Calculator();
        int result = c.add(1, 2);
        Assert.assertEquals(4, result);
    }
}
```

**反射**

* 框架：在框架的基础上进行软件开发，简化编码，框架是用反射实现的
* 反射：将类的各个组成部分封装为其他对象，这就是反射机制

  1.在程序运行的过程中，操作这些对象

  2.解耦，提高程序的拓展性

* 实现

  1.配置文件

  2.反射

**注解**

* 根据注解生成文档
* 代码分析
* 编译检查

**JDK预定义的一些注解**

@Override:检测被该注解标准的方法是否是继承自父类/接口的

@Deprecated：该注解标注的方法，表示以过时（仍可以使用）

@SuppressWarnings：压制警告

```java
@SuppressWarnings("all")  //压制所有警告
public class AnnoDemo {
    @Override  //重写
    public String toString() {
        return super.toString();
    }

    @Deprecated  //标注此方法已过时
    public void show() {
        System.out.println("有缺陷的show方法");
    }

    public void newShow() {
        System.out.println("新的show方法");
    }

    public void demo() {
        show();
        Date date = new Date();
        date.getYear();
    }
}
```

**自定义注解**

格式

* 元注解：用于描述注解的注解

  1.**@Target**：描述注解能够作用的位置

  ​ ElementType的取值

  ​ TYRE：可以作用于类上

  ​ METHOD：可以作用于成员方法上

  ​ FIELD：可以作用于成员变量上

  2.**@Retention**：描述注解能够被保留的阶段

  ​ RetentionPolicy的取值

  ​ SOURCE：不会保留到字节码文件中，停留在编译阶段

  ​ CLASS:会保留到字节码文件中

  ​ RUNTIME：会保留到字节码文件中,并被JVM读取到

  3.**@Documented**：描述注解能否被抽取到api文档中

  4.**@Inherited**：描述注解能否被子类继承

* ```java
  public @interface MyAnno {}
  ```
* 本质:本质是一个接口，接口默认继承Annotation接口

  ```java
  public interface MyAnno extends java.lang.annotation.Annotation{}
  ```

* 属性：接口中的抽象方法

  1.属性的返回值类型：基本数据类型 String 枚举 注解 以上类型的数组

  2.可以使用default初始化，初始化之后不需要赋值

  ```java
  public @interface MyAnno {
      String hello() default "我的注解";
  }
  ```

  3.如果只有一个属性需要赋值，并且叫做value，即可以省略属性名

  4.数组赋值时使用大括号包裹，只有一个值可以省略

**使用\(解析\)注解**

```java
@Pro(className = "day7.study3.Student", methodName = "show")
public class ReflectTest {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        //可以创建任意类的对象，可以执行任意方法
        Class<ReflectTest> c = ReflectTest.class;
        Pro anno = c.getAnnotation(Pro.class); //其实就是在内存中生成了一个该注解接口的子类实现对象
        Class<?> cc = Class.forName(anno.className());
        Constructor<?> con = cc.getConstructor();
        Object obj = con.newInstance();
        Method m = cc.getMethod(anno.methodName());
        m.invoke(obj);
    }
}
```

**简单的测试框架**

```java
/**
 * 自定义的计算器类
 */
public class Calculator {
    //加法
    @Check
    public void add() {
        System.out.println("1+0=" + (1 + 0));
    }

    //减法
    @Check
    public void sub() {
        System.out.println("1-0=" + (1 - 0));
    }

    //乘法
    @Check
    public void mul() {
        System.out.println("1*0=" + (1 * 0));
    }

    //除法
    @Check
    public void div() {
        System.out.println("1/0=" + (1 / 0));
    }

    public void show() {
        System.out.println("永无bug...");
    }
}
```

```java
/**
 * 简单的测试框架
 * 执行加了@Check注解的方法
 * 若方法有异常，将异常信息记录到bug.txt文件中
 */
public class TestCheck {
    public static void main(String[] args) throws IOException {

        Calculator c = new Calculator();
        Class<? extends Calculator> cal = c.getClass();
        Method[] methods = cal.getMethods();//获取所有的方法
        int number = 0;//出现异常的次数
        BufferedWriter bw = new BufferedWriter(new FileWriter("src\\day7\\study4\\bug.txt"));
        for (Method method : methods) {
            if (method.isAnnotationPresent(Check.class)) {
                try {
                    method.invoke(c);
                } catch (Exception e) {
                    //捕获异常
                    //记录在文件中
                    bw.write(method.getName() + "方法出现异常了");
                    bw.newLine();
                    bw.newLine();
                    bw.write("异常名称：" + e.getCause().getClass().getSimpleName());
                    bw.newLine();
                    bw.write("异常原因：" + e.getCause().getMessage());
                    bw.newLine();
                    bw.write("----------------------------");
                    bw.newLine();
                    number++;
                }
            }
        }
        bw.write("本次测试一共出现" + number + "处异常");
        bw.flush();
        bw.close();
    }
}
```

bug.txt 的内容

```text
div方法出现异常了

异常位置：public void day7.study4.Calculator.div()方法
异常名称：ArithmeticException
异常原因：/ by zero
----------------------------
本次测试一共出现1处异常
```

