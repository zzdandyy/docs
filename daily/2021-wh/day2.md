---
description: '2021-01-26'
---

# day2

## 完成情况

{% hint style="success" %}
每一天都学习并记录
{% endhint %}

{% hint style="warning" %}
每天看至少3个leetcode的题目
{% endhint %}

## 学习内容

#### 成员变量与局部变量

| 区别 | 成员变量 | 局部变量 |
| :---: | :---: | :---: |
| 类中位置不同 | 类中方法外 | 方法内 |
| 内存中位置不同 | 堆内存 | 栈内存 |
| 生命周期不同 | 同对象 | 同方法 |
| 初始值不同 | 有初始值 | 必须先赋值 |

#### 封装

**private**

权限修饰符

如果需要修改

* get变量名\(\)，用public修饰
* set变量\(参数\)，用public修饰

**this**

如果方法中有同名局部变量，成员变量要用this修饰

```java
public void setAge(int age) {
        if(age>0 && age<120) {
            this.age = age;
        }
        else{
            System.out.println("你设置的年龄有误，设置为初始值");
        }
    }
```

```java
public static void main(String[] args) {
        Student s = new Student();
        s.setName("小王");
        s.setAge(30);
        System.out.println(s.getName()+","+s.getAge());
    }
```

**标准类**

```java
public class Student {
    //成员变量
    private String name;
    private int age;

    public Student(String name,int age){
        this.name=name;
        this.age=age;
    }
    public Student(String name){
        this.name=name;
    }
    public Student(int age){
        this.age=age;
    }
    public Student(){
    }

    public void setAge(int age) {
        if(age>0 && age<120) {
            this.age = age;
        }
        else{
            System.out.println("你设置的年龄有误，设置为初始值");
        }
    }

    public int getAge() {
        return age;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void show(){
        System.out.println(name+","+age);
    }
}
```

```java
public static void main(String[] args) {
        Student s1 = new Student("小王",20);
        Student s2 = new Student("小林");
        Student s3 = new Student(22);
        Student s4 = new Student();
        System.out.println(s1.getName()+","+s1.getAge());
        System.out.println(s2.getName()+","+s2.getAge());
        System.out.println(s3.getName()+","+s3.getAge());
        System.out.println(s4.getName()+","+s4.getAge());
        s1.show();
        s2.setAge(21);
        s2.show();
        s3.setName("小红");
        s3.show();
        s4.setName("小明");
        s4.setAge(23);
        s4.show();
    }
```

#### 字符串

**字符串构造方法**

```text
String st1="1234";

char[] chs={'1','2','3'};
String st2=new String(chs);
```

**用户登录**

```java
import java.util.Scanner;
public class User {
    private final String  username="doublez";
    private final String password="aa2217117";

    public void lognIn(){
        Scanner sc=new Scanner(System.in);
        for(int i=0;i<3;i++){
            System.out.println("请输入用户名");
            String un=sc.nextLine();
            System.out.println("请输入密码");
            String pw=sc.nextLine();
            if(un.equals(this.username) && pw.equals(this.password)){
                System.out.println("登录成功");
                break;
            }
            else {
                System.out.println("用户名或密码错误");
            }
        }
        System.out.println("失败次数过多,稍后再试");
    }
}
```

**遍历字符串**

```java
public void getString(){
        Scanner sc=new Scanner(System.in);
        System.out.println("请输入一个字符串");
        String line=sc.nextLine();
        for(int i=0;i<line.length();i++){
            System.out.println(line.charAt(i));
        }
    }
```

**字符统计**

```java
public void countNumber(){
        Scanner sc=new Scanner(System.in);
        System.out.println("请输入一个字符串");
        String line=sc.nextLine();
        int bigCount=0;
        int smallCount=0;
        int numberCount=0;
        for(int i=0;i<line.length();i++){
            if(line.charAt(i)>='A' && line.charAt(i)<='Z'){
                bigCount++;
            }
            else if(line.charAt(i)>='a' && line.charAt(i)<='z'){
                smallCount++;
            }
            else{
                numberCount++;
            }
        }
        System.out.println("大写字母有"+bigCount+"个");
        System.out.println("小写字母有"+smallCount+"个");
        System.out.println("数字有"+numberCount+"个");
    }
```

**字符串反转**

```java
public String stringBack(String st){
        String stBack="";
        for(int i=st.length()-1;i>=0;i--){
            stBack+=st.charAt(i);
        }
        return stBack;
    }
```

**StringBulid类**

拼接

反转

**ArrayList**

```java
ArrayList<String> arrayList=new ArrayList<>();
arrayList.add("666");
System.out.println(arrayList);
arrayList.add("555");
System.out.println(arrayList);
arrayList.add(1,"999");
System.out.println(arrayList);
System.out.println(arrayList.size());
```

**学生信息管理系统**

{% page-ref page="../../java/java-1/java-project/project1/" %}



