---
description: '2021-01-27'
---

# day3

## 完成情况

每一天都学习并记录


每天看至少3个leetcode的题目


## 学习内容

#### 继承

extends

父类：基类，超类

子类：派生类

```java
public class Person {
    private String name;
    private int age;
    public void show(){
        System.out.println("父类");
    }
}
```

```java
public class Student extends Person{
    public void ziShow(){
        System.out.println("子类");
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Person p=new Person();
        p.show();
        Student s=new Student();
        s.show();
    }
}
```

继承提高了代码的复用性和维护性，但是削弱了子类的独立性，类的耦合性增强了

只有在A，B类是包含关系的时候才使用继承，不能滥用

（虽然猫狗具有某些相同的特性，但是不能用猫继承狗）

**继承中构造方法的调用**

父类先调用无参数构造方法，再调用子类的对于构造方法

如果要用带参数的父类构造方法初始化，可以用 super\(参数\)

**方法重写**

子类不能重写父类的private方法，可以用@Override注解检验

#### 包，import，修饰符

package 包

import 包.类

**权限修饰符**

本类：public，protested，默认，private

同包的子类：public，protested，默认

同包的其他类：public，protested，默认

不同包的子类：public，protested

不同包的其他类：public

**状态修饰符**

final 修饰的基本变量不可改变（常量）

static修饰的基本变量被对象共享（静态）

可以直接使用 类.变量=。。。

非静态的成员方法可以访问所有的静态和非静态成员变量和方法

静态的成员方法只能访问静态的成员变量和方法

#### 多态

同一个对象在不同时刻表现出的不同状态

多态的形式访问成员变量要看父类

如果有重写方法，才会访问子类的成员方法

#### 抽象类和抽象方法

abstract

子类要重写所有的抽象方法

如果不重写抽象方法，子类也要是抽象类

```java
public abstract class Animal{
    public abstract void eat();
    public void sleep(){
        System.out.pirntln("睡觉");
    }
}
```

```java
public class Cat extends Animal{
    @override
    public void eat(){
        System.out.pirntln("猫吃鱼");
    }
}
```

```java
public class AnimalDemo{
    public static void main(String[] args){
        Animal a=new Cat;
        a.eat();
        a.sleep();
    }
}
```

#### 接口

interface

类实现接口：implements

* 接口中的成员变量默认用public，final和static修饰，即

```java
int num=10;

//相当于

public final static int num=10;
```

* 接口没有构造方法
* 接口可以多继承，类可以实现多个接口

```java
public class Interlmpl implements Inter1,Inter2,Inter3{

}
```

* 接口的多态

```java
public abstract interface Jumpping {
    public abstract void jump();
}
```

```java
public class Cat implements Jumpping{
    @Override
    public void jump() {
        System.out.println("猫可以跳高了");
    }
}
```

```java
public class JumppingDemo {
    public static void main(String[] args) {
        Jumpping j=new Cat();
        j.jump();
    }
}
```

**抽象类和接口的区别**

```java
//抽象类
public abstract class Door{
    public abstract void open(); //开
    public abstract void close(); //关
    public abstract void alarm(); //报警
}
```

```java
//接口
public interface Door{
    void open(); //开
    void colse(); //关
    void alarm(); //报警
}
```

但是这样的设计不合理，应该如下设计

```java
//抽象类
public abstract class Door{
    public abstract void open(); //开
    public abstract void close(); //关
}
//接口
public interface Alarm{
    void alarm(); //报警
}
//实例
public class AlarmDoor extends Door implements Alarm{
    @override
    public void open(){
        //...
    }
    @override
    public void close(){
        //...
    }
    @override
    public void alarm(){
        //...
    }
}
```

即抽象类是对事物的抽象，而接口是得行为的抽象

#### 形参和返回值

```java
public class Cat{
    public void eat(){
        System.out.println("猫吃鱼");
    }
}
```

```java
public class CatOperator{
    public void useCat(Cat c){
        c.eat();
    }
    public Cat getCat(){
        Cat c = new Cat();
        return c;
    }
}
```

```java
public class CatDemo{
    piblic static void main(String[] arg){
        CatOperator co = new CatOperator();
        Cat c1 = new Cat();
        co.useCat(c);
        Cat c2 = co.getCat();
        c2.eat();
    }
}
```

如果是抽象类或者接口，要用多态的方式创建子类对象

#### 内部类

* 内部类可以直接访问外部类成员，包括私有
* 外部类要访问内部类成员，必须要创建内部类对象
* 成员内部类：

```java
public class Computer {
    private int num=10;

    private class Cpu{
        public void show(){
            System.out.println(num);
        }
    }

    public void method() {
        Cpu c=new Cpu();
        c.show();
    }
}
```

* 局部内部类：

```java
public class Computer {
    private int num=10;


    public void method() {
        class Cpu{
            public void show(){
                System.out.println(num);
            }
        }

        Cpu cpu=new Cpu();
        cpu.show();
    }
}
```

* 匿名内部类

```java
public class Computer {
    private int num=10;


    public void method() {
        Inter i=new Inter(){
            @Override
            public void show() {
                System.out.println("匿名内部类");
            }
        };
        i.show();
    }
}
```

#### 常用的API

**Math**

min\(两个数的较小数\)

max（两个数的较大数）

abs（绝对值）

random（\[0,1\)的随机数）

**System**

exit（退出java虚拟机）

currentTimeMillis（返回当前时间）

**Object**

toString\(可以输出类的内容\)

equals（比较类是否相同）

**Arrays**

toString\(返回数组的字符串格式\)

sort（排列数组）

```java
import java.util.Arrays;
public class cyAPI {
    public static void main(String[] args) {
        System.out.println(Math.abs(-88));
        System.out.println(Math.max(88, 66));
        System.out.println(Math.random());

        long start =System.currentTimeMillis();
        for (int i=0;i<10000;i++){
            System.out.println(i);
        }
        long end=System.currentTimeMillis();
        System.out.println(end-start);

        int[] a={1,2,6,3,5,11,2,6,4};
        System.out.println(Arrays.toString(a));
        Arrays.sort(a);
        System.out.println(Arrays.toString(a));
    }
}
```

