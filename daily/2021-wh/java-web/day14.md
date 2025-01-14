---
description: '2021-02-07'
---

# day14

## 完成情况

每一天都学习并记录


每天看至少3个leetcode的题目


## 学习内容

### Sevlet

urlpartten相关配置:Servlet访问路径

1.可以定义多个路径{}

2.路径定义

* 单路径    /demo
* 多层路径     /demo/1
* 模糊路径     /\*
* 自定义后缀     .do

#### HTTP

Hyper Text Transfer Protocol 超文本传输协议

定义了客户端与服务器端通信时，数据通信的格式

* 基于TCP/IP协议
* 默认端口号，80
* 基于请求/响应模型，一次请求对应一次响应
* 无状态的，每次请求之间相互独立，不能交互数据
* 1.0 每次请求就会建立一次连接
* 1.1  复用连接

请求消息数据格式

* 请求行
  * 请求方式
    * GET：请求参数在请求行中，在url后，不够安全
    * POST：请求参数在请求体中，长度没有限制，相对安全
  * 请求url：login.html
  * 请求协议/版本：HTTP/1.1
* 请求头
  * 请求头名称：请求头值
  * Host：主机名
  * User-Agent：浏览器告诉服务器其版本信息
    * 可以在服务器端获取该头信息，解决浏览器的兼容问题
    * Mozilla/5.0 \(Windows NT 10.0; Win64; x64\) AppleWebKit/537.36 \(KHTML, like Gecko\) Chrome/88.0.4324.146 Safari/537.36
  * keep-alive：连接是否可以复用
  * Referer：告诉服务器，当前请求从哪里来

    Referer: [http://localhost:8080/login.html](http://localhost:8080/login.html)

    * 防盗链
    * 统计工作
* 请求空行
  * 空行，分割POST请求的请求头和请求体
* 请求体（正文）
  * 封装POST请求消息的参数

request和response对象是又服务器创建的，我们来使用他们

1.tomcat服务器会根据请求url中的资源路径，创建对应的Servlet对象

2.tomcat服务器创建request对象和response对象，request对象中封装请求消息数据

3.tomcat将request和reponse对象传递给service方法，并且调用service方法

4.通过request对象获取请求消息数据，通过response对象设置响应消息数据

5.服务器在给浏览器做出响应之前，会从response对象中拿程序员设置的响应消息数据

#### Request

继承体系

ServiceRequest 接口

HttpServiceRequest 接口 继承自 ServiceRequest 接口

\(tomcat\) org.apache.catalina.connector.RequestFacade 类 实现 HttpServiceRequest 接口

**获取请求消息数据**

* 获取请求行数据

  GET /day14/demo1?username=zhangsan HTTP/1.1

  * 获取请求方式：String getMethod\(\)  
  * 获取虚拟目录：String getContextPath\(\)
  * 获取Servlet路径：String getServletPath\(\)
  * 获取get方式的请求参数：String getQueryString\(\)
  * 获取请求的URI：String getRequestURI\(\)
  * 获取请求的URL：StringBuffer getRequestURL\(\)
  * 获取协议和版本：String getProtocol\(\)
  * 获取客户机的IP地址：String getRemoteAddr\(\)

  ```text
  请求方式：GET
  虚拟目录：/web1
  Servlet路径：/day14/demo01
  get方式的请求参数：username=zhangsan
  请求的URI:/web1/day14/demo01
  请求的URL：http://localhost:8080/web1/day14/demo01
  协议和版本：HTTP/1.1
  客户机的IP地址0:0:0:0:0:0:0:1
  ```

* 获取请求头数据

  * 通过请求头的名称获取请求头的值：String getHeader\(String name\)
  * 获取所有的请求头名称：Enumeration getHeaderNames\(\)  返回一个迭代器

  ```text
  host------localhost:8080
  connection------keep-alive
  upgrade-insecure-requests------1
  user-agent------Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.146 Safari/537.36
  accept------text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
  sec-fetch-site------none
  sec-fetch-mode------navigate
  sec-fetch-user------?1
  sec-fetch-dest------document
  accept-encoding------gzip, deflate, br
  accept-language------zh,zh-CN;q=0.9
  cookie------JSESSIONID=BAFA669D3DE453EF60C380AAD2354685; csrftoken=qbsM8aS3oMuNno7cZe94Rt3q8VK956WKhkoSW4Lduhjo1o1OtSpPABoqBjcfc1UH; Idea-47cfbfa0=3fd074ce-6130-40e9-ab27-41f87ff8fc78; JSESSIONID=7B172ECC26C47A832CC805CF0EF7A6CF
  ```

* 获取请求体数据
  * 只有post方式，才有请求体，在请求体中封装了post请求的参数
  * 获取流对象
  * 在流对象中拿数据
  * getReader\(\)

    username=doublez&passname=aa2217117

  * getInputStream\(\)

**其他功能**

获取请求方式的通用方式（get/post） String getParameter\(String name\) 根据参数获取值

String\[\] getparameterValues\(String name \) 根据参数获取参数值的数组

Enumerate getparameterNames\(\) 获取所有请求的参数名称

Map getparameterMap\(\) 获得所有参数的map集合

```java
@WebServlet(value = "/day14/demo02")
public class ServletDemo02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取请求参数
        String username = request.getParameter("username");
        System.out.println("post");
        System.out.println(username);
        //获取请求参数的数组,用于复选框
        String[] hobbies = request.getParameterValues("hobby");
        for (String hobby : hobbies) {
            System.out.println(hobby);

        }
        //获取所有请求的参数名称
        Enumeration<String> parameterNames = request.getParameterNames();
        while (parameterNames.hasMoreElements()) {
            String name = parameterNames.nextElement();
            System.out.println(name);
            String value = request.getParameter(name);
            System.out.println(value);
            System.out.println("------");
        }
//        获得所有参数的map集合
        Map<String, String[]> parameterMap = request.getParameterMap();
        Set<String> keySet = parameterMap.keySet();
        for (String name : keySet) {
            String[] values = parameterMap.get(name);
            System.out.println(name);
            for (String value : values) {
                System.out.println(value);
                System.out.println("------");
            }
        }
    }
}
```

* 中文乱码的问题

  request.getCharacterEncoding\(utf-8\);

请求转发

* 服务器内部资源跳转的方式
* 通过request对象获取转发器对象

  ```java
  req.getRequestDispatcher("/day14/demo04").forward(req,resp);
  ```

* 浏览器的地址栏不变
* 只能转发到当前的服务器内部资源中
* 转发是一次请求，多个资源使用同一次请求

共享数据

* 域对象：一个有作用范围的对象，可以在范围内共享数据
* request的域，代表一次请求的范围，一般用于请求转发的多个资源共享数据
* 方法

  * setAttribute\(String name,Object obj\)   存储数据
  * Object getAttribute\(String\)   获取数据
  * void removeAttribute\(String name\) 移除数据

  获取ServletContext

* getServletContext

