---
description: '2021-02-12'
---

# dat19

## 完成情况

{% hint style="success" %}
每一天都学习并记录
{% endhint %}

{% hint style="warning" %}
每天看至少3个leetcode的题目
{% endhint %}

## 学习内容

### Filter

过滤器，可以拦截请求，强化request和response

#### 过滤器链

执行顺序，如果有两个过滤器：过滤器1，过滤器2

* 过滤器1执行
* 过滤器2执行
* 资源执行
* 过滤器2执行
* 过滤器1执行

先后顺序

* 配置
  * 注解
    * 安装类名字符串比较规则比较，值小的先执行
    * 如Afilter和Bfilter，Afilter先执行
  * web.xml
    * 配置在最上面的先执行

#### 登录验证

```java
package pro.doublez.web.Filter;

import javax.servlet.*;
import javax.servlet.annotation.*;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

/**
 * 完成登录验证的过滤器
 */
@WebFilter("/*")
public class LoginFilter implements Filter {

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws ServletException, IOException {
        //强制转换
        HttpServletRequest req = (HttpServletRequest) request;
        //获取请求资源的路径
        String uri = req.getRequestURI();
        //注意排除登录相关的资源
        if (uri.contains("/login.jsp") || uri.contains("/loginServlet") || uri.contains("checkCodeServlet")) {
            chain.doFilter(request, response);
        } else {
            Object user = req.getSession().getAttribute("user");
            if (user != null) {
                chain.doFilter(request, response);
            } else {
                //没有登录
                req.setAttribute("login_msg", "你还没有登录");
                req.getRequestDispatcher("/login.jsp").forward(request, response);
            }
        }

    }

    public void init(FilterConfig config) throws ServletException {
    }

    public void destroy() {
    }
}
```

#### 过滤敏感词汇

* 对request对象的getParament方法进行增强，产生一个新的request对象
* 放行，将新的request对象传入
* 增强对象的功能：代理模式
* 代理对象代理真实对象，达到增强真实对象的功能的目的
  * 静态代理：有一个类文件描述代理模式
  * 动态代理：在内存中形成代理类
* 实现步骤

  * 代理对象和真实对象实现相同的接口
  * 代理对象=Proxy.newInstance\(\)  获取代理对象
  * 使用代理对象调用方法
  * 增强方法
    * 增强参数列表
    * 增强返回值
    * 增强方法体

  ```java
  public class ProxyTest {
      public static void main(String[] args) {
          //创建真实对象
          Lenovo lenovo = new Lenovo();
          //动态代理增强对象
          //三个参数
          //1.类加载器 lenovo.getClass().getClassLoader()
          //2.接口数组 lenovo.getClass().getInterfaces()
          //3.处理器   ew InvocationHandler()
          SaleComputer proxy_lenovo = (SaleComputer) Proxy.newProxyInstance(lenovo.getClass().getClassLoader(), lenovo.getClass().getInterfaces(), new InvocationHandler() {
              @Override
              /*
              代理逻辑编写的方法，代理对象调用的所有方法都会执行该方法
              参数：
              1.proxy：代理对象
              2.method：代理对象调用的方法被封装为的对象
              3.args:代理对象调用方法时传递的实际参数
               */
              public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                  System.out.println(&quot;invoke方法被执行&quot;);
                  //使用真实对象调用该方法
                  if (method.getName().equals(&quot;sale&quot;)) {
                      double money = (double) args[0];
                      //增强参数
                      money = money * 1.15;
                      //增强方法体
                      Object obj = method.invoke(lenovo, money);
                      System.out.println(&quot;免费送货&quot;);
                      //增强返回值
                      return obj + &quot;_鼠标垫&quot;;
                  } else {
                      Object obj = method.invoke(lenovo, args);
                      return obj;
                  }
              }
          });
          String computer = proxy_lenovo.sale(8888);
          System.out.println(computer);
      }
  }
  ```

对request对象进行增强，增强获取参数的相关的方法

```java
package pro.doublez.web.Filter;

import javax.servlet.*;
import javax.servlet.annotation.*;
import java.io.*;
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
import java.util.ArrayList;
import java.util.List;

/*
过滤敏感词汇
 */
@WebFilter("/*")
public class SensitiveWordFilter implements Filter {

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws ServletException, IOException {

        ServletRequest proxy_req = (ServletRequest) Proxy.newProxyInstance(request.getClass().getClassLoader(), request.getClass().getInterfaces(), new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                if (method.getName().equals("getParameter")) {
                    String values = (String) method.invoke(request, args);
                    if (values != null) {
                        for (String s : list) {
                            if (values.contains(s)) {
                                values = values.replaceAll(s, "***");
                            }
                        }
                    }
                    return values;
                }
                return method.invoke(request, args);
            }
        });

        chain.doFilter(proxy_req, response);

    }

    private List<String> list = new ArrayList<>();//敏感词集合

    public void init(FilterConfig config) throws ServletException {
        try {
            //加载文件
            ServletContext servletContext = config.getServletContext();
            String realPath = servletContext.getRealPath("/WEB-INF/classes/sensitive_word.txt");
            BufferedReader br = new BufferedReader(new InputStreamReader(new FileInputStream(realPath), "utf-8"));
            String line = null;
            while ((line = br.readLine()) != null) {
                list.add(line);
            }
            br.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public void destroy() {
    }
}
```

### 监听器

事件的监听机制

* 事件：一件事情
* 事件源：事件发生的地方
* 监听器：一个对象
* 注册监听：将事件，事件源，监听器绑定在一起，当事件源上发生某个事件后，执行监听器代码

ServletContextListen：监听ServletContext对象的创建和销毁

步骤

* 定义一个类，实现ServletContextListener接口
* 复写方法
* 配置
  * web.xml
  * 注解

```java
package pro.doublez.listener;

import javax.servlet.ServletContext;
import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;

@WebListener
public class ContextLoaderListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        //服务器启动后自动调用
        //加载资源文件
        ServletContext servletContext = sce.getServletContext();
        System.out.println("ServletContext被创建");
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        //服务器正常关闭后，自动调用
        System.out.println("ServletContext被销毁");
    }
}
```

