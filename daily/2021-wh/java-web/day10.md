---
description: '2021-02-03'
---

# day10

## 完成情况

{% hint style="success" %}
每一天都学习并记录
{% endhint %}

{% hint style="warning" %}
每天看至少3个leetcode的题目
{% endhint %}

## 学习内容

### HTML

展示页面内容

表单

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>旅游网</title>
</head>
<body>
<!--
    <form> 采集数据的范围
    action 提交数据的URL
    method 提交方式
        get：请求参数会在地址栏中显示，会封装到请求行中
             请求参数的长度是有限制的
             不太安全

        post：请求参数不会在地址里中显示。会封装在请求体中
              请求参数的长度没有限制
              较为安全
-->
<form action="#" method="get">

    <!--        for 和input的id属性对应，可以获取焦点-->
    <!--        placeholder 输入框的提示信息-->
    <label for="username">
        用户名
        <input type="text" name="username" placeholder="请输入用户名" id="username">
    </label><br>
    <label for="password">
        密码
        <input type="password" name="password" placeholder="请输入密码" id="password">
    </label><br>

    <!--        radio 单选框-->
    <!--        checked 标注为默认值-->
    性别 <input type="radio" name="gender" value="man" checked="checked"> 男
    <!--        两个单选框构成一组，name要一样-->
    <input type="radio" name="gender" value="female"> 女 <br>

    <!--        复选框-->
    爱好 <input type="checkbox" name="hobby" value="shopping"> 逛街
    <input type="checkbox" name="hobby" value="game" checked="checked"> 玩游戏
    <input type="checkbox" name="hobby" value="study"> 学习 <br>

    <!--        文件选择框-->
    图片 <input type="file" name="file"><br>

    <!--        隐藏域-->
    隐藏域 <input type="hidden" name="hidden"><br>

    <!--        图片按钮-->
    按钮 <input type="button" name="button"><br>

    <!--        图片按钮-->
    图片按钮 <input type="img" src="image/icon_5.jpg" name="imgbutton"><br>

    <!--        取色器-->
    取色器 <input type="color" name="color"><br>

    <!--        时间-->
    时间 <input type="time" name="time"><br>

    <!--        日期-->
    日期1 <input type="date" name="date"><br>

    <!--        日期-->
    日期2 <input type="datetime-local" name="datetime_time"><br>

    <!--        下拉列表-->
    省份 <select name="select">
    <option id="">请选择</option>
    <option id="1">广州</option>
    <option id="2">汕头</option>
    <option id="3">北京</option>
    <br>

    <!--        文本框-->
    自我描述 <textarea cols="10" rows="10"></textarea>
</select>
    <input type="submit" value="登录">
</form>
</body>
</html>
```

注册页面

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>login01</title>
</head>
<body>
<!--    定义表单-->
<form action="#" method="get">
    <table border="1" align="center" width="500">
        <tr>
            <td><label for="username">用户名</label></td>
            <td><input type="text" name="username" id="username"></td>
        </tr>
        <tr>
            <td><label for="password">密码</label></td>
            <td><input type="password" name="password" id="password"></td>
        </tr>
        <tr>
            <td><label for="email">Email</label></td>
            <td><input type="Email" name="email" id="email"></td>
        </tr>
        <tr>
            <td><label for="name">姓名</label></td>
            <td><input type="text" name="name" id="name"></td>
        </tr>
        <tr>
            <td><label for="phone_number">手机号</label></td>
            <td><input type="text" name="phone_number" id="phone_number"></td>
        </tr>
        <tr>
            <td><label>性别</label></td>
            <td>
                男 <input type="radio" name="username" value="man">
                女 <input type="radio" name="username" value="female">
            </td>
        </tr>
        <tr>
            <td><label for="birthday">出生日期</label></td>
            <td><input type="date" name="birthday" id="birthday"></td>
        </tr>
        <tr>
            <td><label>验证码</label></td>
            <td>
                <input type="text" name="check_code">
                <img src="image/icon_5.jpg">
            </td>
        </tr>
        <tr>
            <td colspan="2" align="center">
                <input type="submit" value="注册">
            </td>
        </tr>
    </table>
</form>
</body>
</html>
```

### CSS

美化页面，布局

```css
p{
    color: red;
    font-size:30px;
    align-content: center;
}

#div1{
    width: 400px;
    height: 400px;
    box-sizing: border-box;
    padding-left: 50px;
    padding-top: 50px;
}

#div2{
    width: 100px;
    height: 100px;
    /*margin-left: 50px;*/
    /*margin-top: 50px;*/
}

div{
    border: 1px solid red;
}

.class1{
    font-size: 80px;
    color:blue;
    text-align: center;
    line-height: 200px;
    /*
    border 边框
     */
    border: 1px solid red;
}
.class2{
    border: 1px solid red;
    height: 20px;
    width: 20px;
}
```

```css
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>login02</title>
    <style>

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: url("image/1.jpg") repeat;
        }

        .rg_layout {
            width: 900px;
            height: 500px;
            border: 5px solid #eeeeee;
            border-radius: 10px;
            background: white;
            /*水平居中*/
            margin: auto;
            margin-top: 80px;
        }

        .rg_left {
            float: left;
            margin: 15px;
        }

        .rg_center {
            background-color: #eeeeee;
            border-radius: 20px;
            float: left;
            width: 450px;
            margin-top: 30px;
        }

        .rg_right {
            margin: 10px;
            float: right;
        }

        .rg_left > p:first-child {
            color: #e0e022;
            font-size: 20px;
        }

        .rg_left > p:last-child {
            color: #b7aeae;
            font-size: 30px;
        }

        .rg_right > p:first-child {
            font-size: 15px;
        }

        .rg_right p a {
            font-size: 15px;
            color: pink;
        }

        .td_left {
            height: 45px;
            width: 100px;
            text-align: right;
        }

        .td_right {
            padding-left: 45px;
        }

        #username, #password, #birthday, #phone_number, #name, #email, #check_code {
            width: 251px;
            height: 32px;
            border: 1px solid #A6A6A6;
            /*圆角*/
            border-radius: 5px;
            padding-left: 15px;
        }

        #btn_sub {
            width: 150px;
            height: 40px;
            background-color: yellow;
            border: 1px solid yellow;
            margin-left: 150px;
        }
    </style>
</head>
<body>

<div class="rg_layout">
    <div class="rg_left">
        <p>新用户注册</p>
        <p>USER REGISTER</p>
    </div>

    <div class="rg_center">
        <form action="#" method="get">
            <table>
                <tr>
                    <td class="td_left"><label for="username">用户名</label></td>
                    <td class="td_right"><input type="text" name="username" id="username" placeholder="请输入账号"></td>
                </tr>
                <tr>
                    <td class="td_left"><label for="password">密码</label></td>
                    <td class="td_right"><input type="password" name="password" id="password" placeholder="请输入密码"></td>
                </tr>
                <tr>
                    <td class="td_left"><label for="email">Email</label></td>
                    <td class="td_right"><input type="Email" name="email" id="email" placeholder="请输入邮箱"></td>
                </tr>
                <tr>
                    <td class="td_left"><label for="name">姓名</label></td>
                    <td class="td_right"><input type="text" name="name" id="name" placeholder="请输入姓名"></td>
                </tr>
                <tr>
                    <td class="td_left"><label for="phone_number">手机号</label></td>
                    <td class="td_right"><input type="text" name="phone_number" id="phone_number" placeholder="请输入手机号">
                    </td>
                </tr>
                <tr>
                    <td class="td_left"><label>性别</label></td>
                    <td class="td_right">
                        <input type="radio" name="username" value="man"> 男
                        <input type="radio" name="username" value="female"> 女
                    </td>
                </tr>
                <tr>
                    <td class="td_left"><label for="birthday">出生日期</label></td>
                    <td class="td_right"><input type="date" name="birthday" id="birthday"></td>
                </tr>
                <tr>
                    <td class="td_left"><label for="check_code">验证码</label></td>
                    <td class="td_right">
                        <input type="text" name="check_code" placeholder="请输入验证码" id="check_code">
                    </td>
                </tr>
                <tr>
                    <td colspan="2">
                        <input type="submit" value="注册" id="btn_sub">
                    </td>
                </tr>
            </table>
        </form>
    </div>

    <div class="rg_right">
        <p>已有账号 <a href="#">立即登录</a></p>
    </div>

</div>


</body>
</html>
```

### JavaScript

网页脚本语言，控制元素，动态效果

1992年，Nombase公司 C--，后更名为ScriptEase

1995年，Netscape公司 LiveScript，请来了SUN公司的专家，修改LiveScript，命名为JavaScript

1996年，微软公司抄袭了JavaScript，开发出JScript语言

1997年，ECMA（欧洲计算机制造商协会）ECMAScript，所有客户端脚本语言的标准

JavaScript = ECMAScript + JavaScript 自己特有的东西（BOM+DOM）

#### ECMAScript

**基本语法**

* 与html的结合方式

  一般是放在最下面

  ```javascript
  <script>
      function displayDate() {
          document.getElementById("demo").innerHTML = Date();
      }
  </script>

  <script src=""></script>
  ```

* 注释

  ```javascript
  //单行注释

  /*
  注释
  多行注释
   */
  ```

* 数据类型
  * 原始数据类型

    number：整数/小数/NaN（not a number）

    string：字符串 "abc","a",'abc'

    boolean：True/false

    null：对象为空的占位符

    undefined：未定义，如果一个变量没有给初始化值，被默认赋值为undefined

  * 引用数据类型：对象
* 变量
  * JavaScript是弱类型变量
  * var 变量名=_\*_；
* 运算符

  == 等于 类型不同先转类型再比较

  === 全等于 先比较类型，如果类型不同，直接false

  number转boolean

  * number 0和NaN 为 false
  * string 空字符串 为 false
  * null和空的undefined 为 false

* 流程控制语句

  switch可以接受任意的原始数据类型

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>javascript01</title>

    <style>
        td {
            border: 1px solid;
        }
    </style>
    <script>
        document.write("<table align='center'>");
        //基本的for循环
        for (var i = 1; i <= 9; i++) {
            document.write("<tr>");
            for (var j = 1; j <= i; j++) {
                document.write("<td>");
                document.write(i + "*" + j + "=" + (i * j) + "&nbsp;&nbsp;&nbsp;");
                document.write("</td>");
            }
        }
    </script>
</head>
<body>
</body>
</html>
```

* 特殊语法
  * 一行如果只有一条语句时；可以省略，但是不建议这样子
  * var a=1； a=1；

    var定义的变量是局部变量

    不用var是全局变量

**基本对象**

Function：函数对象

```javascript
function fun1(a, b) {
    alert(a)
}

var fun2 = function (a, b) {
    alert(b)
}
fun1(1, 2);
fun2(1, 2);
//function的使用和参数无关，只和方法名有关
fun1();
fun2(1,2,3);
//内置对象
function add(){
    var sum =0;
    for (var i=0;i<arguments.length;i++){
        sum+=i;
    }
    return sum;
}
alert(add(1,2,5,6,7,9))
```

Array 数组对象

```javascript
var arr1 = new Array(1, 2, 3, 4);
var arr2 = new Array(5);
var arr3 = [1, 2, 3];
var arr4 = [1, "abc", true];
document.write(arr1 + "<br>");
document.write(arr2 + "<br>");
document.write(arr3 + "<br>");
document.write(arr4 + "<br>"); //数组元素是可变的
document.write(arr4[1] + "<br>"); //访问数组元素
arr4[10] = '666'
document.write(arr4.length + "<br>"); //自动改变数组的长度

//Array的方法
//join 安装某一分隔符拼接数组中的元素
document.write(arr1.join("-") + "<br>");
//push 添加元素
arr1.push(5);
document.write(arr1 + "<br>");
```

Date 日期对象

```javascript
//创建Date对象
var date=new Date();
//返回Date对象对应时间本地字符串格式 toLocaleString
document.write(date.toLocaleString()+"<br>");
//获取毫秒值
document.write(date.getTime());
```

Math 数学工具对象

```javascript
// Math不用创建对象
//PI值
document.write(Math.PI + "<br>");
// random  0-1之间的随机数 (0,1]
document.write(Math.random() + "<br>");
//round 最近的整数值
document.write(Math.round(3.4) + "<br>");
//ceil 向上取整
document.write(Math.ceil(3.2) + "<br>");
//floor 向下取整
document.write(Math.floor(3.8) + "<br>");
```

RegExp 正则表达式对象

```javascript
/*
1.单个字符
    [a]  [ab]  [a-zA-Z0-9_]
    特殊符号表示
    \d 单个数字字符 [0,9]
    \w 单个单词字符 [a-zA-Z0-9]
2.量词符号
    ?:表示出现0次或1次
    *:表示出现0次或多次
    +:表示出现1次或多次
    {m,n}:表示数量在[m,n]之间
        m如果缺省,数量最多n
        n如果缺省,数量最少m
3.开始接受符号
    ^开始
    $结束
4.创建RegExp对象
    var reg1 = new RegExp("正则表达式");
    var reg2 = /正则表达式/;
5.方法
    test()
*/
var reg1 = new RegExp("^\\w{6,12}$");
var reg2 = /^\w{6,12}$/;

var username="sssssdd5sss";
var flag=reg1.test(username);

alert(flag)
```

Global 全局对象 不需要对象就可以直接调用

```javascript
// encodeURI() URL编码
var s1 = encodeURI("http://www.baidu.com?wd=你好");
alert(s1);
// decodeURI() URL解码
var s2 = decodeURI("%E4%BD%A0%E5%A5%BD");
alert(s2)

// encodeURIComponent() URL编码 会编码更多的字符 ? : // 等等
let s3 = encodeURIComponent("http://www.baidu.com?wd=你好");
alert(s3);
// decodeURIComponent() URL解码
let s4 = decodeURIComponent("http%3A%2F%2Fwww.baidu.com%3Fwd%3D%E4%BD%A0%E5%A5%BD");
alert(s4);

// parseInt()  逐一判断每一个字符是否是数字直到不是数字为止，然后把前面的字符串转换为number
var str ="123abc"
var num=parseInt(str)
alert(num+1)

// isNaN() 判断一个值是不是NaN,原因是NaN参与运算，全为NaN，所以不能用==NaN

//解析代码片段并执行
var jscode="alert(123)";
eval(jscode);
```

