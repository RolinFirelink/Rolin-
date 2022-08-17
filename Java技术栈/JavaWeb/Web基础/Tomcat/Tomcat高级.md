# 学生管理系统(下)

学习到这里，我们就到了第三阶段了，此时我们已经学习完了Cookie、Session以及JSP，接着我们就要利用这些技术来完成第三部分的案例

## 案例效果

同样的我们现在看看我们的案例效果

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe957ca1ed45cd4c7a957b43f884bf2c8.png)

首先我们的浏览器要能够判断是否有用户登录，如果有的话就显示添加或者查看学生的界面，如果没有就显示请登录的界面。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE99024b93eecaa5ce80be1b9d6a7ee3d6.png)

之后添加和查看学生的效果和我们第二部分做的差不多，不过这次我们查看学生就要显示出一个HTML的界面了，而不是恩读嗯写入了

## 登录功能实现

那么了解了案例内容之后，我们先来实现我们的登录功能，这也是本篇最有含金量的部分。先来看实现步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEaf5fd44d4c48aaa042b90ba412bda815.png)

那么根据步骤我可以先写入我们的首页的JSP代码如下

```
<%--
    1.page指令
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>学生管理系统首页</title>
  </head>
  <body>
        <%--
            获取会话域中的数据
            如果获取到了则显示添加和查看功能的超链接
            反之则显示登录功能的超链接
        --%>
        <% Object username = session.getAttribute("username");
            if(username==null){
        %>
            <a href="/stu/login.jsp">请登录</a>
        <%} else {%>
            <a href="/stu/addStudent.jsp">添加学生</a>
            <a href="/stu/listStudentServlet">查看学生</a>
        <%}%>
  </body>
</html>

```

继续根据步骤我们可以实现登录页面的JSP代码如下

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>学生登录</title>
</head>
<body>
    <form action="/stu/loginStudentServlet" method="get" autocomplete="off">
        用户名：<input type="text" name="username"> <br>
        密码：<input type="password" name="password"> <br>
        <button type="submit">登录</button>
    </form>
</body>
</html>
```

然后我们可以实现学生登录之后服务器中内部执行的功能类的代码如下

```
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/*
    学生登录
 */
@WebServlet("/loginStudentServlet")
public class LoginStudentServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.获取用户名和密码
        String username = req.getParameter("username");
        String password = req.getParameter("password");

        //2.判断用户名
        if(username == null || "".equals(username)){
            //2.1用户名为空 重定向到登录页面
            resp.sendRedirect("/stu/login.jsp");
            return;
        }

        //2.2用户名不为空 将用户名存储会话域中
        req.getSession().setAttribute("username",username);

        //3.重定向到首页index.jsp
        resp.sendRedirect("/stu/index.jsp");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

这里我们要搞懂其内部机制，首先我们的index.jsp查看我们的对话域中有无用户名，无则跳转到登录页面，然后我们点击登录，重定向到实现登录功能的JSP，然后里面完成登录功能之后内部服务器进行了处理并将内容上传到了对话域中，然后重新跳转到index.jsp，此时对话域中的用户名不为空，此时跳转到我们的添加和查找页面

## 添加功能实现

接着我们来实现添加功能，来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE093b60044a1fb643484235b770184bfc.png)

这里的代码其实和我们第二个案例的一模一样，我就不贴了

最后我们来实现查看功能

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE06a381bf6d6933de718f680ca221ff2a.png)

这里我们只放两个代码，因为其他的只要直接拿第二个部分的代码就可以了，首先是实现查看功能的代码

```
package com.itheima.servlet;

import com.itheima.bean.Student;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.*;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;

/*
    实现查看功能
 */
@WebServlet("/listStudentServlet")
public class ListStudentServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.创建字符输入流对象，关联读取的文件
        BufferedReader br = new BufferedReader(new FileReader("d:\\stu.txt"));

        //2.创建集合对象，用于保存Student对象
        ArrayList<Student> list = new ArrayList<>();

        //3.循环读取文件中的数据，将数据封装到Student对象中。再把多个学生对象添加到集合中
        String line;
        while ((line = br.readLine()) != null){
            //张三,23,95
            Student stu = new Student();
            String[] arr = line.split(",");
            stu.setUsername(arr[0]);
            stu.setAge(Integer.parseInt(arr[1]));
            stu.setScore(Integer.parseInt(arr[2]));
            list.add(stu);
        }

        //4.将集合对象存入到会话域中
        req.getSession().setAttribute("students",list);

        //5.重定向到学生列表页面
        resp.sendRedirect("/stu/listStudent.jsp");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

这里我们做的改动是将数据存到会话域中，然后将页面重定向到学生列表页面

```
<%@ page import="java.util.ArrayList" %>
<%@ page import="com.itheima.bean.Student" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>查看学生</title>
</head>
<body>
    <table width="600px" border="1px">
        <tr>
            <th>学生姓名</th>
            <th>学生年龄</th>
            <th>学生成绩</th>
        </tr>
        <% ArrayList<Student> students = (ArrayList<Student>) session.getAttribute("students");
            for (Student stu :students) {
        %>
        <tr>
            <td><%=stu.getUsername()%></td>
            <td><%=stu.getAge()%></td>
            <td><%=stu.getScore()%></td>
        </tr>
        <%}%>
    </table>
</body>
</html>

```

然后我们构造学生列表的jsp页面如下，这里我们的原理是获得会话域中的集合对象，然后进行遍历。这里值得注意的是，我们的遍历时，如果我们需要写入我们的代码，而又需要我们的页面显示一些相应的信息，那么在代码块的两个大括号间应该要及时且适合地加上<%%>来框住我们的代码块的大括号，在然后在这个大括号内书写我们所需要的界面代码，如果要用上某些数据，那就加上<%%>继续获取就可以了。之所以这样做是因为在<%%>中无法写上我们的界面代码，所以我们采取这种方式，虽然有点麻烦，但是它的确有效

# EL表达式

本章节我们要加快速度了，因此除了必要的代码之外，其他简单的代码示例就直接截图给出了

## EL表达式的介绍

EL表达式是Servlet规范的一部分，其作用是可以再JSP页面中获取数据，让我们的JSP脱离java代码块和JSP表达式，反正就是可以让我们的编程变得更加简单

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE76dc0fb95a253b8489517bcc31a3dc6b.png)

## EL表达式的快速入门

同样我们也用一个案例来进行快速的入门，先来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9fae2351c63e164480531ca3d18955d5.png)

我们的目标是使用三种方式来获取我们域对象的数据，当然在获取之前我们应该要先往里面设置数据，那么最后我们可以构造其代码如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa4d1df81ae9386176645f5cf6212d261.png)

我们显然可以看到，EL表达式获取数据对比前两种方式，其更加简单快捷。当然有的同学可能会有疑问，就是为什么EL表达式里可以直接写入username就可以获取，而不用写上请求域的代码，这个留到我们后面去解释

## EL表达式获取不同类型的数据

本节我们来学习如何使用EL表达式获取各种不同类型的数据，请看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE512143b89dbb02b74d997764a56075e7.png)

那么我们可以构造其代码如下

```
<%@ page import="com.itheima.bean.Student" %>
<%@ page import="java.util.ArrayList" %>
<%@ page import="java.util.HashMap" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>EL表达式获取不同类型的数据</title>
</head>
<body>
    <%--1.获取基本数据类型--%>
    <% pageContext.setAttribute("num",10);%>
    基本数据类型:${num} <br>

    <%--2.获取自定义对象类型--%>
    <%
        Student stu = new Student("张三",23,23);
        pageContext.setAttribute("stu",stu);
    %>
    自定义对象: ${stu} <br>
    <%--stu.name 实现原理 getName()--%>
    学生姓名:${stu.username}
    学生年龄:${stu.age}

    <%--3.获取数组类型--%>
    <%
        String[] arr = {"hello","world"};
        pageContext.setAttribute("arr",arr);
    %>
    数组: ${arr} <br>
    0索引元素: ${arr[0]} <br>
    1索引元素: ${arr[1]} <br>

    <%--4.获取List集合--%>
    <%
        ArrayList<String> list = new ArrayList<>();
        list.add("aaa");
        list.add("bbb");
        pageContext.setAttribute("list",list);
    %>
    List集合: ${list} <br>
    0索引元素: ${list[0]} <br>

    <%--5.获取Map集合--%>
    <%
        HashMap<String,Student> map = new HashMap<>();
        map.put("hm01",new Student("张三",23,23));
        map.put("hm02",new Student("李四",24,24));
        pageContext.setAttribute("map",map);
    %>
    Map集合: ${map} <br>
    第一个学生对象: ${map.hm01} <br>
    第一个学生对象的姓名: ${map.hm01.username}

    <%--
        基本数据类型:10
        自定义对象: com.itheima.bean.Student@7b2cd838
        学生姓名:张三 学生年龄:23 数组: [Ljava.lang.String;@20491402
        0索引元素: hello
        1索引元素: world
        List集合: [aaa, bbb]
        0索引元素: aaa
        Map集合: {hm01=com.itheima.bean.Student@5f34b641, hm02=com.itheima.bean.Student@1cf88f1b}
        第一个学生对象: com.itheima.bean.Student@5f34b641
        第一个学生对象的姓名: 张三
    --%>
</body>
</html>

```

对应的解释都写到代码上了，这里就不再赘述了。不过值得一提的是，EL表达式里很多的调用方式都是.方法名（且不带括号）的形式，虽然其是这么写的，但其实其内部的本质过程是调用了对应的get方法来获得的，当然，这里发生在Tomcat里面的事情了，我们作为程序员不用管

## EL表达式注意事项

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0e445d449a232d3ae1e0512950b0a813.png)

简单来说就是本来应该报出异常的，在EL表达式里不会报这些异常，只会什么都不显示。而关于字符串的拼接，简单来说就是在EL表达式里两个字符串如果使用+号进行拼接，那么这个拼接不会成功，而且+号会被一起打印

## EL表达式的运算符

首先是关系运算符，其记法和作用以前已经讲过了，所以直接看图吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE95937f99708c4cc2b2874159c5d99da5.png)

然后是逻辑运算符

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8555132ea28902b152c9ab24f81fe2c1.png)

最后也是最重要的其他运算符

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEeff7a7f49c8db648f6910ea54709bcb7.png)

我们本节重点所需要演示的就是其他运算符，那么我们构造其演示代码如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8b06011917fc626b83875826c82761d3.png)

最后我们的到的结果是这样的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd71feea66179e0281f3255cce9450af6.png)

前三个true是因为我们的empty进行的判断，可以看到我们的empty的确有判断对象是否为null以及是否为空字符串和容器内元素是否为0的功能。最后一个功能是我们希望实现判断用户提供的数据里其是男是女，若是男我们就自动给其性别先勾选为男，因此我们这里利用EL表达式进行了一个三元运算符的判断，判断其是否为men，若是则输入checked，代表自动勾选，不是则不写入，代表不自动勾选

## EL表达式的使用细节

这里的使用细节其实和我们之前学习的JSP的细节差不多，其都可以调用其他八大隐式对象和四大域对象。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEba419ea4c8fc930b70e630915081da25.png)

但有些不同的是，EL表达式是直接根据名称从小到大在域对象中查找的，一旦查找到其就拿此值作为它的值，后续的值就不会再检查了。那么我们可以构造其测试代码如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE161370efbc9ceb1f28f4b97d094fc31e.png)

如果忘了什么是隐式对象，记得及时回去复习

## EL表达式隐式对象

本节我们来学习如何用EL表达式来表达隐式对象，EL表达式中一共有11个隐式对象，我们来一起看看其功能和不同

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE65cafa2ec1cd6b973839371fb5704534.png)

那么我们构造其演示代码如下

```
<%@ page import="com.itheima.bean.Student" %>
<%@ page import="java.util.ArrayList" %>
<%@ page import="java.util.HashMap" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>EL表达式11个隐式对象</title>
</head>
<body>
    <%--pageContext对象 可以获取其他三个域对象和JSP中八个隐式对象--%>
    ${pageContext.request.contextPath} <br>

    <%--applicationScope sessionScope requestScope pageScope 操作四大域对象中的数据--%>
    <% request.setAttribute("username","zhangsan");%>
    ${username} <br><%--在四大域中从小到大寻找--%>
    ${requestScope.username} <br><%--指定只在请求域中寻找--%>


    <%--header headerValues  获取请求头数据--%>
    ${header["connection"]} <br>
    ${headerValues["connection"][0]} <br>

    <%--param paramValues 获取请求参数数据--%>
    ${param.username} <br>
    ${paramValues.hobby[0]} <br>
    ${paramValues.hobby[1]} <br>

    <%--initParam 获取全局配置参数--%>
    <%--
        预先在web.xml写入
        <!--配置全局参数-->
        <context-param>
            <param-name>pname</param-name>
            <param-value>bbb</param-value>
        </context-param>
    --%>
    ${initParam["pname"]} <br>

    <%--cookie 获取cookie信息--%>
    <%--cookie只有两个元素，其中Cookie在前，session在后，其数据形式是map集合，以键值对的形式，key是名称，value是对象--%>
    ${cookie} <br> <%--获取Map集合--%>
    ${cookie.JSESSIONID} <br> <%--获取map集合中的第二个元素,即session对象，这里是通过指定key名称的方式获得的--%>
    ${cookie.JSESSIONID.name} <br> <%--获取cookie对象的名称--%>
    ${cookie.JSESSIONID.value} <%--获取cookie对象的值--%>
</body>
</html>
```

需要解释的部分都直接写到代码上了，这里就不赘述了

# JSTL

## JSTL的介绍

JSTL是JSP标准标签库的简称，其主要作用就是提供给开发人员一个标准同样的标签库。开发人员可以利用这些标签取代JSP页面上的Java代码，从而提高程序的可读性，降低程序的维护难度。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE220348a44cae991fc40d323732b1ec0c.png)

我们的标签库有很多，我们这里主要学习core，核心标签库

## JSTL的核心标签使用

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEdb9b67cb87f31b25775df4e7be2c8a66.png)

我们要学习的核心标签并不多，就图上这三个。先来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE46cdc76c25db152d79c092c7c70149a7.png)

我们先演示前两个，我们可以构造演示代码如下

```
<%@ page import="com.itheima.bean.Student" %>
<%@ page import="java.util.ArrayList" %>
<%@ page import="java.util.HashMap" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<html>
<head>
    <title>流程控制</title>
</head>
<body>
    <%--向域对象中添加成绩数据--%>
    ${pageContext.setAttribute("score","A")}

    <%--对成绩进行判断--%>
    <c:if test="${score eq 'A'}">
        优秀
    </c:if>

    <%--对成绩进行多条件判断--%>
    <c:choose>
        <c:when test="${score eq 'A'}">优秀</c:when>
        <c:when test="${score eq 'B'}">良好</c:when>
        <c:when test="${score eq 'C'}">及格</c:when>
        <c:when test="${score eq 'D'}">较差</c:when>
        <c:otherwise>成绩非法</c:otherwise>
    </c:choose>
</body>
</html>

```

我们这里要注意，我们使用JSTL之前要先引入对应的jar包，然后我们要在我们的JSP开头中先引入对应的标签库，上面的例子里引入标签库的代码是在第五行，uri代表了引入标签库的地址，而prefix则是我们给标签库取的名字，这个名字在我们后续使用标签中会用到

然后下面就是一些经典的使用代码的感觉了，我们的第一个语句就相当于if，第二个就相当于是switch，可以看到使用标签库不但可以使用这些语法，而且用起来比JSP的方式要来得简单和好看得多，JSP的光大括号都够折磨人的了

接着哦我们来演示最后的一段，其演示代码如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE09b7e78c95d29de28bf20814bc962479.png)

这里我们的items里面要填写的是我们要遍历的集合对象，而var则是我们每次将遍历的对象所保存的地方，这个其实不难理解，我们每次都拿到str并打印，这样我们就可以实现迭代我们集合并打印元素的功能

# Filter

## 过滤器的介绍

其实Filter就是过滤器，那么什么是过滤器呢？过滤器又有什么作用呢？请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEdd5b4cb59c13fdd310f4b44af4547bce.png)

简单来说，就是客户端请求响应之前要先经过Filter过滤器了，更加具体的作用请看上图的解释

## Filter介绍

简单来说Filter是一个接口，其下有三种方法，分别是用于初始化、销毁和实现过滤的方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6a8030488d6f29eb0b673ebe8b331246.png)

Filter的使用还需要配置，其有两种配置方式，一种是配置文件，另一种是注解

更加详细的解释请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2e9b33eab0a1015de167d7ce4949f386.png)

再来看看其下方法的说明

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb7280a7ba20487cd5c8117ef9d4e7b80.png)

## FilterChain的介绍

FilterChain是一个接口，其代表过滤器链对象。Servlet容器会自动提供实现类对象，因此我们直接使用就可以了。

过滤器可以定义多个，其会组成过滤器链，过滤器链FfilterChain本身作为参数要传入到Filter对象里的doFilter中，然而其自身也有一个doFilter方法，这个方法的作用是放行，简单来说就是我们的过滤器拦截了请求之后，还要将请求放出去，传给下一个过滤器或者是Servlet（如果有下一个过滤器的话），这个动作就叫做放行。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8a2cf4af64aa39cc4cac3b131c3d4a81.png)

关于FilterChain更加具体的解释请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE46f4c2006d264dd29679f9eae09a3b0e.png)

## Filter过滤器的使用

本节我们通过实现一个需求来学会如何使用Filter过滤器，请先来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3339bbde470d87b9379479763ca2dce0.png)

我们先编写一个测试类servletDemo01并写入如下代码

```
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/servletDemo01")
public class ServletDemo01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("servletDemo01执行了...");
        //resp.setContentType("text/html;charset=UTF-8");
        resp.getWriter().write("servletDemo01执行了");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

同样也构造了一个02的类，我们现在要解决浏览器上的中文乱码问题，我们可以采取直接指定浏览器的字符编码格式的方式，但如果类很多的话，我们肯定不能对每一个类都写入一个同样的代码，这时就们就要依赖我们的过滤器了，因此我们构建一个过滤器类并写入代码如下

```
package com.itheima.filter;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import java.io.IOException;

/*
    过滤器基本使用
 */
//@WebFilter("/servletDemo01")

@WebFilter("/*")
public class FilterDemo01 implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("filterDemo01执行了...");

        //处理乱码
        servletResponse.setContentType("text/html;charset=UTF-8");

        //放行
        filterChain.doFilter(servletRequest,servletResponse);
    }

    @Override
    public void destroy() {

    }
}

```

我们这里重写了其三个抽象方法，但只有一个doFilter方法是我们目前要考虑的，我们在这个方法里进行了乱码的处理，然后将该请求放行。我们这里采用注解的方式来配置这个过滤器，如果我们希望我们的过滤器只过滤某个功能类的请求，那么在注解中就填入该功能类，填入/*代表所有请求都会被拦截并处理

最终我们会发现这个案例是可以通过的，那么就没有问题了

## Filter过滤器的使用细节

首先我们的Filter过滤器有两种配置方式，注解方式我们前面已经演示过了，下图里演示了用配置文件进行配置的方式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE216006ca1e98644ea60754fe602c797c.png)

如果我们有多个过滤器的话，那么过滤器的先后取决于过滤器映射的先后的顺序（所谓映射，其实就是filter-mapping标签），请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE508ae9940b7b8fc2f526980b4645cff9.png)

我们这里01先映射，02后映射，因此我们的过滤器自然也是01先拦截，02后拦截

## Filter过滤器的生命周期

Filter的声明周期，其执行init初始化方法时被创建，其服务时执行doFilter方法，其销毁时执行destroy方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8cbb3e1040c6b24f49dc1dccd76d8ca8.png)

测试代码就不构建了，这个知道就可以了

## FilterConfig过滤器配置对象的使用

我们先要解决一个问题，就是什么是FilterConfig，FilterConfig我们已经知道是在我们的Filter对象的init方法里所需要的参数了，其本身是一个接口，同样不需要我们去提供实现类，服务器会给我们提供。而FilterConfig是可以帮助我们去获取过滤器所初始化的一些参数的，至于初始化的参数应该如何配置，这个以后再说，我们现在假设我们的过滤器已经将所有参数都配置好了，只等待我们去用FilterConfig去获取。其下有四个用于获取过滤器参数的方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4c56ff5b467f6c5dfcb2367989063677.png)

我们可以在init方法里调用我们的FilterConfig的方法来获取过滤器的配置信息，我们可以构造测试代码如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEfd78617f6eea689a6c372efef7e5708a.png)

接着我们给予我们的过滤器一些配置

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa927ba8a1efe0227ef18be70d56f9a32.png)

然后我们就可以获取到我们想要的结果了，我们的确获得了过滤器的名称，还获得了我们最开始配置的信息，利用username获得了zhangsan这个数据

关于FilterConfig方法的进一步解释请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8cc62c54e090e5a13455369e3bf1cdf9.png)

## Filter过滤器五种拦截行为

Filter过滤器默认拦截直接向某个功能类的请求，但是在实际开发中，我们还有请求转发、请求包含、以及由服务器出发调用的全局错误界面，这些情况下我们的过滤器是不参与过滤的，如果我们希望在这些情况下过滤器仍然参与过滤，就需要我们进行相应的配置

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEfbcef912fb5396bbd43e2cd414948b1d.png)

配置的方式其实都已经写在图上面了，这里就不再赘述了。这里值得一提的是全局的错误页面，什么是全局错误页面，其又应该要怎么配置？请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEfb6633b7d5ace95d70c35fe1e908c816.png)

可以看到全局错误页面的配置就如上所示，我们先配置了两种情况，一种是出现任何的异常，另一种是出现了404的错误码，这两种情况我们都进行了配置，一旦页面出现了这种异常的任何一种，我们就令其跳转到我们指定的界面

接着我们不妨来看看我们的测试代码，加强一下我们对请求转发和请求包含的理解

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6ef187d2a9485744a85873aa7e8774b1.png)

最后我们来看看我们的配置文件里对我们的过滤器的配置信息，其表示在任何情况下我们的过滤器都会进行拦截，由于异步线程我们还没有学习，因此这里就不提了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe0eabafeaeacb9be08eb00c10af523c0.png)

最后值得一提的是，第一个REQUEST是默认值，其代表任何直接指向功能类的请求会被对应的过滤器所拦截

# Listener

## 监听器的介绍

在学习监听器之前，我们要先学习观察者设计模式，因为所有的监听器都是基于观察者设计模式设计的。

观察者设计模式有三个组组成部分，分别是事件源（触发事件的对象）、事件（触发的动作，其封装了事件源）、监听器（当事件源出发事件后，可以完成一些功能）。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2186ee3cd5fc9834b6823c2117fbc1cc.png)

在程序当中，我们可以对对象的销毁创建、域对象中属性的变化以及会话相关的内容进行监听。Servlet规范中一共提供了共计8个监听器并且都以接口形式提供，具体功能需要我们自己去实现

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE391748e76380053feb79301560fa36b7.png)

## 监听对象的监听器介绍

监听对象很多种，有监听对象的，有监听对象中的属性的 ，本节我们先来介绍监听对象的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf8970acb1ff74277994da4a00ac76406.png)

首先是ServletContextListener，其是用于监听ServletContext对象的创建和销毁，其中ServletContextEvent代表的是事件对象，也是我们的方法的参数。事件对象中封装了事件源，也就是ServeltContext，真正的事件指的是创建或销毁ServeltContext

依葫芦画瓢还有HttpSessionListener

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4cae3d49860a98beaeb89fa99354248d.png)

以及ServletRequest

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6788731d496712cfe33ff5c4049ededc.png)

## 监听域对象属性变化的监听器介绍

这个东西的作用就跟标题说的一模一样其实，这里就不多谈了。直接看图吧，首先是ServletContextAttributeListener

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6cea3e9705f487f62031c573f3cb1b7f.png)

然后是HttpSessionAttributeListener

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE70ddcedb2a78bab3af9cf3db3382bd44.png)

最后是ServletRequestAttributeListener

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE96b8c0b201735ddef227fbafe657ed31.png)

## 监听会话相关的感知型监听器介绍

一般来说，我们的监听器在重写好了我们的方法之后，是要进行配置才能够进行相关的使用的。而感知性监听器的不同之处在于，其可以不用配置直接用

首先是HttpSessionBindingListener

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc86f3b4f0b876d9e33d2b34fc00f25aa.png)

然后是

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbb5a70e28052b309ebaa89714a761d80.png)

## 监听器的使用

经过我们上面的学习，我们也会发现虽然监听器的种类很多，但他们都大同小异，所以关于监听器的使用，我们这里也是只讲一个重点的，后面的各位依葫芦画瓢自己去试就可以了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE604311727d7a0bd6e344eb4244a5f524.png)

那么我们可以写入其测试代码如下

```
package com.itheima.listener;

import javax.servlet.ServletContext;
import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;

/*
    ServletContext对象的创建和销毁的监听器
 */
@WebListener
public class ServletContextListenerDemo implements ServletContextListener {

    /*
        ServletContext对象创建的时候执行此方法
     */
    @Override
    public void contextInitialized(ServletContextEvent servletContextEvent) {
        System.out.println("监听到了对象的创建");

        //获取对象
        ServletContext servletContext = servletContextEvent.getServletContext();
        System.out.println(servletContext);
    }

    /*
        ServletContext对象销毁的时候执行此方法
     */
    @Override
    public void contextDestroyed(ServletContextEvent servletContextEvent) {
        System.out.println("监听到了对象的销毁...");
    }
}

```

这里我们重写好了我们监听器的方法之后还要对监听器进行配置，这里配置的方式同样有两种，注解和文件配置，我们这里采用注解的方式，直接写上@WebListener就完了，后面什么都不用加

## ServletContextAttributeListener监听器的使用

我们接下来来学习ServletContextAttributeListener监听器的使用，至于会话相关的监听器我们就不演示了，因为我们直到会话相关的监听器是不需要配置的，定义了就可以用了。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf498a4b32e84f394ec60458b3c7e2782.png)

那么我们可以构造其测试代码如下

```
package com.itheima.listener;

import javax.servlet.ServletContext;
import javax.servlet.ServletContextAttributeEvent;
import javax.servlet.ServletContextAttributeListener;
import javax.servlet.annotation.WebListener;

/*
    应用域对象中的属性变化的监听器
 */
@WebListener
public class ServletContextAttributeListenerDemo implements ServletContextAttributeListener {

    /*
        向应用域对象中添加属性时执行此方法
     */
    @Override
    public void attributeAdded(ServletContextAttributeEvent servletContextAttributeEvent) {
        System.out.println("监听到了属性的添加...");

        //获取应用域对象
        ServletContext servletContext = servletContextAttributeEvent.getServletContext();
        //获取属性
        Object value = servletContext.getAttribute("username");
        System.out.println(value);
    }

    /*
        向应用域对象中移除属性时执行此方法
     */
    @Override
    public void attributeRemoved(ServletContextAttributeEvent servletContextAttributeEvent) {
        System.out.println("监听到了属性的移除");

        //获取应用域对象
        ServletContext servletContext = servletContextAttributeEvent.getServletContext();
        //获取属性
        Object value = servletContext.getAttribute("username");
        System.out.println(value);
    }

    /*
        向应用域中替换属性时执行此方法
     */
    @Override
    public void attributeReplaced(ServletContextAttributeEvent servletContextAttributeEvent) {
        System.out.println("监听到了属性的替换");

        //获取应用域对象
        ServletContext servletContext = servletContextAttributeEvent.getServletContext();
        //获取属性
        Object value = servletContext.getAttribute("username");
        System.out.println(value);
    }
}

```

我们为了测试，我们在上一个ServletContextListenerDemo类中，我们获取到了ServletContext应用域对象，然后我们的在这个应用域对象里做了添加、替换和移除等动作

```
//添加属性
servletContext.setAttribute("username","zhangsan");

//替换属性
servletContext.setAttribute("username","lisi");

//移除属性
servletContext.removeAttribute("username");
```

然后我们启动服务器，可以获得我们想要的结果，说明我们已经成功了。

# 学生管理系统(终)

那么现在终于到了我们的最后一个案例了，也就是我们最后一个部分的实现，当我们到达这里的时候，我们已经学习完了EL表达式、JSTL、Filter以及Listener的内容，那么接下来我们就用这些新学习的知识来实现我们的案例。不过我们这一次，我们要做的事情是在我们的原来的案例上进行再优化，我们首先来分析下我们的最开始实现的案例有什么问题

## 案例分析

我们最开始实现的案例的问题有三个，第一个是我们手动指定了浏览器的编码格式来解决乱码问题，这个我们应该要改成用过滤器来解决要更好。其次是我们的JSP文件里有一大堆的JSP代码块，非常难看，我们可以使用EL表达式和JSTL来进行相应的优化。最后也是最严重的问题是，我们的项目即使不登录，直接在网址上输入相应的文件名，我们也可以实现查看和添加功能，这是肯定不行的，因为按理说不登录那么就不能使用这些功能，因此我们要解决这个问题

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7439b1562b4008bc0697e043e17f9ab0.png)

## 乱码优化

那么现在我们先来用过滤器解决我们的乱码问题，我们创建一个filter类并写入代码如下

```
/*
    解决全局乱码问题
 */
@WebFilter("/*")
public class EncodingFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        try {
            //1.将请求和响应对象转换为HTTP协议相关的对象
            HttpServletRequest request = (HttpServletRequest) servletRequest;
            HttpServletResponse response = (HttpServletResponse) servletResponse;

            //2.设置编码格式
            request.setCharacterEncoding("UTF-8");
            response.setContentType("text/html;charset=UTF-8");

            //3.放行
            filterChain.doFilter(request,response);
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    @Override
    public void destroy() {

    }
}
```

## 登录功能优化

那么解决了乱码问题之后，接下来我们来实现我们的检查登录的功能，我们同样可以用过滤器来实现，我们可以新创造一个过滤器类并写入其代码如下

```
package com.itheima.filter;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/*
    检查登录
 */
@WebFilter(value = {"/addStudent.jsp","/listStudentServlet"})
public class LoginFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        try {
            //1.将请求和响应对象转换为HTTP协议相关的对象
            HttpServletRequest request = (HttpServletRequest) servletRequest;
            HttpServletResponse response = (HttpServletResponse) servletResponse;

            //2.获取会话域对象中数据
            Object username = request.getSession().getAttribute("username");

            //3.判断用户名
            if(username == null || "".equals(username)){
                //重定向到登录页面
                response.sendRedirect(request.getContextPath()+"/login.jsp");
                return;
            }

            //4.放行
            filterChain.doFilter(request,response);
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    @Override
    public void destroy() {

    }
}

```

这里我们是通过使用过滤器查看会话域中是否有数据的方式来决定要不要令页面跳转到登录页面的形式的，这里我们希望过滤器只过滤两个关键的功能类，因此我们这里的注解配置采用了value数组的形式，并且只写入了两个功能类的地址

## JSP文件优化

那么最后我们来解决最后一个问题，用EL表达式来改进我们的JSP文件，首先是我们的addStudent内的代码

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>添加学生</title>
</head>
<body>
<!--提前通过Run Edit设置其虚拟目录为stu，此处先写入虚拟目录，然后写入构造的类，就可以将数据传入到指定的类-->
    <form action="${pageContext.request.contextPath}/addStudentServlet" method="get" autocomplete="off">
        学生姓名：<input type="text" name="username"> <br/>
        学生年龄：<input type="number" name="age"> <br/>
        学生成绩：<input type="number" name="score"><br/>
        <button type="submit">保存</button>
    </form>
</body>
</html>

```

这里我们用EL表达式动态获取我们的虚拟目录，这样即使我们的虚拟目录改变了我们的代码仍然可以正确运行

接着是我们首页的jsp

```
<%--
    1.page指令
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<html>
  <head>
    <title>学生管理系统首页</title>
  </head>
  <body>
        <%--
            获取会话域中的数据
            如果获取到了则显示添加和查看功能的超链接
            反之则显示登录功能的超链接
        --%>
            <c:if test="${sessionScope.username eq null}">
                <a href="${pageContext.request.contextPath}/login.jsp">请登录</a>
            </c:if>

            <c:if test="${sessionScope.username ne null}">
                <a href="${pageContext.request.contextPath}/addStudent.jsp">添加学生</a>
                <a href="${pageContext.request.contextPath}/listStudentServlet">查看学生</a>
            </c:if>
  </body>
</html>

```

这里我们结合了JSTL和EL表达式共同构建我们的代码，最终的效果就让我们的代码看起来清爽很多

然后是查看学生的JSP

```
<%@ page import="java.util.ArrayList" %>
<%@ page import="com.itheima.bean.Student" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<html>
<head>
    <title>查看学生</title>
</head>
<body>
    <table width="600px" border="1px">
        <tr>
            <th>学生姓名</th>
            <th>学生年龄</th>
            <th>学生成绩</th>
        </tr>
        <c:forEach items="${students}" var="s">
            <tr align="center">
                <td>${s.username}</td>
                <td>${s.age}</td>
                <td>${s.score}</td>
            </tr>
        </c:forEach>
    </table>
</body>
</html>

```

我们这里使用打了forEach循环来帮助我们改造我们的代码

最后我们来改造我们登录页面的JSP

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>学生登录</title>
</head>
<body>
    <form action="${pageContext.request.contextPath}/loginStudentServlet" method="get" autocomplete="off">
        用户名：<input type="text" name="username"> <br>
        密码：<input type="password" name="password"> <br>
        <button type="submit">登录</button>
    </form>
</body>
</html>

```

其实就只是修改了一个路径获取方式，无了。

最后由于我们采用工具类添加学生的方式有问题，所以我们换回手动方式来，下面是手动封装的代码

```
package com.itheima.servlet;

import com.itheima.bean.Student;
import org.apache.commons.beanutils.BeanUtils;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.util.Map;

/*
    实现添加功能
 */
@WebServlet("/addStudentServlet")
public class AddStudentServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException, IOException {
        //1.获取表单中的数据
        String username = req.getParameter("username");
        String age = req.getParameter("age");
        String score = req.getParameter("score");

        //创建学生对象并赋值
        Student stu = new Student();
        stu.setUsername(username);
        stu.setAge(Integer.parseInt(age));
        stu.setScore(Integer.parseInt(score));

        //3.将学生对象的数据保存到d:\\stu.txt文件中
        BufferedWriter bw = new BufferedWriter(new FileWriter("d:\\stu.txt",true));
        bw.write(stu.getUsername()+","+stu.getAge()+","+stu.getScore());
        bw.newLine();
        bw.close();

        //4.通过定时刷新功能响应给浏览器
        //resp.setContentType("text/html;charset=UTF-8");
        resp.getWriter().write("添加成功，2秒后自动跳转到首页");
        resp.setHeader("Refresh","2;URL=/stu/index.jsp");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

虽然最后测试的时候有问题，检查了几遍，感觉代码上没什么问题，那就不管了，反正学到了知识就够了。那么最后，我们这一章，就算是完毕了。