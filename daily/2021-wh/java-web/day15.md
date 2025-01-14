---
description: '2021-02-08'
---

# day15

## 完成情况

每一天都学习并记录


每天看至少3个leetcode的题目


## 学习内容

### response

数据格式

* 响应行
  * 协议/版本   响应状态码   状态码描述
  * 响应状态码：
    * 服务器告诉客户端浏览器本次请求和响应的一个状态
    * 状态码都是3为数字
    * 100+：客户端接收客户端消息，但是没有接收完成，等待一段时间之后，发送100+的状态码
    * 200+：成功   代表：200
    * 300+：代表：302（重定向）   304（访问缓存）
    * 400+：客户端错误   比如说路径写错（404）   访问路径没有对应的doGet方法（405）
    * 500+：服务器错误   服务器内部出现异常（500）
* 响应头
  * 头名称：值
  * Content-Type：服务器告诉浏览器，本次响应体的数据格式以及编码方式
  * Content-disposition:服务器告诉客户端以什么格式打开响应体数据
    * in-line:默认值，在当前页面打开
    * attachment；filename=_\*_：以附件形式打开响应体数据（下载）
* 响应空行
* 响应体：响应的内容

#### response对象

设置响应行

* 格式：HTTP/1.1   200   OK
* 设置状态码：setStatus（int sc）

设置响应头

* setHeader\(String name,String value\)

设置响应体

* 获取输出流
* 使用输出流，将数据输出到客户端浏览器中
* 字符输出流：getWriter\(\)
* 字节输出流：getOutputStream\(\)

**完成重定向**

资源跳转的方式

A资源：告诉浏览器重定向，设置状态码：302

​ 告诉浏览器B资源的路径，响应头location：B资源的路径

```java
@Override
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    //重定向，访问这个资源，会自动跳转到demo03
    //设置状态码为302
    //设置响应头
    System.out.println("demo02被访问了");
    resp.setStatus(302);
    resp.setHeader("location", "demo03");
    //一种简单的重定向的方式
    resp.sendRedirect("demo03");
}
```

转发的特点 forward

* 转发的地址栏不变
* 转发只能访问当前服务器的资源
* 转发是一次请求，可以使用request共享数据

重定向的特点 redirect

* 重定向的地址栏变化
* 重定向可以访问其他服务器的资源
* 重定向是两次选择，不能共享数据

路径的分类

* 相对路径：不可以确定唯一资源
  * 直接写路径
  * demo01
  * 规则：确定访问的当前资源与目标资源的相对位置
  * ./    当前目录
  * ../   上一级页面
* 绝对路径：通过路径可以确定唯一资源（推荐使用绝对路径）
  * / 开头的路径
  * 如 /web1/day15/demo01
  * 规则：判断路径是给服务器使用还是给客户端使用
  * 客户端使用：需要写虚拟目录（重定向需要使用，动态获取虚拟目录）

    ```java
    //动态获取虚拟目录
    String contextPath = req.getContextPath();
    resp.sendRedirect(contextPath+"/day15/demo03");
    ```

  * 服务器使用：不需要写虚拟目录（如转发路径，服务器内部的使用）

**服务器输出字符数据到浏览器**

中文的操作系统，浏览器打开默认的字符集是GBK\(GB2132\)

tomcat服务器默认的字符集是ISO-8859-1

所以要设置编码的方式

```java
@Override
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    //设置编码
    resp.setCharacterEncoding("utf-8");
    //告诉浏览器，服务器发送的消息体数据的编码，建议浏览器安装该方式解码
    resp.setHeader("content-type", "text/html;charset=utf-8");
    //简单的形式设置编码
    resp.setContentType("text/html;charset=utf-8");
    //获取字符输出流
    PrintWriter pw = resp.getWriter();
    //输出数据
    pw.write("<h1>hello response</h1>");
    pw.write("你好");
}
```

**服务器输出字节数据到浏览器**

```java
@Override
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    resp.setContentType("text/html;charset=utf-8");
    //字节输出流
    ServletOutputStream os = resp.getOutputStream();
    os.write("hello".getBytes(StandardCharsets.UTF_8));
}
```

**验证码**

```java
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
    Random random = new Random();
    int charLen = 4;
    for (int i = 1; i <= charLen; i++) {
        int index = random.nextInt(str.length());
        char ch = str.charAt(index);
        g.drawString(ch + "", width / (charLen + 1) * i, height / 2 + 10);
    }
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
```

```javascript
<script>
    //点击超连接或者图片，要换一张
    //给超连接和图片绑定单机事件
    //重新设置图片的src值
    window.onload = function () {
        let img = document.getElementById("checkCode");
        img.onclick = function () {
            //加时间戳
            let date = new Date().getTime();
            img.src = "/web1/day15/demo05?" + date;
        }
        let changeText = document.getElementById("change");
        changeText.onclick = function () {
            let date = new Date().getTime();
            img.src = "/web1/day15/demo05?" + date;
        }
    }
</script>
```

#### ServletContext对象

概念：代表整个web应用，可以和程序的容器（服务器）通信

获取

* 通过request对象获取
* 通过HttpServlet获取

```java
//ServletContext对象的获取
ServletContext context1 = req.getServletContext();
ServletContext context2 = this.getServletContext();
System.out.println(context1==context2);
System.out.println(context1);
```

功能：

* 获取MIME类型：文件上传下载
  * 格式：大类型/小类型   text/html      image/jpeg
  * 一个文件的后缀会对应一个MiME类型
* 域对象：共享数据
  * setAttribute\(String name,Object value\)
  * getAttribute\(Stirng name\)
  * removeAttribute\(String name\)
  * 生命周期很长，所有用户都可以访问，但是要谨慎使用
* 获取文件的真实（服务器）路径

  * web项目会在本地存在一份，tomcat存在一份（真实路径）
  * getRealPath\(\)

  ```java
  ServletContext context = this.getServletContext();
  String realPath = context.getRealPath("/b.txt");
  //   /指的是webapp目录，下级目录有/WEB-INF，再下级有classes，就是对应的src目录
  System.out.println(realPath);
  File file = new File(realPath);
  ```

**文件下载**

* 默认情况下，文件不能被浏览器解析，即弹出下载框，然后能解析，就展示

我们要设置资源的打开方式

* 定义页面

  ，编辑超连接的href属性，指向Servlet，传递资源名称

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>下载</title>
  </head>
  <body>
  <a href="/web1/day15/download?filename=1.jpg">图片1</a>
  <a href="/web1/day15/download?filename=2.jpg">图片2</a>
  </body>
  </html>
  ```

* 定义Servlet

  * 获取文件名称
  * 使用字节输入流加载文件进内存（需要找到文件的真实路径）
  * 指定response响应头
  * 将数据写到response输出流
  * 中文乱码问题
    * 获取客户端使用的浏览器版本信息
    * 根据不用的版本信息，设置filename的编码方式

  ```java
  @WebServlet("/day15/download")
  public class DownloadDemo extends HttpServlet {
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          String filename = req.getParameter("filename");
          String realPath = this.getServletContext().getRealPath("/img/" + filename);
          FileInputStream fis = new FileInputStream(realPath);
          //设置response的响应头
          String agent = req.getHeader("user-agent");
          //解决中文乱码的问题
          filename = DownLoadUtils.getFileName(agent, filename);

          resp.setHeader("content-type", this.getServletContext().getMimeType(filename));
          resp.setHeader("content-disposition", "attachment;filename=" + filename);

          //将输入流的数据写到输出流中
          ServletOutputStream sos = resp.getOutputStream();
          byte[] buff = new byte[1024 * 8];
          int len = 0;
          while ((len = fis.read(buff)) != -1) {
              sos.write(buff, 0, len);
          }
          fis.close();
      }
  ```

### 会话

一次会话中包含多次的请求和响应

* 一次会话：浏览器第一次给服务器发送请求，会话建立，直到有一方断开连接
* 在一次会话的返回内共享数据

在http协议中，每次请求都是独立的，而要进行会话间数据共享，就要使用会话技术

#### Cookie

概念：客户端会话技术，数据会存到客户端

1.第一次请求的时候，服务器会携带数据响应给客户端，客户端会保存数据

2.第二次请求时，会携带第一次的数据

* 创建Cookie对象，绑定数据   
* 通过响应，发送cookie对象
* 获取cookie，拿到数据

```java
@Override
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    Cookie[] cs = request.getCookies();
    if (cs != null) {
        for (Cookie c : cs) {
            System.out.println(c.getName() + "," + c.getValue());
        }
    }
}
```

过程

1.客户端发送请求给服务器，这个时候服务器会发送给客户端一个cookie

2.实际上是一个响应头set-cookie:msg=hello

3.客户端接收到这个响应头的时候，客户端会存储cookie的数据

4.下一次发送请求的时候，客户端的请求中会携带请求头

5.请求头：cookie:msg=hello

问题

* 一次可不可以发送多个cookie

  add多个Cookie即可

  ```java
  Cookie cookie1=new Cookie("msg1","hello");
  Cookie cookie2=new Cookie("msg2","java");
  response.addCookie(cookie1);
  response.addCookie(cookie2);
  ```

* cookie在浏览器中保存多长时间

  * 默认情况下，关闭浏览器，cookie被删除
  * 可以设置cookie的生命周期，持久化存储，setMaxAge\(int seconds\)
    * 正数：将Cookie写到硬盘文件中，持久化存储，代表cookie的存活时间（秒）
    * 负数：默认值，只写在内存中，关闭浏览器即删除
    * 0：删除cookie

  ```java
  @Override
  protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
      Cookie cookie1 = new Cookie("msg", "setMaxAge");
      //设置cookie的存活时间
      //30表示cookie持久化到硬盘，30秒会自动删除
      cookie1.setMaxAge(30);
      response.addCookie(cookie1);

      Cookie cookie2 = new Cookie("msg2", "setMaxAge2");
      //设置cookie的存活时间
      //-1即表示默认方法，关闭浏览器就会删除
      cookie2.setMaxAge(-1);
      response.addCookie(cookie2);

      Cookie cookie3 = new Cookie("msg3", "setMaxAge3");
      //设置cookie的存活时间
      //0表示删除cookie
      cookie3.setMaxAge(0);
      response.addCookie(cookie3);
  }
  ```

* cookie能不能存中文
  * 在tomcat 8 之前，cookie中不能直接存储中文数据，但是可以通过转码实现
  * 在tomcat 8 之后，cookie中可以存储中文数据，对于特殊字符还是不支持
* cookie获取的范围
  * 假设在一个tomcat服务器中，部署了多个web项目，cookie能否共享
    * 默认情况下，cookie不能共享
    * setPath 设置cookie获取的范围，默认情况下是设置当前的虚拟目录，即当前的项目
    * 可以设置为   "/"    可以整个服务器共享
  * 不同服务器的共享问题
    * setDomain\(String path\):如果设置一级域名相同，那么多个服务器的cookie可以共享
    * 例如，设置setDomain\(".baidu.com"\),那么tieba.baiodu.com和news.baidu.com可以共享

cookie的特点

* 存储数据在客户端浏览器，不够安全
* 浏览器对单个cookie的大小由限制（4kb），对同一域名的cookie的数量也有限制（20）
* 作用
  * cookie一般用于存储少量的不太重要的信息
  * 在不登陆的情况下，完成服务器对客户端的身份识别（设置，保存在浏览器）

cookie案例：记住上一次的访问时间

* 如果是第一次访问，提示：你好，欢迎您首次访问
* 如果不是第一次访问，提示：欢迎回来，上一次访问时间为：

```java
@WebServlet(name = "CookieDemo05", value = "/day15/CookieDemo05")
public class CookieDemo05 extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
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
    }
```

