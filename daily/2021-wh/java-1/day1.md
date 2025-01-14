---
description: '2021-01-25'
---

# day1

## 完成情况

每一天都学习并记录


每天看至少3个leetcode的题目


## 学习内容

**八种基础数据类型**

byte

short

int

long

float

double

char

boolean

**命名规则**

小驼峰（变量，方法）

student

studentGet

大驼峰（类）

Student

StudentGet

**switch case**

case穿透

**for循环语句**

for\(int i=0;i&lt;5;i++\)

**输入和随机数**

```text
import java.util.Scanner;
import java.util.Random;
```

**IDEA快捷键**

快速生成main方法 psvm 回车

快速生成输出语句 sout 回车

#### **数组**

**数组动态初始化**

```java
int[] arr1 =new int[3];
```

**数组静态初始化**

```java
int[] arr2 =new int[]{1,2,3,...};
​
int[] arr3 ={1,2,3,...};  //简化
```

**遍历数组**

```java
int[] arr4={1,2,3,4,5,...};
for(int i=0;i<arr.length;i++){
    System.out.println(arr4[i]);
}
```

**数组的最值**

```java
int[] arr5={12,36,81,4,96,22,...};
int maxArr=arr5[0];
for(int i=1;i<arr.length;x++){
    if(arr5[i]>maxArr){
        maxArr=arr5[i];
    }
}
System.out.println(maxArr);
```

**方法**

```java
public class Method {
    public void getMAxNum (int a,int b) {
        System.out.println(Math.max(a,b));
    }
    public int getMinNum(int c,int d){
        return Math.min(c,d);
    }
}
```

**方法重载**

```java
public static int sum(int a,int b){
    return a+b;
}
public static double sum(double a,double b){
    return a+b;
}
public static int sum(int a,int b,int c){
    return a+b+c;
}
```

**类和方法**

```java
public class Student {
    String name;
    int age;
​
    public void study(){
        System.out.println("学习");
    }
    public void doHomeWork(){
        System.out.println("做作业");
    }
​
    public static void main(String[] args) {
        Student a=new Student();
        a.name="小王";
        a.age=22;
        System.out.println(a.name);
        System.out.println(a.age);
        a.study();
        a.doHomeWork();
    }
}
```

