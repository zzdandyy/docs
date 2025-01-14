---
description: '2021-02-13'
---

# day20

## 完成情况

每一天都学习并记录


每天看至少3个leetcode的题目


## 学习内容

### JQuery

概念：JavaScript框架，封装了js的原生代码

* JQuery对象与js对象的转换

```javascript
<script>
    let divs = document.getElementsByTagName("div");
    let $divs = $("div");
    //对divs的所有内容，将其标签体的内容，变成aaa
    for (let i = 0; i < divs.length; i++) {
        divs[i].innerHTML = "hello"
    }
    //变成bbb
    $divs.html("bbb");
    //JQuery对象在操作时更加方便
    //Jquery对象和js对象的方法是不通用的
    //js->jq   ${js对象}
    //jq->js   jq对象[索引]
</script>
```

#### 选择器

* 事件绑定
* 入口函数
* 样式控制

### AJAX

概念：异步的JavaScript和xml

无需重新加载整个网页的情况下，能够更新部分网页的技术

* 异步和同步：建立在客户端和服务器端的通信
  * 同步：客户端必须等待服务器端的响应，在等待期间，客户端不能做其他操作
  * 异步：客户端不需要等待服务器端的响应

实现方式：

* 原生的js实现方式

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
  </head>
  <script>
      function fun() {
          var xmlhttp;
          if (window.XMLHttpRequest) {
              xmlhttp = new XMLHttpRequest();
          } else {
              xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
          }
          //建立连接
          xmlhttp.open("GET", "ajaxServlet?username=tom", true)
          //发送请求
          //GET发送空参数，POST要加参数
          // xmlhttp.send("username=tom")
          xmlhttp.send();
          //接收并处理来着服务器的响应结果
          xmlhttp.onreadystatechange = function () {
              if (xmlhttp.readyState == 4 && xmlhttp.status == 200) {
                  let responseText = xmlhttp.responseText;
                  alert(responseText);
              }
          }
      }
  </script>
  <body>
  <input type="button" value="附送异步请求" onclick="fun();">
  <input type="text" id="input">
  </body>
  </html>
  ```

* Jquery实现方式
  * $.ajax\(\)

    ```javascript
    function fun(){
            $.ajax({
                url:"ajaxServlet",
                type:"POST",
                data:{"username":"jack","age":23},
                success:function (data){
                    //响应成功后的回调函数
                    alert(data);
                },
                error:function () {
                  //请求响应出错的回调函数
                  alert("出错啦")
                },
                //设置接收到的数据格式的取值
                dataType:"json"
            })
        }
    ```

  * $.get\(\)
  * &.post\(\)

    ```javascript
    function fun2(){
        $.get("ajaxServlet",{username: "rose"},function (){},"json")
        $.post("ajaxServlet",{username: "rose"},function (){},"json")
    }
    ```

### JSON

概念：javaScript对象表示法

let p={"name":"张三","age":18,"gender":"男"}；

* 存储和交换数据的文本格式
* 比xml更小，更快，更易于解析

语法

* 基本规则
  * 数据在名称/值对中：json的数据是由键值对构成的
    * 键用引号包裹，可以不可以使用引号
    * 数字，字符串（""包括），逻辑值，数组，json对象（多重嵌套）
  * 数据由逗号分割
  * 花括号保存对象:使用{}定义json
  * 方括号表示数组:\[\]
* 获取数据
  * json对象.键名
  * json对象\[”键名"\]
  * 数组对象\[索引\]

    ```javascript
    <script>
        let person = {name: "张三", gender: "男", age: 18}

        for (let key in person) {
            alert(key)
            alert(person[key])
        }

    </script>
    ```

**JSON解析器**

Jackson

**Java对象转JSON**

```java
public class JacksonTest {
    @Test
    public void test1() throws IOException {
        Person p = new Person();
        p.setName("张三");
        p.setAge(19);
        p.setGender("男");
        ObjectMapper mapper = new ObjectMapper();
        /**
         * 转换方法：
         * writeValue(obj)
         *  File:将obj对象转换为JSON字符串，并保存到指定的文件中
         *  Writer：将obj对象转换为JSON字符串，并填充到字符输出流
         *  OutputStream：将obj对象转换为JSON字符串，并填充到字节输出流
         * writeValueAsString(obj)
         */
        String json = mapper.writeValueAsString(p);
        System.out.println(json);
        mapper.writeValue(new FileWriter("a.txt"), p);
    }
}
```

注解

```java
@JsonIgnore //忽略某个属性
private Date birthday;


@JsonFormat(pattern = "yyyy-MM-dd") //格式化
private Date birthday;
```

数组和map的转换

```java
@Test
public void test2() throws IOException {
    Person p1 = new Person();
    p1.setName("张三");
    p1.setAge(19);
    p1.setGender("男");
    p1.setBirthday(new Date());

    Person p2 = new Person();
    p2.setName("李四");
    p2.setAge(24);
    p2.setGender("女");
    p2.setBirthday(new Date());

    Person p3 = new Person();
    p3.setName("王五");
    p3.setAge(17);
    p3.setGender("男");
    p3.setBirthday(new Date());

    //集合
    List<Person> ps = new ArrayList<>();
    ps.add(p1);
    ps.add(p2);
    ps.add(p3);

    ObjectMapper mapper = new ObjectMapper();
    String json = mapper.writeValueAsString(ps);
    //数组里面由三个json对象
    System.out.println(json);
}

@Test
public void test3() throws IOException {

    Map<String,Object> map=new HashMap<>();
    map.put("name","张三");
    map.put("age",18);
    map.put("gender","男");


    ObjectMapper mapper = new ObjectMapper();
    String json = mapper.writeValueAsString(map);
    //json对象
    System.out.println(json);
}
```

**JSON转Java对象**

```java
@Test
public void test4() throws IOException {
    String json = "{\"gender\":\"男\",\"name\":\"张三\",\"age\":18}";
    ObjectMapper mapper = new ObjectMapper();
    Person person = mapper.readValue(json, Person.class);
    System.out.println(person);
}
```

校验用户名是否存在

