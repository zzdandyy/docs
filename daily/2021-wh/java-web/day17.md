---
description: '2021-02-10'
---

# day17

## 完成情况

每一天都学习并记录


每天看至少3个leetcode的题目


## 学习内容

### JSTL

JavaServlet Pages Tag Library JSP标准标签库

* 由Apache组织提供的开源的免费的jsp标签
* 作用：用于简化和替换jsp页面的java代码

```text
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
```

常用的JSTL标签

* if：相当于if语句
* choose：相当于switch语句
* foreach：相当于for语句

```text
<%@ page import="java.util.ArrayList" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

<html>
<head>
    <title>JSTL</title>
</head>
<body>

<%
    ArrayList<String> list = new ArrayList<>();
    list.add("aaa");
    list.add("bbb");
    list.add("ccc");
    request.setAttribute("list", list);
    request.setAttribute("number", 3);
%>


<%--
    if标签
    test 必须属性，接收boolean表达式
    如果表达式为true，显示标签体的内容
--%>
<c:if test="${not empty list}">
    遍历集合
</c:if>
<br>
<%--
    数字编号代表星期几的案例
    域中存储一个数字
    使用choose去除数字，相当于switch
    使用when做数字的判断，相当于case
    otherwise做其他情况的声明，相当于default
--%>
<c:choose>
    <c:when test="${number==1}">星期一</c:when>
    <c:when test="${number==2}">星期二</c:when>
    <c:when test="${number==3}">星期三</c:when>
    <c:when test="${number==4}">星期四</c:when>
    <c:when test="${number==5}">星期五</c:when>
    <c:when test="${number==6}">星期六</c:when>
    <c:when test="${number==7}">星期日</c:when>
    <c:otherwise>数字输入有误</c:otherwise>
</c:choose>
<br>
<%--
    完成重复操作
    begin：开始值（包含）
    end：结束值（包含）
    var：临时变量
    step：步长
--%>

    <c:forEach begin="5" end="15" step="2" var="i" varStatus="s">
        ${i} ${s.index} ${s.count}<br>
<%--
index:容器中元素的索引
count:循环的次数
--%>
    </c:forEach>
    <br>

    <c:forEach var="i" items="${list}" varStatus="s">
        ${i}   ${s.index}    ${s.count} <br>
    </c:forEach>
</body>
</html>
```

### 三层架构

1.界面层（表示层）：用户看得到的界面，用户可以通过界面上的组件和服务器进行交互

2.业务逻辑层：处理业务逻辑的

3.数据访问层：操作数据，存储文件

#### 案例

* 需求：用户信息的增删改查操作
* 设计
  * 技术选型：Servlet+JSP+MySQL+JDBCTemplete+Druid+BeanUtils+tomcat
  * 数据库设计

    ```sql
    create database day17; -- 创建数据库
    use day17;  -- 使用数据库
    create table user(  -- 使用数据库
        id int primary key auto_increment,  -- id 主键
        name varchar(20) not null , -- 姓名
        gender varchar(5),  -- 性别
        age int, -- 年龄
        address varchar(32),    -- 住址
        qq varchar(20), -- qq号码
        email varchar(50)   -- 邮箱
    );
    ```
* 开发
  * 环境搭建
    * 创建数据库环境
    * 创建项目，导入需要的jar包
  * 编码
* 测试
* 部署运维

思路

UserListServlet

* 调用service层的findAll\(\),返回List集合，List
* 将list集合存入request域中
* 转发list.jsp页面展示
  * jstl+el
  * foreach标签遍历

UserService接口

* public list findAll\(\);

UserServiceImpl类

* public list findAll\(\);{

  }

UserDao接口

* public List findAll\(\)

UserDaoImpl类

* public List findAll\(\){

  }

