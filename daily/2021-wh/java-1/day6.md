---
description: '2021-01-30'
---

# day6

## 完成情况

{% hint style="success" %}
每一天都学习并记录
{% endhint %}

{% hint style="warning" %}
每天看至少3个leetcode的题目
{% endhint %}

## 学习内容

**录入学生信息，自动排序写入文件**

```java
public class TreeSetToTxtDemo {
    public static void main(String[] args) throws IOException {
        TreeSet<Student> ts = new TreeSet<>((s1, s2) -> {
            int num = s2.getSum() - s1.getSum();
            int num2 = num == 0 ? s1.getChinese() - s2.getChinese() : num;
            int num3 = num2 == 0 ? s1.getEnglish() - s2.getEnglish() : num2;
            return num3 == 0 ? s1.getName().compareTo(s2.getName()) : num3;
        });
        for (int i = 0; i < 5; i++) {
            Scanner sc = new Scanner(System.in);
            System.out.println("请输入第" + (i + 1) + "个学生信息");
            System.out.println("姓名：");
            String name = sc.nextLine();
            System.out.println("语文成绩：");
            int chinese = sc.nextInt();
            System.out.println("数学成绩：");
            int math = sc.nextInt();
            System.out.println("英语成绩：");
            int english = sc.nextInt();

            Student st = new Student();
            st.setName(name);
            st.setChinese(chinese);
            st.setMath(math);
            st.setEnglish(english);
            ts.add(st);
        }
        BufferedWriter bw = new BufferedWriter(new FileWriter("src\\day6\\study1\\student.txt"));
        for (Student s : ts) {
            bw.write(s.getName() + "," + s.getChinese() + "," + s.getMath() + "," + s.getEnglish() + " 总分" + s.getSum());
            bw.newLine();
            bw.flush();
        }
        bw.close();
        System.out.println("录入结束");
    }
}
```

**键盘录入**

```java
BufferedReader br =new BufferedReader(new InputStreamReader(System.in));
//但是这样子太麻烦，可以使用Scanner

Scanner sc=new Scanner(System.in);
```

**Properties与IO流**

```java
public class PropertiesAndIoDemo {
    public static void main(String[] args) throws IOException {
        //集合中的数据保存到文件
        myStore();
        //文件数据保存到集合
        myLoad();
    }

    private static void myLoad() throws IOException {
        Properties prop = new Properties();
        FileReader fr = new FileReader("src\\day6\\study2\\test.txt");
        prop.load(fr);
        fr.close();
        System.out.println(prop);
    }

    private static void myStore() throws IOException {

        Properties prop = new Properties();
        FileWriter fw = new FileWriter("src\\day6\\study2\\test.txt");
        prop.setProperty("001", "小王");
        prop.setProperty("002", "小李");
        prop.setProperty("003", "小明");
        prop.store(fw, null);
        fw.close();
    }
}
```

**游戏次数上限**

```java
public class PropertiesDemo {
    public static void main(String[] args) throws IOException {
        Properties prop = new Properties();

        FileReader fr = new FileReader("src\\day6\\study2\\game.txt");
        prop.load(fr);
        fr.close();

        int number = Integer.parseInt(prop.getProperty("count"));
        if (number >= 3) {
            System.out.println("次数已达上限(https://www.baidu.com)");
        } else {
            GameDemo.play(); //游戏类
            number++;
            prop.setProperty("count", String.valueOf(number));
            FileWriter fw = new FileWriter("src\\day6\\study2\\game.txt");
            prop.store(fw, null);
            fw.close();
        }
    }
}
```

#### 线程

**进程和线程**

进程

* 系统正在运行的程序
* 每个进程都有自己独立的空间

线程

* 进程中的单个顺序控制流，是一天执行路径
* 单线程：进程只有一条执行路径
* 多线程：进程有多条执行路径

**多线程**

```java
public class MyThreadDemo {
    public static void main(String[] args) {
        MyThread mt1 = new MyThread("线程001");
        MyThread mt2 = new MyThread("线程002");
        //线程的重命名
        mt1.setName("线程1");
        mt2.setName("线程2");
        //start开始执行线程，自动调用run方法
        mt1.start();
        mt2.start();
        //获取main方法的线程名称
        System.out.println(Thread.currentThread().getName());
    }
}
```

**线程调度**

```java
public class MyThreadPriorityDemo {
    public static void main(String[] args) {
        MyThread mt1 = new MyThread("飞机");
        MyThread mt2 = new MyThread("高铁");
        MyThread mt3 = new MyThread("汽车");

        //线程优先级
        mt1.setPriority(10);
        mt2.setPriority(5); //5是默认的
        mt3.setPriority(1);
        System.out.println(mt1.getPriority());
        System.out.println(mt2.getPriority());
        System.out.println(mt3.getPriority());

        //start开始执行线程，自动调用run方法
        mt1.start();
        mt2.start();
        mt3.start();
    }
}
```

join 等待线程死亡

```java
mt1.start();
try {
    mt1.join();
} catch (InterruptedException e) {
    e.printStackTrace();
}
mt2.start();
mt3.start();
```

sleep 线程休眠

```java
try {
    Thread.sleep(10);
} catch (InterruptedException e) {
    e.printStackTrace();
```

setDaemon 设置为守护线程

```java
public class MyThreadControlDemo {
    public static void main(String[] args) {
        MyThread mt1 = new MyThread("关羽");
        MyThread mt2 = new MyThread("张飞");

        Thread.currentThread().setName("刘备");  //主线程

        mt1.setDaemon(true);  //设置为守护线程
        mt2.setDaemon(true);
        mt1.start();
        mt2.start();
        for (int i = 0; i < 10; i++) {
            System.out.println(Thread.currentThread().getName() + ":" + i);
        }
    }
}
```

**实现Runnable接口实现多线程**

```java
public class MyRunnable implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println(Thread.currentThread().getName() + ":" + i);
        }
    }
}
```

```java
public class MyRunnableDemo {
    public static void main(String[] args) {
        MyRunnable mr = new MyRunnable();
        Thread t1 = new Thread(mr, "小明");
        Thread t2 = new Thread(mr, "小王");
        t1.start();
        t2.start();
    }
}
```

**同步代码块（给代码块上锁）**

```java
@Override
public void run() {
    while (true) {
        synchronized (obj) {
            if (tickets > 0) {
                System.out.println(Thread.currentThread().getName() + "卖出票，剩余" + tickets + "张票");
                tickets--;
            }
        }
    }
}
```

同步方法 修饰符 synchronized

同步方法的锁是 this

同步静态方法 修饰发 static synchronized

同步静态方法的锁是 类名.class

**线程安全的类**

```java
public class StartDemo {
    public static void main(String[] args) {
        StringBuffer sb = new StringBuffer();   //线程安全，方法均加了synchronized
        StringBuilder sb2 = new StringBuilder();

        Vector<String> v = new Vector<>();   //线程安全，方法均加了synchronized
        ArrayList<String> array = new ArrayList<>();

        Hashtable<String, String> ht = new Hashtable<>();   //线程安全，方法均加了synchronized
        HashMap<String, String> hm = new HashMap<>();

        //但是一般用这个
        List<String> list = Collections.synchronizedList(new ArrayList<String>());
        Map<String, String> map = Collections.synchronizedMap(new HashMap<String, String>());
    }
}
```

**Lock锁**

```java
public class SellTicket implements Runnable {
    private int tickets = 100;
    private Lock lock = new ReentrantLock();

    @Override
    public void run() {
        while (true) {
            try {
                lock.lock(); //加锁
                if (tickets > 0) {
                    System.out.println(Thread.currentThread().getName() + "卖出票，剩余" + tickets + "张票");
                    tickets--;
                }
            } finally {
                lock.unlock(); //释放锁
            }
        }
    }
}
```

**生产者和消费者（牛奶）**

```java
public class Box {
    private boolean state=false;

    //存储
    public synchronized void put() {
        if(state){
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("送奶工将牛奶放入奶箱");
        this.state=true;

        notifyAll(); //唤醒其他等待的线程
    }

    //获取
    public synchronized void get() {
        if (!state){
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("用户拿到牛奶");
        this.state=false;

        notifyAll(); //唤醒其他等待的线程
    }
}
```

#### 网络编程

```java
public class InetAddressDemo {
    public static void main(String[] args) throws UnknownHostException {
        InetAddress address = InetAddress.getByName("zzd");
        String name = address.getHostName();
        String ip = address.getHostAddress();
        System.out.println(ip + "," + name);
    }
}
```

**UDP**

发送

```java
public class SendDemo {
    public static void main(String[] args) throws IOException {
        DatagramSocket ds = new DatagramSocket();
        byte[] bytes = "hello UDP,我来了".getBytes(StandardCharsets.UTF_8);
        InetAddress address = InetAddress.getByName("192.168.137.1");

        DatagramPacket dp = new DatagramPacket(bytes, bytes.length, address, 10086);
        ds.send(dp);

        ds.close();
    }
}
```

接收

```java
public class ReceiveDemo {
    public static void main(String[] args) throws IOException {
        DatagramSocket ds = new DatagramSocket(10086);
        byte[] bytes = new byte[1024];
        DatagramPacket dp = new DatagramPacket(bytes, bytes.length);

        ds.receive(dp);

        byte[] data = dp.getData();
        String s = new String(data, 0, dp.getLength());
        System.out.println(s);
    }
}
```

**TCP**

服务器 接收数据写入文本文件

```java
public class ServerDemo {
    public static void main(String[] args) throws IOException {
        ServerSocket ss = new ServerSocket(10086);

        Socket s = ss.accept();
        InputStream is = s.getInputStream();

        byte[] bytes = new byte[1024];
        int len = is.read(bytes);
        String data = new String(bytes, 0, len);
        System.out.println("数据是：" + data);

        s.close();
        ss.close();
    }
}
```

客户端 用户输入数据（886接收输入）

```java
public class ClientDemo {
    public static void main(String[] args) throws IOException {
        Socket s = new Socket("192.168.137.1", 10086);

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
        String line;

        while ((line = br.readLine()) != null) {
            if (line.equals("886")) {
                break;
            }

            bw.write(line);
            bw.newLine();
            bw.flush();
        }
        s.close();
    }
}
```

**多线程实现文件上传**

```java
public class ServerDemo {
    public static void main(String[] args) throws IOException {
        BufferedWriter bw = new BufferedWriter(new FileWriter("src\\day6\\study7\\test.txt"));
        ServerSocket ss = new ServerSocket(10086);

        while (true) {
            Socket s = ss.accept();
            new Thread(new ServerThread(s)).start();
        }
    }
}
```

```java
public class ServerThread implements Runnable {
    private Socket s;

    public ServerThread(Socket s) {
        this.s = s;
    }
    @Override
    public void run() {
        //接收数据
        try {
            BufferedReader br = new BufferedReader(new InputStreamReader(s.getInputStream()));

            BufferedWriter bw = new BufferedWriter(new FileWriter("src\\day6\\study7\\client.txt"));

            String line;
            while ((line = br.readLine()) != null) {
                bw.write(line);
                bw.newLine();
                bw.flush();
            }
            //给出反馈
            BufferedWriter bwServer = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
            bwServer.write("文件上传成功");
            bwServer.newLine();
            bwServer.flush();

            s.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

```java
public class ClientDemo {
    public static void main(String[] args) throws IOException {
        Socket s = new Socket("192.168.137.1", 10086);

        BufferedReader br = new BufferedReader(new FileReader("src\\day6\\study7\\my.txt"));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
        String line;

        while ((line = br.readLine()) != null) {
            bw.write(line);
            bw.newLine();
            bw.flush();
        }
        s.shutdownOutput();
        //接收反馈
        BufferedReader brClient = new BufferedReader(new InputStreamReader(s.getInputStream()));
        String data = brClient.readLine();
        System.out.println(data);

        s.close();
    }
}
```

#### Lambda

```java
public class LambdaDemo {
    public static void main(String[] args) {
        new Thread(() -> {
            System.out.println("多线程程序启动了");
        }).start();
    }
}
```

```java
//不带参数和返回值
public class LambdaDemo02 {
    public static void main(String[] args) {
        //主方法中调用useEatable方法
        Eatable e = new Eatablelmpl();
        useEatable(e);
        //匿名内部类
        Eatable e2 = new Eatable() {
            @Override
            public void eat() {
                System.out.println("一天一个苹果");
            }
        };
        //Lambda表达式
        useEatable(() -> {
            System.out.println("一天一个苹果");
        });
    }

    private static void useEatable(Eatable e) {
        e.eat();
    }
}
```

```java
//带参数
public class LambdaDemo03 {
    public static void main(String[] args) {
        //匿名内部类
        useFlyable(new Flyable() {
            @Override
            public void fly(String s) {
                System.out.println(s);
                System.out.println("飞机自驾游");
            }
        });
        System.out.println("--------");

        useFlyable((String s) -> {
            System.out.println(s);
            System.out.println("飞机自驾游");
        });
    }

    private static void useFlyable(Flyable f) {
        f.fly("风和日丽，晴空万里");
    }
}
```

```java
//带参数和返回值
public class LambdaDemo04 {
    public static void main(String[] args) {
        //Lambda
        useAddable((int x, int y) -> {
            return x + y;
        });
    }

    private static void useAddable(Addable a) {
        int sum = a.add(10, 20);
        System.out.println(sum);
    }
}
```

**接口**

接口的默认方法 default void show\(\){方法体}

接口的静态方法 static void show\(\){方法体}

接口的私有方法 private void show\(\){方法体}

```java
public interface MyInterface {
    void show1();

    void show2();

    default void show3() {
        getName();
        System.out.println("默认方法1");
    }

    default void show4() {
        getName();
        System.out.println("默认方法2");
    }

    static void show5() {
        System.out.println("静态方法");
    }

    private void getName() {
        System.out.println("私有方法");
    }
}
```

```java
public class MyInterfaceDemo {
    public static void main(String[] args) {
        MyInterface my = new MyInterfaceImplOne();
        my.show1();
        my.show2();
        my.show3();
        my.show4();
        MyInterface.show5();
    }
}
```

**方法引用**

```java
public class PrintableDemo {
    public static void main(String[] args) {
        usePrintable(System.out::println);
    }

    private static void usePrintable(Printable p) {
        p.printString("爱生活，爱Java");
    }
}
```

构造器

```java
public class StudentDemo {
    public static void main(String[] args) {
        useStudentBuilder(Student::new);
    }

    public static void useStudentBuilder(StudentBuilder sb) {
        Student s = sb.build("小王", 26);
        System.out.println(s.getName() + "," + s.getAge());
    }
}
```

**函数式接口**

有且仅有一个抽象方法的接口

```java
@FunctionalInterface
public interface MyInterface {
    void show();
}
```

```java
public class MyInterfaceDemo {
    public static void main(String[] args) {
        ArrayList<String> array = new ArrayList<>();
        array.add("cccc");
        array.add("aa");
        array.add("b");
        array.add("ddd");

        System.out.println("排序前：" + array);
        array.sort(getComparable());
        System.out.println("排序后：" + array);
    }

    public static Comparator<String> getComparable() {
        return (s1, s2) -> s1.length() - s2.length();
    }
}
```

```java
//Supplier接口
public class SupplierDemo {
    public static void main(String[] args) {
        int[] arr = {19, 50, 28, 37, 46};
        int intMax = getMax(() -> {
            int max = arr[0];
            for (int i : arr) {
                if (i > max) {
                    max = i;
                }
            }
            return max;
        });
        System.out.println(intMax);
    }

    public static int getMax(Supplier<Integer> sup) {
        return sup.get();
    }
}
```

```java
//consumer接口
public class ConsumerDemo {
    public static void main(String[] args) {
        operatorString("小王", System.out::println);
        operatorString("小福贵", s -> System.out.println(new StringBuilder(s).reverse().toString()));
        System.out.println("--------");
        operatorString("王二小", s -> System.out.println(s), s -> System.out.println(new StringBuilder(s).reverse().toString()));
    }

    public static void operatorString(String name, Consumer<String> con) {
        con.accept(name);
    }

    public static void operatorString(String name, Consumer<String> con1, Consumer<String> con2) {
        con1.andThen(con2).accept(name);
    }
}
```

```java
//Predicate接口
public class PredicateDemo {
    public static void main(String[] args) {
        boolean b1 = checkString("hello", s -> s.length() > 8);
        System.out.println(b1);

        boolean b2 = checkString("hello world", s -> s.length() > 8);
        System.out.println(b2);

        boolean b3 = checkString("hello world", s -> s.length() > 8, s -> s.length() < 16);
        System.out.println(b2);
    }

    //判断给定的字符串是否满足要求
    private static boolean checkString(String s, Predicate<String> pre) {
        return pre.test(s);
    }

    private static boolean checkString(String s, Predicate<String> pre1, Predicate<String> pre2) {
        //return pre1.and(pre2).test(s);
        return pre1.or(pre2).test(s);
    }
}
```

**筛选数组中符合条件的值**

```java
public class PredicateTest {
    public static void main(String[] args) {
        String[] strArray = {"小王,30", "小钱钱,20", "大红花,35", "小青,25", "小葵花,22"};
        ArrayList<String> arrayList = myFilter(strArray, s -> s.split(",")[0].length() > 2, s -> Integer.parseInt(s.split(",")[1]) > 33);
        System.out.println(arrayList);

    }

    private static ArrayList<String> myFilter(String[] strings, Predicate<String> pre1, Predicate<String> pre2) {
        ArrayList<String> array = new ArrayList<>();
        for (String string : strings) {
            if (pre1.and(pre2).test(string)) {
                array.add(string);
            }
        }
        return array;
    }
}
```

```java
//Function接口
public class FunctionDemo {
    public static void main(String[] args) {
        convert("100", s -> Integer.parseInt(s));

        convert(100, i -> String.valueOf(i + 566));

        convert("100", s -> Integer.parseInt(s), i -> String.valueOf(i + 566));
    }

    //定义一个方法，把字符串类型转换为int，并在控制台输出
    private static void convert(String s, Function<String, Integer> fun) {
        int i = fun.apply(s);
        System.out.println(i);
    }

    //定义一个方法，把一个int类型的数据加上一个整数之后，转换成字符串后输出
    private static void convert(int i, Function<Integer, String> fun) {
        String s = fun.apply(i);
        System.out.println(s);
    }

    //定义一个方法，把字符串数据转换成int之后加上一个整数，再转换成字符串输出
    private static void convert(String s, Function<String, Integer> fun1, Function<Integer, String> fun2) {
        String ss = fun1.andThen(fun2).apply(s);
        System.out.println(ss);
    }
}
```

