# day4

## 完成情况

{% hint style="success" %}
每一天都学习并记录
{% endhint %}

{% hint style="warning" %}
每天看至少3个leetcode的题目
{% endhint %}

## 学习内容

#### 基本数据类型包装类

| 基本数据类型 | 包装类 |
| :---: | :---: |
| byte | Byte |
| short | Short |
| int | Integer |
| long | Long |
| float | Float |
| double | Double |
| char | Character |
| boolean | Boolean |

```java
public class IntegerDemo {
    public static void main(String[] args) {
        //此方法已过时
        Integer i1=new Integer(100);
        System.out.println(i1);

        Integer i2=Integer.valueOf(200);
        System.out.println(i2);
    }
}
```

**int 与 String 的相互转换**

```java
public class IntegerDemo {
    public static void main(String[] args) {
        //int->String
        int number = 100;
        String s=String.valueOf(number);
        System.out.println(s);

        //String->int
        String s1="200";
        int number1 = Integer.parseInt(s1);
        System.out.println(number1);
    }
}
```

**自动装箱和拆箱**

```java
public class IntegerDemo {
    public static void main(String[] args) {
        //自动装箱，基本数据类型自动转换成包装类类型
        Integer i=100;
        //自动拆箱，包装类类型自动转换成基本数据类型
        int ii=i+200;
        System.out.println(ii);
    }
}
```

#### 日期类

```java
public class DateDemo01 {
    public static void main(String[] args) {
        Date d1=new Date();
        //输出当前时间
        System.out.println(d1);

        long date=1000*60*60;
        Date d2=new Date(date);
        System.out.println(d2);
    }
}
```

```java
public class SimpleDateFormatDemo {
    public static void main(String[] args) throws ParseException {
        SimpleDateFormat sdf=new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
        Date d=new Date();

        //Date->String

        String s=sdf.format(d);
        System.out.println(s);

        //String->Date

        String ss="1999年01月28日 11:45:21";
        Date dd=sdf.parse(ss);
        System.out.println(dd);
    }
}
```

```java
public class CalendarDemo {
    public static void main(String[] args) {
        Calendar c = Calendar.getInstance();//多态的形式
        System.out.println(c);

        //十年前的5天后
        c.add(Calendar.YEAR,-10);
        c.add(Calendar.DATE,5);
        int year = c.get(Calendar.YEAR);
        int month = c.get(Calendar.MONTH) + 1;  //月份记得要+1
        int date = c.get(Calendar.DATE);
        System.out.println(year + "年" + month + "月" + date + "日");
    }
}
```

**二月天**

```java
public class CalendarDemo {
    public static void main(String[] args) {
        Calendar c = Calendar.getInstance();//多态的形式
        Scanner sc=new Scanner(System.in);
        System.out.println("请输入年份");
        int year =sc.nextInt();
        //3月1日
        c.set(year,2,1);
        //往前推一天
        c.add(Calendar.DATE,-1);
        //获取2月份的天数
        int date=c.get(Calendar.DATE);
        //输出
        System.out.println(date);
    }
}
```

#### 异常

Throwable是所有异常和错误的超类

Throwable\(Error,Exception\)

* Error 严重问题，不需要处理
* Exception 异常类，表示程序本身可以处理的问题

Exception（RuntimeException,非RuntimeException）

* RuntimeException 编译时不检测，出错后回来修改代码
* 非RuntimeException 编译期需要处理，否则不能通过编译

#### JVM默认处理

```java
public class ExceptionDemo {
    public static void main(String[] args) {
        System.out.println("开始");
        method();
        System.out.println("结束");
    }
    public static void method(){
        int[] arr={1,2,3};
        System.out.println(arr[3]);//RuntimeException
    }
}
```

* 把异常的名称,异常的原因以及出现的位置等信息输出到控制台
* 程序停止执行

**try catch**

**运行时异常 RuntimeException**

```java
public class ExceptionDemo {
    public static void main(String[] args) {
        System.out.println("开始");
        method();
        System.out.println("结束");
    }
    public static void method(){
        int[] arr={1,2,3};
        try{
            System.out.println(arr[3]);//ArrayIndexOutOfBoundsException
        }
        catch (ArrayIndexOutOfBoundsException e){
            System.out.println(e.getMessage());//出现异常的原因
            System.out.println(e.toString());  //异常类和异常原因
            e.printStackTrace();               //控制台输出异常信息
        }
    }
}
```

**编译时异常**

这里很神奇，就算没写错，也要写上异常处理，否则就不能编译

```java
public class ExceptionDemo02 {
    public static void main(String[] args) {
        method();
    }
    public static void method(){
        try {
            String s = "2020-01-01";
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
            Date d = sdf.parse(s);
            System.out.println(d);
        } catch (ParseException e){
            e.printStackTrace();
        }
    }
}
```

**throws**

仅仅抛出异常，要处理还是要使用try catch

```java
public class ExceptionDemo02 {
    public static void main(String[] args) {
        try {
            method();
        } catch (ParseException e) {
            e.printStackTrace();
        }
    }
    public static void method() throws ParseException{

        String s = "2020-01-01";
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        Date d = sdf.parse(s);
        System.out.println(d);
    }
}
```

**自定义异常**

```java
//自定义异常类
public class ScoreException extends Exception{
    public ScoreException(){
    }
    public  ScoreException(String message){
        super(message);
    }
}
```

```java
public class Teacher {
    public void checkScore(int score) throws ScoreException{
        if(score<0 || score>100){
            throw new ScoreException("你给的分数有误，分数应该在0-100之间");
        } else{
            System.out.println("分数正常");
        }
    }
}
```

```java
public class TeacherTest {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入成绩");
        int score = sc.nextInt();
        Teacher t = new Teacher();
        try {
            t.checkScore(score);
        } catch (ScoreException e) {
            e.printStackTrace();
        }
    }
}
```

#### 集合

Collection

```java
public class CollectionDemo01 {
    public static void main(String[] args) {
        //创建Collection集合的对象
        Collection<String> c = new ArrayList<String>();
        //添加元素
        c.add("hello"); //添加元素
        c.add("world");
        c.add("java");
        c.add("aaa");
        System.out.println(c.size()); //集合元素个数
        System.out.println(c); //重写了toString方法
        c.remove("aaa"); //输出元素
        System.out.println(c); //重写了toString方法
        System.out.println(c.contains("java")); //是否存在某一元素
        System.out.println(c.isEmpty()); //集合是否为空
        c.clear();
        System.out.println(c.isEmpty());
    }
}
```

Iterator 集合的迭代器

```java
public class CollectionDemo02 {
    public static void main(String[] args) {
        Collection<String> c = new ArrayList<String>();
        c.add("hello");
        c.add("world");
        c.add("java");
        Iterator<String> it = c.iterator();

        while (it.hasNext()) {
            String s = it.next();
            System.out.println(s);
        }
    }
}
```

List集合

ArrayList 数组 （查询快，增删慢）

LinkedList 链表 （查询慢，增删快）

```java
public class ListDemo {
    public static void main(String[] args) {
        List<String> list = new ArrayList<String>();
        list.add("hello");
        list.add("world");
        list.add("java");
        list.add("world");
        System.out.println(list.get(2));
        list.add(2, "222");
        System.out.println(list);
        System.out.println(list.get(2));
        list.remove(1);
        list.set(3, "333");
        System.out.println(list);
    }
}
```

**并发修改异常**

迭代器遍历过程中，通过集合对象修改了集合的长度，造成了迭代器获取元素中判断预期修改值和实际修改值不一致

解决方法

用for循环遍历，然后用集合对象做相应的操作

**增强for循环**

内部原理：Iterator迭代器

```java
public class CollectionDemo {
    public static void main(String[] args) {
        Collection<Student> c=new ArrayList<Student>();
        c.add(new Student("001","小王",26));
        c.add(new Student("002","小明",22));
        c.add(new Student("003","小刚",20));

        for (Student s : c) {
            System.out.println(s.getSid() + "," + s.getName() + "," + s.getAge());
        }
    }
}
```

**Set集合**

* 不包含重复元素
* 没有索引
* 迭代顺序不作保证

```java
public class SetDemo {
    public static void main(String[] args) {
        Set<String> s = new HashSet<String>();
        s.add("hello");
        s.add("world");
        s.add("java");
        for (String ss : s) {
            System.out.println(ss);
        }
    }
}
```

**哈希值**

```java
public class HashDemo {
    public static void main(String[] args) {
        Student s1 = new Student("001", "小王", 26);
        System.out.println(s1.hashCode());
        System.out.println(s1.hashCode());
        //同一对象的哈希值相同
        System.out.println("--------");
        Student s2 = new Student("001", "小王", 26);
        System.out.println(s2.hashCode());
        //默认情况下不同对象的哈希值不同
        //通过方法重写，可以让不同对象的哈希值相同
        System.out.println("--------");
        System.out.println("hello".hashCode());
        System.out.println("world".hashCode());
        System.out.println("java".hashCode());
        System.out.println("--------");
        System.out.println("重地".hashCode());
        System.out.println("通话".hashCode());
    }
}
```

**HashSet**

* 底层数据结构是数组和哈希表
* 不带索引
* 没有重复元素（先比较哈希值，后比较内容）

**LinkedHashSet**

* 底层数据结构是链表和哈希表
* 由链表保证数据有序
* 不带索引
* 没有重复元素

**TreeSet**

* 元素有序，这里的有序是指按一定的规则进行排序
* 无参构造：自然排序
* 带参数构造：比较器
* 不带索引
* 没有重复元素

**Comparable**

自然排序（需要重写）

```java
public class Student implements Comparable<Student>{
    @Override
    public int compareTo(Student s) {
        int num = this.age-s.age;
        if(num==0){
            if(this.name.equals(s.name)){
                num=0;
            } else {
                num=1;
            }
        }
        return num;
    }
}
```

Comparator比较器

```java
public class TreeSetDemo {
    public static void main(String[] args) {
        TreeSet<Student> ts = new TreeSet<>(new Comparator<Student>() {
            @Override
            public int compare(Student s1, Student s2) {
                int num = s1.getAge() - s2.getAge();
                int num2 = num == 0 ? s1.getName().compareTo(s2.getName()) : num;
                return num2;
            }
        });
        Student s1 = new Student("001", "小红", 22);
        Student s2 = new Student("002", "小明", 22);
        Student s3 = new Student("003", "小刚", 26);
        Student s4 = new Student("004", "小王", 24);

        ts.add(s1);
        ts.add(s2);
        ts.add(s3);
        ts.add(s4);
        for (Student s : ts) {
            System.out.println(s);
        }
    }
}
```

**不重复的随机数**

```java
public class SetDemo {
    public static void main(String[] args) {
        Set<Integer> s = new HashSet<>();
        Random r=new Random();
        while(s.size()<10){
            int num = r.nextInt(20)+1;
            s.add(num);
        }
        for(Integer i:s){
            System.out.println(i);
        }

    }
}
```

#### 泛型

泛型类

```java
public class Generic<T> {
    private T t;

    public T getT() {
        return t;
    }

    public void setT(T t) {
        this.t = t;
    }
}
```

泛型方法

```java
public class Generic {
    public <T> void show(T t){
        System.out.println(t);
    }
}
```

泛型接口

```java
public interface Generic2 <T>{
    void show(T t);
}
```

```java
public class Genericlmpl<T> implements Generic2<T>{
    @Override
    public void show(T t) {
        System.out.println(t);
    }
}
```

```java
public class GenericDemo {
    public static void main(String[] args) {
        Generic2<String> g1=new Genericlmpl<>();
        g1.show("小王");
    }
}
```

**通配符**

```java
public class GenericDemo {
    public static void main(String[] args) {
        //所有
        List<?> l1= new ArrayList<Object>();
        List<?> l2=new ArrayList<Number>();
        List<?> l3=new ArrayList<Integer>();

        //通配符的上限
        List<? extends Number> l4= new ArrayList<Integer>();

        //通配符的下限
        List<? super Number> l5= new ArrayList<Object>();
    }
}
```

**可变参数**

```java
public class ArgsDemo {
    public static void main(String[] args) {
        System.out.println(sum(1, 2, 3, 4));
        System.out.println(sum(1, 2, 3));
    }
    public static int sum(int... a) {
        int sum = 0;
        for (int i : a) {
            sum += i;
        }
        return sum;
    }
}
```

