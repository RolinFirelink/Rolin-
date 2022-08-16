照例我们先来介绍下什么是JSP

- JSP的介绍

首先JSP的全称是Java Server Pages，是一种动态网页技术标准。其次是JSP部署在服务器上，其不但可以处理客户端发送的请求，并且还可以根据请求内容动态生成HTML、XML文件等Web网页再响应给客户端。我们之前学习过Servlet也是用于处理客户端发送的请求并返回给客户端的，而这个也可以，这说明了什么？这说明了JSP的本质，其实就是Servlet

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa45119e8426e507c9cde410106e8498b.png)

最后再上图里我们还复习了我们曾经学习过的技术和未来要学习的相关技术，自己去看吧

- JSP快速入门

接着我们就来学习我们的JSP，我们首先创建一个JSP的新项目，具体请看步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa266f4b3aa0a798a0f80890b33eb9142.png)

其实我们一创建完我们就可以直接看到其已经帮我们生成了index.jsp文件了，其代码如下

```
<%--
  Created by IntelliJ IDEA.
  User: 22592
  Date: 2022/3/17
  Time: 13:37
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  $END$
  </body>
</html>

```

可以看到这个代码跟我们以前学习的HTML文件不能说十分相似，只能说一模一样。最上面的是注释信息，这个可以不用管。然后我们看到第8行，可以看到contentType="text/html;charset=UTF-8"，这个跟我们之前学习的指定编码格式的代码不谋而合，那么现在我们也应该能明白为什么我们指定编码格式的字符串总是统一格式了的，因为页面只能这样读取，我们的方法那样写最终会在页面将这个语句交给浏览器识别并执行

我们将我们的代码改造如下

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>JSP</title>
  </head>
  <body>
  我还就那个永失吾爱，举目破败
  </body>
</html>
```

如果是以前的话，我们可以直接通过浏览器来查看我们的HTML文件的效果，但是我们这个是JSP文件，要查看的话必须要通过服务器响应的方式来查看，因此我们要启动Tomcat服务器，然后打开这个文件来查看

最后查看我们发现的确有这个效果，那么我们的案例就成功了

- JSP执行过程

那么为什么我们的JSP文件不可以直接执行呢？而一定要通过Tomcat服务器发布文件的形式来可以查看到呢？要解决这个问题，我们就必须分析JSP的执行过程了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc6c18cdff73f0ac9397efde5c3a6cb4e.png)

首先，我们的客户端浏览器向Tomcat服务器发起请求，服务器接收到请求之后会解析请求地址然后找到具体应用，然后其会找到jsp文件，但是jsp文件本身啥也干不了，因此其会将jsp文件翻译成java文件，然后运行该java文件生成字节码文件，接着将该字节码文件响应给客户端浏览器，最终我们就可以浏览器上看到我们的页面效果。这里翻译和编译过程，都是Tomcat帮我们做的，我们不用去理会。

- JSP生成的文件内容介绍

为了更近一步的去了解JSP，我们现在来查看下其翻译的JSP的java文件的内容，一打开对应的java文件，我们就可以看到下面这个内容

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe75d1b943db79440f0c89dedcd5d795f.png)

可以看到我们翻译之后的jsp文件是继承了一个叫HttpJspBase的类的，我们接着再来查看下这个类

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc593aab869a5e82d91ee7979cb6c553e.png)

我们可以轻易地发现这个类其实是继承了HttpServlet类的，所以说其实我们的JSP本质上还是一个Servlet，因为其也继承了HttpServlet，下面当然也有HttpServlet的方法，而且还要有一些它自己定义的方法。不但如此，我们回到原来的java类中继续往下看，还可以看到其也有初始化，摧毁以及service方法，这更加说明了其本质就是一个Servlet

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa7b49334333a922fa9695fc5af271363.png)

当然上面的不算太重要，真正重量级的还是下面的内容，我们一起来看

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa3e59cab469e4469feb96c57596d15e1.png)

我们可以看到，这个代码里，先指定了编码格式，然后获得了各种域对象，接着其获得了一个输出流，直接向浏览器内响应了当初我们写出的内容，这就是为什么我们能够在浏览器里看到我们的写入的内容的原因，因为其帮我们响应过去了

最后我们可以做一个总结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5d78d4af5693922b15e96c3550a14844.png)

- JSP的语法

接下来我们来学习JSP的语法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8fd250107934ec1efc9500eb3a184201.png)

我们可以构造测试代码如下

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>JSP</title>
  </head>
  <body>
    <%--
        1.这是注释
    --%>

    <%--
        2.java代码块
        System.out.println("Hello JSP");普通输出语句，输出在控制台
        out.println("Hello JSP");
            out是JspWriter对象，输出在页面上
        如果想要在页面上实现换行，使用Printlin语句是没有用的，必须在字符串
        结尾处加上<br>
    --%>
  <%
    System.out.println("Hello JSP<br>");
    out.println("hello JSP<br>");
    String str = "hello<br>";
    out.println(str);
  %>

  <%--
      3.jsp表达式
      <%="Hello"%>  相当于 out.println("Hello")
  --%>
  <%="Hello<br>"%>

  <%--
      4.jsp中的声明(变量或方法)
      如果加！代表的是声明的是成员变量
      如果不加！代表的是声明的是局部变量
      方法必须定义在！中，否则则是试图在方法内部定义方法，编译无法通过
  --%>
  <%!String s = "abc";%>
  <%String s = "abc";%>
  <%=s%>
  <%! public void getSum(){}%>
  </body>
</html>

```

知识点的相关内容都写在注释里了，因此这里就不再赘述了

- JSP的指令

学习完了JSP的语法之后，我们现在来学习JSP的指令，具体请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE9dcc8b65a1d7f76205bf0f44ed2e61f8.png)

可以看到有点小多啊，但是没关系，因为大多数指令我们的服务器都给赋予一个初始的值，我们哪怕不关心也没有问题，不过我们这里还是演示两个代码，一个是errorPage，一个是import

那么我们可以构造测试代码如下

```
<%--
    1.page指令
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" errorPage="/error.jsp" import="java.util.ArrayList" %>
<html>
  <head>
    <title>JSP</title>
  </head>
  <body>
      <% int result = 1 / 0; %>
      <%ArrayList list = new ArrayList<>();%>
  </body>
</html>

```

这个error.jsp我们已经预先创建了，不过没写什么代码，这里不再放一遍了，因为没什么意义。

接着还有一个include指令和taglib指令，前者的作用是引入其他界面，然后可以调用其他界面定义的变量。后者是可以引入外部标签库，然后可以使用哪些标签。这里我们就不做演示了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE99d5219750a75429d04527df3587ff72.png)

- JSP的使用细节

JSP的使用里有九大隐式对象，他们的名称及其实际代表对象如下图所示

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEbb863202fe96456102335a4edda4dcdd.png)

这里我们要着重讲下pageContext隐式对象，因为他是四大域里面的最后一个，就是页面域。

首先PageContxt是JSP中独有的，且不但可以操作其他三个域对象中的属性，还可以获取其他八个隐式对象。其生命周期随着JSP的创建而存在，也随着JSP的结束而消失，并且每一个JSP页面都有一个PageContext对象

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5dcf22f2d9a56b5e3c1612d4ff7d55f5.png)

最后我们可以来看看PageContxt对象的方法详解

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd62391aa2d5af11c3fd3c18c2f4c3c3f.png)

- 四大域对象的复习

接下来我们对我们四大域进行一个复习。我们的四大域共有PageContxt（页面域，范围最小）、ServletRequest（请求域）、HttpSession（会话域）、ServletContext（应用域）。这四大域的区别和级别都在下图中有讲解了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE081949f6cfcd0b2a29620e20f9e7e2d9.png)

- MVC模型的介绍

接下来我们介绍一种比较经典的一种业务开发模型，MVC模型。所谓MVC模型，M指的是Model，也就是模型，用于封装数据模型。而V代表view，意为视图，用于显示数据，动态资源用JSP，静态则是用HTML。最后是C，其为Controller，意为控制器，用于处理请求和响应，例如Servlet

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc30807bc587092289896fa5943c51ff2.png)

上图中我们的DB代表的是数据库，MVC的流程是先从客户端浏览器向控制器发起请求，然后控制器将请求通过模型进行业务处理，然后模型通过数据库进行数据处理，然后数据库返回数据给模型，接着模型返回数据模型给控制器，控制器会通过数据模型选择合适的视图来展示模型，最后返回响应给客户端浏览器。

这个模型的最大好处当然就是降低耦合度了，这个大家都懂，不多说了











