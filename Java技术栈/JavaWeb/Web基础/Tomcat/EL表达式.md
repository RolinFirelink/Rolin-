本章节我们要加快速度了，因此除了必要的代码之外，其他简单的代码示例就直接截图给出了

- EL表达式的介绍

EL表达式是Servlet规范的一部分，其作用是可以再JSP页面中获取数据，让我们的JSP脱离java代码块和JSP表达式，反正就是可以让我们的编程变得更加简单

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE9fae2351c63e164480531ca3d18955d5.png)

- EL表达式的快速入门

同样我们也用一个案例来进行快速的入门，先来看看步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE76dc0fb95a253b8489517bcc31a3dc6b.png)

我们的目标是使用三种方式来获取我们域对象的数据，当然在获取之前我们应该要先往里面设置数据，那么最后我们可以构造其代码如下

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa4d1df81ae9386176645f5cf6212d261.png)

我们显然可以看到，EL表达式获取数据对比前两种方式，其更加简单快捷。当然有的同学可能会有疑问，就是为什么EL表达式里可以直接写入username就可以获取，而不用写上请求域的代码，这个留到我们后面去解释

- EL表达式获取不同类型的数据

本节我们来学习如何使用EL表达式获取各种不同类型的数据，请看步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE512143b89dbb02b74d997764a56075e7.png)

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

- EL表达式注意事项

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE0e445d449a232d3ae1e0512950b0a813.png)

简单来说就是本来应该报出异常的，在EL表达式里不会报这些异常，只会什么都不显示。而关于字符串的拼接，简单来说就是在EL表达式里两个字符串如果使用+号进行拼接，那么这个拼接不会成功，而且+号会被一起打印

- EL表达式的运算符

首先是关系运算符，其记法和作用以前已经讲过了，所以直接看图吧

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8555132ea28902b152c9ab24f81fe2c1.png)

然后是逻辑运算符

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8b06011917fc626b83875826c82761d3.png)

最后也是最重要的其他运算符

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE95937f99708c4cc2b2874159c5d99da5.png)

我们本节重点所需要演示的就是其他运算符，那么我们构造其演示代码如下

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEeff7a7f49c8db648f6910ea54709bcb7.png)

最后我们的到的结果是这样的

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEba419ea4c8fc930b70e630915081da25.png)

前三个true是因为我们的empty进行的判断，可以看到我们的empty的确有判断对象是否为null以及是否为空字符串和容器内元素是否为0的功能。最后一个功能是我们希望实现判断用户提供的数据里其是男是女，若是男我们就自动给其性别先勾选为男，因此我们这里利用EL表达式进行了一个三元运算符的判断，判断其是否为men，若是则输入checked，代表自动勾选，不是则不写入，代表不自动勾选

- EL表达式的使用细节

这里的使用细节其实和我们之前学习的JSP的细节差不多，其都可以调用其他八大隐式对象和四大域对象。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd71feea66179e0281f3255cce9450af6.png)

但有些不同的是，EL表达式是直接根据名称从小到大在域对象中查找的，一旦查找到其就拿此值作为它的值，后续的值就不会再检查了。那么我们可以构造其测试代码如下

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE65cafa2ec1cd6b973839371fb5704534.png)

如果忘了什么是隐式对象，记得及时回去复习

- EL表达式隐式对象

本节我们来学习如何用EL表达式来表达隐式对象，EL表达式中一共有11个隐式对象，我们来一起看看其功能和不同

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE161370efbc9ceb1f28f4b97d094fc31e.png)

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

