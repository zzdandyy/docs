---
description: '2021-02-04'
---

# day11

## 完成情况

{% hint style="success" %}
每一天都学习并记录
{% endhint %}

{% hint style="warning" %}
每天看至少3个leetcode的题目
{% endhint %}

## 学习内容

## DOM简单学习

功能：控制html文档的内容

获取页面标签（元素）对象 Element

* document.getElementById\("id值"\)  :   通过元素的id获取元素对象
* 设置属性
* 修改标签体

## BOM

概念：浏览器对象模型 Brower Object Model

Window：窗口对象

History：历史记录对象

Location：地址栏对象

Navigator：浏览器对象

Screen：显示器屏幕对象

* Window

  对象不需要创建，可以直接使用window.调用

  window.也可以省略，直接使用 方法名\(\)

  * 弹出框

    alect\(\) 警告框

    confirm\(\) 确定或取消的对话框

    prompt\(\) 可以输入信息的对话框

  * 打开关闭有关的方法

    open\(\) 打开一个页面

    close\(\) 谁调用就关闭谁

  * 定时器

    setTimeout\(\) 在一定毫秒数之后执行函数

    clearTimeout\(\) 清除Timeout定时器

    setInterval\(\) 隔一定毫秒数之后执行函数

    clearInterval\(\) 清除Interval定时器

  * 获取其他的BOM对象

    History

    Location

  * 获取DOM对象

    document

  ```markup
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>DOM_01</title>
  </head>
  <body>
  <input id="open" type="button" value="打开窗口"><input/>
  <img src="img/off.gif" id="light">

  <script>
      var light = document.getElementById("light");
      var flog = false;
      light.onclick = function () {
          if (!flog) {
              light.src = "img/on.gif";
              flog = true;
          } else {
              light.src = "img/off.gif";
              flog = false;
          }
      }
      var flag = confirm("确定要退出吗？");
      var result = prompt("请输入邀请码");

      var openBtn = document.getElementById("open");
      openBtn.onclick = function () {
          open("https://www.baidu.com");
      }

      //定时器
      function fun() {
          alert("boom~~~~")
      }

      var timeout = setTimeout(fun, 3000);
      clearTimeout(timeout);
      var interval = setInterval(fun, 2000);
      clearInterval(interval);
  </script>
  </body>
  </html>

  ```

Location

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>BOM_03</title>
</head>
<body>
    <button id="button1">刷新</button>
    <button id="button2">百度</button>
    <script>
        let btn1 = document.getElementById("button1");
        btn1.onclick=function (){
            //刷新页面
            location.reload();
        }
        let btn2 = document.getElementById("button2");
        btn2.onclick=function (){
            //重置地址
            location.href="https://www.baidu.com";
        }
    </script>
</body>
</html>
```

跳转回主页

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>BOM_04</title>
    <style>
        #text {
            text-align: center;
        }

        #num {
            color: red;
        }
    </style>
</head>
<body>
<p id="text">
    <span id="num">5</span>秒之后,自动跳转到首页
</p>
<script>
    let number = 4;
    let num = document.getElementById("num");

    function fun() {
        num.innerHTML = number;
        number--;
        if (number === 0) {
            location.href = "https://www.baidu.com";
        }
    }

    setInterval(fun, 1000);
</script>
</body>
</html>
```

轮播图

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>BOM_01</title>
</head>
<body>
<img src="img/banner_1.jpg" id="img" alt="轮播图">


<script>
    let img = document.getElementById("img");
    let number = 1;
    setInterval(fun, 3000);

    function fun() {
        if (number === 4) {
            number = 1;
        }
        img.src = "img/banner_" + number + ".jpg";
        number++;
    }
</script>
</body>
</html>
```

History

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>BOM_05</title>
</head>
<body>
<button id="bt1">历史记录个数</button>
<button id="bt2">前进</button>
<button id="bt3">后退</button>
<button id="bt4">去往</button>
<script>
    let myhref = location.href;
    let bt1 = document.getElementById("bt1");
    let bt2 = document.getElementById("bt2");
    let bt3 = document.getElementById("bt3");
    let bt4 = document.getElementById("bt4");
    bt1.onclick = function () {
        alert(history.length);
    }
    bt2.onclick = function () {
        history.forward();
    }
    bt3.onclick = function () {
        history.back();
    }
    bt3.onclick = function () {
        //正数：前进几个页面，负数：后退几个页面
        history.go(2);
    }
</script>
</body>
</html>
```



### DOM

文档对象模型 Document Object Model

dom树

核心DOM----1.XML DOM----2.HTML.DOM

#### 核心DOM

* Document：文档对象
* Element：元素对象
* Attribute：属性对象
* Text：文本对象
* Comment：注释对象
* Node：结点对象，其他对象的父对象

Document

获取Element对象

* getElementById 根据id获取元素对象
* getElementsByTagName 根据元素名称获取元素对象们，返回数组
* getElementsByClassname 根据元素class获取元素对象们，返回数组
* getElementsByName 根据元素name获取元素对象们，返回数组

创建DOM对象

* createElement等等

Element

* 通过document来获取和创建
* removeAttribute 移除属性
* setAttribute 添加属性

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>DOM_03</title>
</head>
<body>
<a>点我试一试</a>
<button id="but_set">设置属性</button>
<button id="but_remove">删除属性</button>
<script>
    let set_button = document.getElementsByTagName("button")[0];
    set_button.onclick = function () {
        let element_a = document.getElementsByTagName("a")[0];
        //设置属性
        element_a.setAttribute("href", "https://www.baidu.com")
    }
    let remove_button = document.getElementsByTagName("button")[1];
    remove_button.onclick = function () {
        let element_a = document.getElementsByTagName("a")[0];
        // 删除属性
        element_a.removeAttribute("href");
    }
</script>
</body>
</html>
```

Node 结点对象

方法 CRUD dom树

* appendChild 添加子节点
* removeChild 删除子节点
* replaceChild 替换子节点

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>DOM_04</title>
    <style>
        div {
            border: 1px solid red;
        }

        #div1 {
            width: 200px;
            height: 200px;
        }

        #div2 {
            width: 100px;
            height: 100px;
        }

        #div3 {
            width: 50px;
            height: 50px;
        }
    </style>
</head>
<body>
<div id="div1">
    <div id="div2">div2</div>
    div1
</div>
<a id="del" href="javascript:void(0)">删除子节点</a>
<a id="add" href="javascript:void(0)">添加子节点</a>

<script>
    let element_del = document.getElementById("del");
    element_del.onclick = function () {
        let div1 = document.getElementById("div1");
        let div2 = document.getElementById("div2");
        div1.removeChild(div2)
    }

    let element_add = document.getElementById("add");
    element_add.onclick = function () {
        let div1 = document.getElementById("div1");
        let div3 = document.createElement("div");
        div3.setAttribute("id", "div3");
        div1.appendChild(div3);
    }

    let div2 = document.getElementById("div2");
    let div2_parent = div2.parentNode;
    alert(div2_parent);
</script>
</body>
</html><!DOCTYPE html>

```

动态表格

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>动态表格</title>

    <style>
        table {
            border: 1px solid;
            margin: auto;
            width: 500px;
        }

        td, th {
            text-align: center;
            border: 1px solid;
        }

        div {
            text-align: center;
            margin: 50px;
        }
    </style>

</head>
<body>

<div>
    <input type="text" id="id" placeholder="请输入编号">
    <input type="text" id="name" placeholder="请输入姓名">
    <input type="text" id="gender" placeholder="请输入性别">
    <input type="button" value="添加" id="btn_add">

</div>


<table>
    <caption>学生信息表</caption>
    <tr>
        <th>编号</th>
        <th>姓名</th>
        <th>性别</th>
        <th>操作</th>
    </tr>

    <tr>
        <td>1</td>
        <td>令狐冲</td>
        <td>男</td>
        <td><a href="javascript:void(0)" onclick=delTr(this)>删除</a></td>
    </tr>

    <tr>
        <td>2</td>
        <td>任我行</td>
        <td>男</td>
        <td><a href="javascript:void(0)" onclick=delTr(this)>删除</a></td>
    </tr>

    <tr>
        <td>3</td>
        <td>岳不群</td>
        <td>?</td>
        <td><a href="javascript:void(0)" onclick=delTr(this)>删除</a></td>
    </tr>


</table>

<script>
    /*
    1.添加
    给添加按钮绑定单机事件
    获取文本框的内容
    创建td，td文本是文本框的内容
    td添加到td中
     */
    document.getElementById("btn_add").onclick = function () {
        let id = document.getElementById("id").value;
        let name = document.getElementById("name").value;
        let gender = document.getElementById("gender").value;
        let td_id = document.createElement("td");
        let text_id = document.createTextNode(id);
        td_id.appendChild(text_id);
        let td_name = document.createElement("td");
        let text_name = document.createTextNode(name);
        td_name.appendChild(text_name);
        let td_gender = document.createElement("td");
        let text_gender = document.createTextNode(gender);
        td_gender.appendChild(text_gender);
        let td_a = document.createElement("td");
        let ele_a = document.createElement("a");
        ele_a.setAttribute("href", "javascript:void(0)");
        ele_a.setAttribute("onclick", "delTr(this)");
        let text_a = document.createTextNode("删除");
        ele_a.appendChild(text_a);
        td_a.appendChild(ele_a);
        //创建dr
        let tr = document.createElement("tr");
        tr.appendChild(td_id);
        tr.appendChild(td_name);
        tr.appendChild(td_gender);
        tr.appendChild(td_a);
        let table = document.getElementsByTagName("table")[0];
        table.appendChild(tr);
    }

    /*
    删除
     */
    function delTr(obj) {
        let table = obj.parentNode.parentNode.parentNode;
        let tr = obj.parentNode.parentNode;
        table.removeChild(tr);
    }
</script>

</body>
</html>
```

#### HTML DOM

```markup
document.getElementById("btn_add").onclick = function () {
    let id = document.getElementById("id").value;
    let name = document.getElementById("name").value;
    let gender = document.getElementById("gender").value;
    let table = document.getElementsByTagName("table")[0];
    table.innerHTML += "<tr>\n" +
        "        <td>" + id + "</td>\n" +
        "        <td>" + name + "</td>\n" +
        "        <td>" + gender + "</td>\n" +
        "        <td><a href=\"javascript:void(0)\" onclick=delTr(this)>删除</a></td>\n" +
        "    </tr>"
}
```

控制样式

