本章节我们来学习AJAX，这虽然说也是前端的技术，但是看了下后面的内容似乎涉及到了java代码本身，所以就稍微认真一点吧，首先来看看AJAX的介绍

- AJAX的介绍

首先AJAX的最重的两点是，其是异步的，其次是其可以实现在不重新加载整个页面的情况下实现对部分内容进行局部更新。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5d4024a7dd5c55f1fcc6bd5cf686434e.png)

那么什么是同步和异步？简单来说就是，在同步情况下，服务器一旦处理请求，其不可以再进行其他操作，而在异步中是可以的

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe49f498d06eeec24dade9fa1762dd55b.png)

- 原生JS方式实现AJAX

本节内容我们做对应的了解就可以了，并不要求着重学习。首先我们要创建一个对应的web模块，然后我们在web目录下创建regist01.html并写入代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>用户注册</title>
</head>
<body>
<form autocomplete="off">
    姓名：<input type="text" id="username">
    <span id="uSpan"></span>
    <br>
    密码：<input type="password" id="password">
    <br>
    <input type="submit" value="注册">
</form>
</body>
<script>
    //1.为姓名绑定失去焦点事件
    document.getElementById("username").onblur = function() {
        //2.创建XMLHttpRequest核心对象
        let xmlHttp = new XMLHttpRequest();

        //3.打开链接
        let username = document.getElementById("username").value;
        xmlHttp.open("GET","userServlet?username="+username,true);
        //xmlHttp.open("GET","userServlet?username="+username,false);

        //4.发送请求
        xmlHttp.send();

        //5.处理响应
        xmlHttp.onreadystatechange = function() {
            //判断请求和响应是否成功
            if(xmlHttp.readyState == 4 && xmlHttp.status == 200) {
                //将响应的数据显示到span标签
                document.getElementById("uSpan").innerHTML = xmlHttp.responseText;
            }
        }
    }
</script>
</html>
```

然后我们创建对应的实现类com.itheima01.UserServlet类并写入代码如下

```
package com.itheima01;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
@WebServlet("/userServlet")
public class UserServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //设置请求和响应的乱码
        req.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=UTF-8");

        //1.获取请求参数
        String username = req.getParameter("username");

        //模拟服务器处理请求需要5秒钟
        /*try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }*/

        //2.判断姓名是否已注册
        if("zhangsan".equals(username)) {
            resp.getWriter().write("<font color='red'>用户名已注册</font>");
        }else {
            resp.getWriter().write("<font color='green'>用户名可用</font>");
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}

```

然后创建一个对应的js文件夹并放入js文件，然后运行该项目会发现项目的确能运行，那就没问题了。

- 原生JS实现AJAX的步骤详解

然后我们来分析下我们的步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE643b3389e49d7072322d07a6cfc32fe9.png)

最后值得一提的是，由于我们这里对这两节内容只做了解，所以没做太多注释，如果要复习属实看不懂，那还是拿出视频来自己温习温习吧

- jQuery的GET方式实现AJAX

然后我们这节就用jQuery的GET方式来去实现我们上一个案例的相同的需求，看来看看步骤以及对应的方法分析

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE7a69ddec0b740a4c9d58ba63137cb6e5.png)

那么我们新创建一个regist2.html并写入其代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>用户注册</title>
</head>
<body>
<form autocomplete="off">
    姓名：<input type="text" id="username">
    <span id="uSpan"></span>
    <br>
    密码：<input type="password" id="password">
    <br>
    <input type="submit" value="注册">
</form>
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    //1.为用户名绑定失去焦点事件
    $("#username").blur(function () {
        let username = $("#username").val();
        //2.jQuery的GET方式实现AJAX
        $.get(
            //请求的资源路径
            "userServlet",
            //请求参数
            "username=" + username,
            //回调函数
            function (data) {
                //将响应的数据显示到span标签
                $("#uSpan").html(data);
            },
            //响应数据形式
            "text"
        );
    });
</script>
</html>
```

可以看到我们的get方式其实就是$.get()，然后其下我们给其传入对应的参数，这里输入数据的内容是通过21行代码获得的（这行代码调用了.val()方法获得数据），并且我们给其进行了一个字符串的拼接，最后值得一提的是，回调函数就是处理相应的函数，我们这里接收的data对象就是响应对象，我们令其展示到浏览器上

最后我们会发现这个案例的效果和之前的一模一样，而代码实现方式比起前一个简单了不少

- jQuery的POST方式实现AJAX

然后我们来看看用POST方式实现AJAX的步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE13f609d69a1e8276ffe99958d6b717b6.png)

然后我们可以写入其代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>用户注册</title>
</head>
<body>
<form autocomplete="off">
    姓名：<input type="text" id="username">
    <span id="uSpan"></span>
    <br>
    密码：<input type="password" id="password">
    <br>
    <input type="submit" value="注册">
</form>
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    //1.为用户名绑定失去焦点事件
    $("#username").blur(function () {
        let username = $("#username").val();
        //2.jQuery的POST方式实现AJAX
        $.post(
            //请求的资源路径
            "userServlet",
            //请求参数
            "username=" + username,
            //回调函数
            function (data) {
                //将响应的数据显示到span标签
                $("#uSpan").html(data);
            },
            //响应数据形式
            "text"
        );
    });
</script>
</html>
```

其实有改动的地方也就是$.get()变成$.post()而已，其他的倒是没有变动，注意我们这里响应数据形式我们都写文本形式，其实一般都是用json格式的，但是我们这里还没有学习在java中如何将数据格式转换为json格式，因此这个格式先按下不表

- jQuery的通用方式实现AJAX

最后我们来介绍一种通用方式，这种方式可以随便指定返回方式，泛用性广

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE55643e4498528711e7b6c2b49085cc97.png)

那么我们可以写入其代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>用户注册</title>
</head>
<body>
<form autocomplete="off">
    姓名：<input type="text" id="username">
    <span id="uSpan"></span>
    <br>
    密码：<input type="password" id="password">
    <br>
    <input type="submit" value="注册">
</form>
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    //1.为用户名绑定失去焦点事件
    $("#username").blur(function () {
        let username = $("#username").val();
        //2.jQuery的通用方式实现AJAX
        $.ajax({
            //请求资源路径
            url:"userServletxxx",
            //是否异步
            async:true,
            //请求参数
            data:"username="+username,
            //请求方式
            type:"POST",
            //数据形式
            dataType:"text",
            //请求成功后调用的回调函数
            success:function (data) {
                //将响应的数据显示到span标签
                $("#uSpan").html(data);
            },
            //请求失败后调用的回调函数
            error:function () {
                alert("操作失败...");
            }
        });
    });
</script>
</html>
```

- AJAX快速入门的小结

最后我们做一个小结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE252fda35053d4090baf9c3a72d6d4df0.png)



![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8671eb5e4a44a2d7b202dae8eea9bdf7.png)

