---
description: '2021-02-06'
---

# day13

## 完成情况

每一天都学习并记录


每天看至少3个leetcode的题目


## 学习内容

## XML

* 第一行必须是文档声明
* xml文档中有且仅有一个根标签
* 标签要有结束标签或者是自闭合标签
* 标签区分大小写

文档声明

```html
<?xml version='1.0' encoding="utf-8"?>
```

标签是自定义的，但是有一定的规则，id值唯一

* 可以包含字母，数字，其他字符
* 不能以数字和符号开始
* 不能以xml开始
* 名称不能包含空格

框架的使用者

* 在xml中引入约束文档
* 简单的读懂约束文档

**约束文档**

* DTD：简单的约束文档
  * ```text
    <!ELEMENT students (student*) >
    <!ELEMENT student (name,age,sex)>
    <!ELEMENT name (#PCDATA)>
    <!ELEMENT age (#PCDATA)>
    <!ELEMENT sex (#PCDATA)>
    <!ATTLIST student number ID #REQUIRED>
    ```
  * 引入DTD文档
  * ```html
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE students SYSTEM "student.dtd">

    <students>
        <student number="s001">
            <name>tom</name>
            <age>18</age>
            <sex>male</sex>
        </student>

    </students>
    ```
* Schema 较复杂的约束文档
  * ```html
    <?xml version="1.0"?>
    <xsd:schema xmlns="http://www.itcast.cn/xml"
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"
            targetNamespace="http://www.itcast.cn/xml" elementFormDefault="qualified">
        <xsd:element name="students" type="studentsType"/>
        <xsd:complexType name="studentsType">
            <xsd:sequence>
                <xsd:element name="student" type="studentType" minOccurs="0" maxOccurs="unbounded"/>
            </xsd:sequence>
        </xsd:complexType>
        <xsd:complexType name="studentType">
            <xsd:sequence>
                <xsd:element name="name" type="xsd:string"/>
                <xsd:element name="age" type="ageType" />
                <xsd:element name="sex" type="sexType" />
            </xsd:sequence>
            <xsd:attribute name="number" type="numberType" use="required"/>
        </xsd:complexType>
        <xsd:simpleType name="sexType">
            <xsd:restriction base="xsd:string">
                <xsd:enumeration value="male"/>
                <xsd:enumeration value="female"/>
            </xsd:restriction>
        </xsd:simpleType>
        <xsd:simpleType name="ageType">
            <xsd:restriction base="xsd:integer">
                <xsd:minInclusive value="0"/>
                <xsd:maxInclusive value="256"/>
            </xsd:restriction>
        </xsd:simpleType>
        <xsd:simpleType name="numberType">
            <xsd:restriction base="xsd:string">
                <xsd:pattern value="heima_\d{4}"/>
            </xsd:restriction>
        </xsd:simpleType>
    </xsd:schema>
    ```

    \`\`\`xml &lt;?xml version="1.0" encoding="UTF-8" ?&gt; &lt;!-- 1.填写xml文档的根元素 2.引入xsi前缀. xmlns:xsi="[http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)" 3.引入xsd文件命名空间. xsi:schemaLocation="[http://www.itcast.cn/xml](http://www.itcast.cn/xml) student.xsd" 4.为每一个xsd约束声明一个前缀,作为标识 xmlns="[http://www.itcast.cn/xml](http://www.itcast.cn/xml)"

```text
 -->
<students xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.itcast.cn/xml  student.xsd"
          xmlns="http://www.itcast.cn/xml"
>
    <student number="heima_0001">
        <name>张三</name>
        <age>22</age>
        <sex>female</sex>
    </student>
</students>
```
```

**解析xml文档思想**

* DOM：将标记语言文档一次性加载进内存，形成一个DOM树（服务器）
  * 优点：操作方便，可以对文档进行CRUD的所有操作
  * 缺点：占内存
* SAX ：逐行读取，基于事件驱动（手机）
  * 优点：占内容少
  * 缺点：只能读取，不能增删改

xml常见的解析器

* JAXP：sun公司提供的解析器，支持dom和sax两种思想
* DOM4J：非常优秀的解析器，支持dom
* Jsoup：一般用来解析html，但是也可以解析xml，支持dom
* PULL：Android操作系统内置的解析器，支持SAX

**Jsoup**

```java
public class JsoupDemo01 {
    //快速入门
    public static void main(String[] args) throws IOException {
        //获取Document对象，根据xml文档获取
        Document document = Jsoup.parse(new File("src\\day13\\study2\\student.xml"), "utf-8");
        //获取Element对象，返回的是一个集合
        Elements elements = document.getElementsByTag("name");
        Element element = elements.get(0);
        String name = element.text();
        System.out.println(name);
        System.out.println("--------");
        //属性名为id的对象
        Elements id = document.getElementsByAttribute("id");
        System.out.println(id);
        System.out.println("--------");
        //属性名为number且值为s001的对象
        Elements elementsByAttributeValue = document.getElementsByAttributeValue("number", "s001");
        System.out.println(elementsByAttributeValue);
        System.out.println("--------");
        //id属性值的元素对象
        Element name01 = document.getElementById("name01");
        System.out.println(name01);
    }
}
```

```java
public class JsoupDemo02 {
    public static void main(String[] args) throws IOException {
        //获取Document对象，根据xml文档获取
        Document document = Jsoup.parse(new File("src\\day13\\study2\\student.xml"), "utf-8");
        //获取属性值  attr
        Elements students = document.getElementsByTag("student");
        for (Element student : students) {
            System.out.println(student.attr("number"));
        }
        System.out.println("--------");
        //获取子标签的值
        System.out.println(students.get(0).getElementsByTag("name"));
        System.out.println("--------");
        //获取所有子标签的纯文本内容
        for (Element student : students) {
            System.out.println(student.text());
        }
        System.out.println("--------");
        //获取所有子标签的html字符串
        for (Element student : students) {
            System.out.println(student.html());
        }
    }
}
```

根据选择器查询

```java
public class JsoupDemo03 {
    public static void main(String[] args) throws IOException {
        Document document = Jsoup.parse(new File("src\\day13\\study2\\student.xml"), "utf-8");
        //获取标签为name的
        Elements elements01 = document.select("name");
        System.out.println(elements01);
        System.out.println("--------");
        //获取id值为name01的
        Elements elements02 = document.select("#name01");
        System.out.println(elements02);
        System.out.println("--------");
        //获取student标签number值为s0001下的age子标签
        Elements elements03 = document.select("student[number=\"s001\"] > age");
        System.out.println(elements03);
    }
}
```

根据Xpath查询

```java
public class JsoupDemo04 {
    public static void main(String[] args) throws IOException, XpathSyntaxErrorException {
        Document document = Jsoup.parse(new File("src\\day13\\study2\\student.xml"), "utf-8");
        //根据Document对象创建JXDocument对象
        JXDocument jxDocument = new JXDocument(document);
        //selN 查询某个标签，不考虑其在文档中的位置
        List<JXNode> jxNodes = jxDocument.selN("//student");
        for (JXNode jxNode : jxNodes) {
            System.out.println(jxNode);
        }
        System.out.println("--------");
        //查询student下的name标签
        List<JXNode> jxNodes1 = jxDocument.selN("//student/name");
        for (JXNode jxNode : jxNodes1) {
            System.out.println(jxNode);
        }
        System.out.println("--------");
        //查询student标签下带有id属性的name标签
        List<JXNode> jxNodes2 = jxDocument.selN("//student/name[@id]");
        for (JXNode jxNode : jxNodes2) {
            System.out.println(jxNode);
        }
        //查询student标签下带有id属性且值为name01的name标签
        System.out.println("--------");
        List<JXNode> jxNodes3 = jxDocument.selN("//student/name[@id='name01']");
        for (JXNode jxNode : jxNodes3) {
            System.out.println(jxNode);
        }
    }
}
```

### Tomcat

服务器：安装了服务器软件的计算机

常见的服务器软件（java相关）

webLogic：oracle公司，大型的JavaEE服务器，支持所有的JavaEE规范，收费的

* JavaEE，Java语言在企业级开发中使用的技术规范的总和，一共规定了13项的规范

webSphere：IBM公司，大型的JavaEE服务器，支持所有的JavaEE规范，收费的

JBOSS：JBOSS公司，大型的JavaEE服务器，支持所有的JavaEE规范，收费的

Tomcat：Apache基金组织，中小型的JavaEE服务器，仅仅支持少量的Java规范，开源免费

#### Tomcat目录

bin 可执行文件

conf 配置文件

lib 依赖的jar包

logs 日志文件

temp 临时文件

webapps 存放web项目

work 运行时的数据

### Servlet

就是一个接口，定义了Java类被浏览器访问到（tomcat识别）的规则

我们要定义一个类，实现Servlet，复写方法

在web.xml内配置

```html
<servlet>
    <servlet-name>demo1</servlet-name>
    <servlet-class>com.example.web.ServletDemo</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>demo1</servlet-name>
    <url-pattern>/demo1</url-pattern>
</servlet-mapping>
```

Servlet 3.0起，可以使用注解

```java
@WebServlet(name="demo3",value = "/demo3")
```

* 执行原理

1.服务器接收到客户端浏览器请求后，会解析URL路径，获取资源路径

2.根据资源路径去web.xml中遍历，看是否有对应的内容

3.如果有，会去找中的全类名

4.tomcat会将字节码文件加载到内容中，创建其对象

5.调用其方法

* Servlet的生命周期
  * 被创建，执行init\(\)方法，只执行一次
  * 提供服务，执行service\(\)方法，可执行多次
  * 被销毁，执行destroy\(\)方法，只执行一次
* init\(\)
  * 默认情况下，第一次被访问时，被创建
  * 可以配置

    ```html
    servlet>
            <servlet-name>demo2</servlet-name>
            <servlet-class>com.example.web.ServletDemo02</servlet-class>
    <!--        指定Servlet的创建时机
                1.第一次被访问时，创建
                <load-on-startup>的值是负数
                2.服务器启动时，创建
                <load-on-startup>的值是非负整数
    -->
            <load-on-startup>5</load-on-startup>
        </servlet>
    ```

  * Servlet在内存中只存在一个对象，是单例的
  * 多个用户同时访问时，可能存在线程安全问题
* service\(\)
  * Servlet每次被访问时都会执行
* destroy\(\)
  * 服务器被正常关闭时执行
  * Servlet被销毁之前执行，一般用于释放资源

**Servlet体系结构**

Servlet---GenericServlet---HttpServlet

一般选择继承HttpServlet抽象类，其是对http协议的封装

