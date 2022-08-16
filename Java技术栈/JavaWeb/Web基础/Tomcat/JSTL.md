- JSTL的介绍

JSTL是JSP标准标签库的简称，其主要作用就是提供给开发人员一个标准同样的标签库。开发人员可以利用这些标签取代JSP页面上的Java代码，从而提高程序的可读性，降低程序的维护难度。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE09b7e78c95d29de28bf20814bc962479.png)

我们的标签库有很多，我们这里主要学习core，核心标签库

- JSTL的核心标签使用

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE220348a44cae991fc40d323732b1ec0c.png)

我们要学习的核心标签并不多，就图上这三个。先来看看步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEdb9b67cb87f31b25775df4e7be2c8a66.png)

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE46cdc76c25db152d79c092c7c70149a7.png)

这里我们的items里面要填写的是我们要遍历的集合对象，而var则是我们每次将遍历的对象所保存的地方，这个其实不难理解，我们每次都拿到str并打印，这样我们就可以实现迭代我们集合并打印元素的功能