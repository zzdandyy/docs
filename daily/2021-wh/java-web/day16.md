---
description: '2021-02-09'
---

# day16

## 完成情况

{% hint style="success" %}
每一天都学习并记录
{% endhint %}

{% hint style="warning" %}
每天看至少3个leetcode的题目
{% endhint %}

## 学习内容

### JSP入门

Java Server Pages Java服务器端页面

* 可以定义html标签，也可以定义Java代码
* 用于简化书写
* 服务器会将.jsp文件转换为.java文件，然后编译成.class文件\(Servlet\)
* JSP本质上是一个servlet
* JSP脚本：定义Java代码的方式
  * &lt;% 代码 %&gt;：在Servlet方法中，Servlet方法中可以定义什么，该脚本就可以定义什么
  * &lt;%! 代码 %&gt; ：定义的是成员（成员变量，成员方法）
  * &lt;%= 代码 %&gt;：输出语句，会把内容的值输出到页面上
* JSP的内置对象
  * 在jsp页面中不需要获取和创建的对象
  * out和response.getWriter\(\)的区别
    * response.getWriter\(\)的输出在前
    * 尽量不用response.getWriter\(\)
* ```text
  <%@ page import="java.text.DateFormat" %>
  <%@ page import="java.text.SimpleDateFormat" %>
  <%@ page import="java.util.Date" %>
  <%@ page import="java.net.URLEncoder" %>
  <%@ page import="java.net.URLDecoder" %>
  <%@ page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
  <!DOCTYPE html>
  <html>
  <head>
      <title>JSP - Hello World</title>
  </head>
  <body>
  <h1><%= "Hello World!" %>
  </h1>
  <br/>
  <a href="day14/demo01">Hello Servlet</a>
  ​
  <%
      //设置cookie
      DateFormat dateFormat = new SimpleDateFormat("yyyy年MM月dd HH:mm:ss");
      String time = dateFormat.format(new Date());
      //编码
      time = URLEncoder.encode(time, "utf-8");
      Cookie c = new Cookie("lastTime", time);
  //        c.setMaxAge(30000);
      Cookie[] cs = request.getCookies();
      boolean isCome = false;
      for (Cookie cookie : cs) {
          if ("lastTime".equals(cookie.getName())) {
              //有cookie
              response.addCookie(c);
              isCome = true;
              //解码
              String value = URLDecoder.decode(cookie.getValue(), "utf-8");
              response.getWriter().write("欢迎回来，上一次访问时间为：" + value);
          }
      }
      if (!isCome) {
          //没有cookie
          response.addCookie(c);
          response.getWriter().write("欢迎你首次访问");
      }
  %>
  </body>
  </html>
  ```

### Session

HttpSession 域对象

```text
HttpSession session = request.getSession();
```

* Object getAttribute\(String\)
* void setAttribute\(String name\)
* Session是依赖于Cookie的
  * 第一次获取Session时，没有Cookie，会在内存中创建一个新的Session对象
  * 响应的时候，会set-cookie:JSESSIONID=......
  * 浏览器下次访问的时候，会在请求头中携带Cookie
  * 服务器会根据Cookie得到Session
* 细节
  * 客户端关闭后，服务器不关闭，两次获取Session是否是同一个
    * 默认不是同一个
    * 如果需要相同，可以创建Cookie，键为JSESSIONID，设置最大存活时间
  * 客户端不关闭，服务器关闭，两次获取的Session是否是同一个
    * 不是同一个，但是要确保数据不丢失
    * Session的钝化：在服务器正常关闭之前，将Session对象序列化到硬盘上
    * Session的活化：将Session文件转换为Session对象
    * tomcat已经自动完成了钝化和活化，在work目录中
    * 但是在idea开发的时候，每次重启服务器会删除work目录，即可以钝化但是不能活化
  * Session的失效时间
    * 服务器关闭
    * 调用invalidate\(\)方法
    * 有一个默认的失效时间：30min，但是可以在tomcat中配置
* 特点
  * Session用于存储一次会话的数据，存在于服务器
  * Session可以存储任意类型，任意大小的数据
  * 与Cookie的区别
    * session在服务器，cookie在客户端
    * session大小和数据类型没有限制，cookie有限制
    * session比较安全，cookie不够安全

登录案例

登录页面

```text
<%@ page import="java.lang.annotation.Target" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>登录</title>
    <script>
        //点击超连接或者图片，要换一张
        //给超连接和图片绑定单机事件
        //重新设置图片的src值
        window.onload = function () {
            let img = document.getElementById("checkCode");
            img.onclick = function () {
                //加时间戳
                let date = new Date().getTime();
                img.src = "day16/check?" + date;
            }
        }
    </script>
​
    <style>
        #check {
            color: red;
        }
​
        #login {
            text-align: center;
            margin-top: 200px;
        }
    </style>
</head>
<body>
<%
    HttpSession session1 = request.getSession();
    Object check = session1.getAttribute("check");
    String checkText = "";
    if (check != null) {
        if (check.equals("upFalse")) {
            checkText = "用户名或密码错误";
            session1.invalidate();
        } else if (check.equals("acFalse")) {
            checkText = "验证码错误";
            session1.invalidate();
        }
    }
%>
<div id="login">
    <form action="day16/is_success" method="post">
        用户名 <input type="text" name="username" placeholder="请输入用户名"><br>
        密码 <input type="password" name="password" placeholder="请输入密码"><br>
        验证码 <input type="text" name="check" placeholder="请输入验证码"><br>
        <img id="checkCode" src="day16/check" alt="验证码"/>
        <input type="submit" value="登录">
        <br>
    </form>
    <div id="check"><%= checkText %>
    </div>
</div>
</body>
</html>
```

主页

```text
<%@ page import="java.text.DateFormat" %>
<%@ page import="java.text.SimpleDateFormat" %>
<%@ page import="java.util.Date" %>
<%@ page import="java.net.URLEncoder" %>
<%@ page import="java.net.URLDecoder" %>
<%@ page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<!DOCTYPE html>
<html>
<head>
    <title>JSP - Hello World</title>
</head>
<body>
<h1><%= "Hello World!" %>
</h1>
<br/>
<a href="day14/demo01">Hello Servlet</a>
​
<%
    //设置cookie
    HttpSession session1 = request.getSession();
    Object check = session1.getAttribute("check");
    if (!("true".equals(check))) {
        String contextPath = request.getContextPath();
        response.sendRedirect(contextPath + "/login.jsp");
    }
    DateFormat dateFormat = new SimpleDateFormat("yyyy年MM月dd HH:mm:ss");
    String time = dateFormat.format(new Date());
    //编码
    time = URLEncoder.encode(time, "utf-8");
    Cookie c = new Cookie("lastTime", time);
//        c.setMaxAge(30000);
    Cookie[] cs = request.getCookies();
    boolean isCome = false;
    for (Cookie cookie : cs) {
        if ("lastTime".equals(cookie.getName())) {
            //有cookie
            response.addCookie(c);
            isCome = true;
            //解码
            String value = URLDecoder.decode(cookie.getValue(), "utf-8");
            response.getWriter().write("欢迎回来，上一次访问时间为：" + value);
        }
    }
    if (!isCome) {
        //没有cookie
        response.addCookie(c);
        response.getWriter().write("欢迎你首次访问");
    }
%>
</body>
</html>
```

验证码生成

```text
package com.day16.web.servlet;
​
import javax.imageio.ImageIO;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.IOException;
import java.util.Random;
​
@WebServlet("/day16/check")
public class Check extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //验证码
        //1.创建一个对象，在内存中画图（验证码图片对象）
        int width = 100;
        int height = 50;
        BufferedImage image = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);
        //2.美化图片
        //填充背景色
        Graphics g = image.getGraphics();
        g.setColor(Color.pink);
        g.fillRect(0, 0, width, height);
        //话边框
        g.setColor(Color.blue);
        g.drawRect(0, 0, width - 1, height - 1);
        //写验证码
        String str = "QWERTYUIOPASDFGHJKLZXCVBNMqwertyuiopasdfghjklzxcvbnm11234567890";
        StringBuilder authCode = new StringBuilder();
        Random random = new Random();
        int charLen = 4;
        for (int i = 1; i <= charLen; i++) {
            int index = random.nextInt(str.length());
            char ch = str.charAt(index);
            g.drawString(ch + "", width / (charLen + 1) * i, height / 2 + 10);
            authCode.append(ch);
        }
        HttpSession session = req.getSession();
        session.setAttribute("authCode", authCode.toString());
​
        //画干扰线
        g.setColor(Color.green);
        int wireLen = 10;
        for (int i = 0; i < wireLen; i++) {
            int x1 = random.nextInt(width);
            int x2 = random.nextInt(width);
            int y1 = random.nextInt(height);
            int y2 = random.nextInt(height);
            g.drawLine(x1, y1, x2, y2);
        }
        //3.将图片输出到页面
        ImageIO.write(image, "jpg", resp.getOutputStream());
    }
​
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doPost(req, resp);
    }
}
```

验证验证码和用户名和密码

```text
package com.day16.web.servlet;
​
import com.day16.web.dao.UserDao;
import com.day16.web.domain.User;
​
import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.annotation.*;
import java.io.IOException;
​
@WebServlet(name = "IsSuccess", value = "/day16/is_success")
public class IsSuccess extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setCharacterEncoding("utf-8");
        String check = request.getParameter("check");
        HttpSession session = request.getSession();
        String realCode = (String) session.getAttribute("authCode");
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        if (realCode.equalsIgnoreCase(check)) {
            User loginUser = new User();
            loginUser.setUsername(username);
            loginUser.setPassword(password);
            UserDao userDao = new UserDao();
            User user = userDao.login(loginUser);
            if (user == null) {
                //登录失败
                String contextPath = request.getContextPath();
                HttpSession session1 = request.getSession();
                session1.setAttribute("username", username);
                session1.setAttribute("password", password);
                session1.setAttribute("check", "upFalse");
                response.sendRedirect(contextPath + "/login.jsp");
            } else {
                //登录成功
                String contextPath = request.getContextPath();
                HttpSession session1 = request.getSession();
                session1.setAttribute("check", "true");
                response.sendRedirect(contextPath + "/index.jsp");
            }
​
        } else {
            String contextPath = request.getContextPath();
            HttpSession session2 = request.getSession();
            session2.setAttribute("username", username);
            session2.setAttribute("password", password);
            session2.setAttribute("check", "acFalse");
            response.sendRedirect(contextPath + "/login.jsp");
        }
    }
​
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

### JSP

#### 指令

用于配置JSP页面，导入资源文件

&lt;%@ 指令名称 name1=value1 name2=value2 %&gt;

* page 配置jsp页面啊
  * contentType:设置响应体的mime类型和字符集和当前页面的编码
  * import：导包
  * errorPage：当前页面发送异常后会自动跳转到错误页面
  * isErrorPage：标识当前页面是否是错误页面，
* include 导入页面的资源文件
* taglib 导入资源

  ```text
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  ```

#### 内置对象

* pageContext：当前页面共享数据，还可以获取其他8个内置对象
* request：一次请求访问的多个资源（转发）
* session：一次会话的多次请求
* application：所有用户间共享数据
* response：响应对象
* page：当前页面 this
* out：输出对象，数据输出到页面上
* config：Servlet的配置对象
* exception：异常对象

### MVC开发模式

* jsp的演变历史
  * 早期只有Servlet，只能使用response输出标签数据，非常麻烦
  * 后来有了jsp，简化了Servlet的开发
  * 但是如果大量使用Java代码和html标签，但是会难以维护，难以分工
  * Java的web开发，借鉴MVC的开发模式，是的程序的开发更加合理
* M：Model 模型
  * 业务逻辑操纵
* V：view 视图
  * 展示数据
* C：Controller 控制器
  * 获取客户端的输入
  * 调用模型
  * 将数据交给视图展示
* 优点
  * 耦合性低，方便维护，利于分工协作
  * 重用性高
* 缺点
  * 没有明确的定义，需要精心的设计
  * 对开发人员的要求比较多

### EL表达式

Expression Language 表达式语言

作用：替换和简化jsp页面中Java代码的编写

语法：${表达式} （\可以忽略表达式，原样输出）

* 运算
  * 算数运算符
  * 比较运算符
  * 逻辑运算符
  * 空运算符：用于判断字符串，集合，数组对象是否为null或者长度是否为0

```text
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>el1</title>
</head>
<body>
<h3>算数运算符</h3>
${3+4}
${3-4}
${3/4}
${3 div 4}
${3%4}
${3 mod 4}
<h3>逻辑运算符</h3>
${true && false}
<h3>空运算符</h3>
</body>
</html>
```

* 获取值
  * 只能从域对象中获取值
  * ${域名称.键名}:指定域中获取指定键的值
    * pageScope --&gt;pageContext
    * requestScope --&gt;request
    * sessionScope --&gt;session
    * applicationScope --&gt;application\(ServletContext\)
  * {键名}：从最小的域开始查找起
  * 获取对象，获取对象中的值
    * 通过对象的get方法，去掉get，首字母小写，就就是对于的属性值
    * 要操作某个对象，一般是在对应的JavaBean对象中创建对应的get方法，操作后返回值即可
    * ${requestScope.user.name}
    * ${requestScope.user.date.year} 

