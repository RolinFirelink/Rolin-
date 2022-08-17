本章节我们来学习AJAX，这虽然说也是前端的技术，但是看了下后面的内容似乎涉及到了java代码本身，所以就稍微认真一点吧，首先来看看AJAX的介绍

# AJAX快速入门

首先AJAX的最重的两点是，其是异步的，其次是其可以实现在不重新加载整个页面的情况下实现对部分内容进行局部更新。

## AJAX介绍

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5d4024a7dd5c55f1fcc6bd5cf686434e.png)

那么什么是同步和异步？简单来说就是，在同步情况下，服务器一旦处理请求，其不可以再进行其他操作，而在异步中是可以的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe49f498d06eeec24dade9fa1762dd55b.png)

## 原生JS方式实现AJAX

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

## 原生JS实现AJAX的步骤详解

然后我们来分析下我们的步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE643b3389e49d7072322d07a6cfc32fe9.png)

最后值得一提的是，由于我们这里对这两节内容只做了解，所以没做太多注释，如果要复习属实看不懂，那还是拿出视频来自己温习温习吧

## jQuery的GET方式实现AJAX

然后我们这节就用jQuery的GET方式来去实现我们上一个案例的相同的需求，看来看看步骤以及对应的方法分析

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7a69ddec0b740a4c9d58ba63137cb6e5.png)

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

## jQuery的POST方式实现AJAX

然后我们来看看用POST方式实现AJAX的步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE252fda35053d4090baf9c3a72d6d4df0.png)

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

## jQuery的通用方式实现AJAX

最后我们来介绍一种通用方式，这种方式可以随便指定返回方式，泛用性广

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE55643e4498528711e7b6c2b49085cc97.png)

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

## AJAX快速入门的小结

最后我们做一个小结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8671eb5e4a44a2d7b202dae8eea9bdf7.png)

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE13f609d69a1e8276ffe99958d6b717b6.png)

# JSON的处理

之前我们说过，我们最常用的返回数据的格式是JSON，因此本节我们来学习JSON的处理

## JSON的回顾

首先我们进行一个回顾

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEdee24e9d026bf9f43fa7e86324adb971.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9a62f13251505c61538e1482087fce62.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE579771d09eeb3a27f4389c420b5eafe1.png)

## JSON转换工具的介绍

本章节我们来介绍下我们的JSON的转换工具，Jackson，其是开源免费的JSON转换工具，SpringMVC转换默认使用Jackson

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc90aa9fb7ed951bcef8c9e9dada66f7f.png)

然后我们来看看在该工具类里的常用类的常用方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6ddeb801598c9591be65334f7e3813e2.png)

## JSON的转换练习

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb399cf861bec09dfd2e59006dfd9fc57.png)

这个练习有时间自己做吧，我现在反正不想做，实际开发的时候就没出现过我手写JSON的情况

# 搜索联想案例

学习完了基本的语法之后，我们现在来实现一个搜索联想的案例

## 案例效果和环境的介绍

案例的内容很简单，就是实现一个模糊搜索

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa8e901a629ae31f804a98c319a4bc773.png)

## 案例的实现

先来看看我们实现案例的步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5dc1bb3daba6f2d0a401db5b74c25950.png)

然后我们要知道，我们的案例只需要我们实现对应的页面代码和服务器的处理代码，其他的代码都已经事先帮我们做好了。我们这里先贴出来

先看看我们的案例的包的分层格式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3bbebb4a4e9613f19c075ca0301a5139.png)

首先是我们创建对应的数据的sql代码

```
-- 创建db10数据库
        CREATE DATABASE db10;

        -- 使用db10数据库
        USE db10;

        -- 创建user表
        CREATE TABLE USER(
        id INT PRIMARY KEY AUTO_INCREMENT, -- 主键id
        NAME VARCHAR(20),        -- 姓名
        age INT,            -- 年龄
        search_count INT                        -- 搜索数量
        );

        -- 插入测试数据
        INSERT INTO USER VALUES (NULL,'张三',23,25),(NULL,'李四',24,5),(NULL,'王五',25,3)
        ,(NULL,'赵六',26,7),(NULL,'张三丰',93,20),(NULL,'张无忌',18,23),(NULL,'张强',33,21),(NULL,'张果老',65,6);
```

然后是我们的连接的代码config.properties，这个代码只放一个格式，具体内容还需要我们进行更改

```
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://192.168.59.143:3306/db10
username=root
password=itheima
```

然后是log4j日志文件的固定代码

```
# Global logging configuration
log4j.rootLogger=DEBUG, stdout
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

然后是我们的Mybatis的核心配置文件

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--configuration：核心根标签-->
<configuration>

    <!--引入数据库连接信息配置文件-->
    <properties resource="config.properties"/>

    <!--集成LOG4J日志信息-->
    <settings>
        <setting name="logImpl" value="log4j"/>
    </settings>

    <!--配置数据库环境，可以有多个。default指定默认使用哪个-->
    <environments default="mysql">
        <!--配置环境，id是唯一标识-->
        <environment id="mysql">
            <!--事务管理 采用JDBC默认的事务-->
            <transactionManager type="JDBC" />
            <!--数据源：连接池-->
            <dataSource type="POOLED">
                <!--获取数据库连接相关配置-->
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <!--配置映射关系-->
    <mappers>
        <!--接口所在的包-->
        <package name="com.itheima.mapper"/>
    </mappers>

</configuration>
```

接着是模糊查询的代码，这里我们要注意我们如果在sql中使用字符串的拼接，就要使用CONCAT字符串

```
package com.itheima.mapper;

import com.itheima.bean.User;
import org.apache.ibatis.annotations.Select;

import java.util.List;

public interface UserMapper {
    /*
        模糊查询
     */
    @Select("SELECT * FROM user WHERE name LIKE CONCAT('%',#{username},'%') ORDER BY search_count DESC LIMIT 0,4")
    public abstract List<User> selectLike(String username);
}

```

然后是我们的模糊查询的具体的实现类的代码，当然，这里还有一个接口的才是，但是我们这里就不放出来了

```
package com.itheima.service.impl;

import com.itheima.bean.User;
import com.itheima.mapper.UserMapper;
import com.itheima.service.UserService;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class UserServiceImpl implements UserService {
    @Override
    public List<User> selectLike(String username) {
        List<User> users = null;
        SqlSession sqlSession = null;
        InputStream is = null;
        try{
            //1.加载核心配置文件
            is = Resources.getResourceAsStream("MyBatisConfig.xml");
            //2.获取SqlSession工厂对象
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
            //3.通过SqlSession工厂对象获取SqlSession对象
            sqlSession = sqlSessionFactory.openSession(true);
            //4.获取UserMapper接口的实现类对象
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            //5.调用实现类对象的模糊查询方法
            users = mapper.selectLike(username);
        }catch (Exception e) {
            e.printStackTrace();
        }finally {
            //6.释放资源
            if(sqlSession != null) {
                sqlSession.close();
            }
            if(is != null) {
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
        //7.返回结果到控制层
        return users;
    }
}

```

然后是我们的页面本身的参数，这里还有其他的一些图片的内容，这里就不一一放出来了

到这里为止我们就可以正式来实现我们的案例了，首先我们要创建对应的User类，我们将其放置于bean包下

```
package com.itheima.bean;
/*
    用户的实体类
 */
public class User {
    private Integer id;              // 主键id
    private String name;             // 姓名
    private Integer age;             // 年龄
    private Integer search_count;   //搜索数量

    public User() {
    }

    public User(Integer id, String name, Integer age, Integer search_count) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.search_count = search_count;
    }

    public Integer getSearch_count() {
        return search_count;
    }

    public void setSearch_count(Integer search_count) {
        this.search_count = search_count;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }


}

```

然后我们在对应的controller包里写入我们的服务器端处理用户发送的请求的代码

```java
package com.itheima.controller;

import com.fasterxml.jackson.databind.ObjectMapper;
import com.itheima.bean.User;
import com.itheima.service.UserService;
import com.itheima.service.impl.UserServiceImpl;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.List;

@WebServlet("/userServlet")
public class UserServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //设置请求和响应的编码
        req.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=UTF-8");

        //1.获取请求参数
        String username = req.getParameter("username");

        //2.调用业务层的模糊查询方法得到数据
        UserService service = new UserServiceImpl();
        List<User> users = service.selectLike(username);

        //3.将数据转成JSON，响应到客户端
        ObjectMapper mapper = new ObjectMapper();
        String json = mapper.writeValueAsString(users);
        resp.getWriter().write(json);
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req,resp);
    }
}

```

我们这里的处理就是获得其请求的参数，然后调用业务层的模糊查询方法得到我们的数据，这个数据是一个List集合，然后将其转换为JSON格式之后直接响应到用户的浏览器中

最后我们在界面中做对应的处理，我们可以写入其代码如下

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>用户搜索</title>
    <style type="text/css">
        .content {
            width: 643px;
            margin: 100px auto;
            text-align: center;
        }

        input[type='text'] {
            width: 530px;
            height: 40px;
            font-size: 14px;
        }

        input[type='button'] {
            width: 100px;
            height: 46px;
            background: #38f;
            border: 0;
            color: #fff;
            font-size: 15px
        }

        .show {
            position: absolute;
            width: 535px;
            height: 100px;
            border: 1px solid #999;
            border-top: 0;
            display: none;
        }
    </style>
</head>
<body>
<form autocomplete="off">
    <div class="content">
        <img src="img/logo.jpg">
        <br/><br/>
        <input type="text" id="username">
        <input type="button" value="搜索一下">
        <!--用于显示联想的数据-->
        <div id="show" class="show"></div>
    </div>
</form>
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    //1.为用户名输入框绑定鼠标点击事件
    $("#username").mousedown(function () {
        //2.获取输入的用户名
        let username = $("#username").val();

        //3.判断用户名是否为空
        if(username == null || username == "") {
            //4.如果为空，将联想框隐藏
            $("#show").hide();
            return;
        }

        //5.如果不为空，发送AJAX请求。并将数据显示到联想框
        $.ajax({
            //请求的资源路径
            url:"userServlet",
            //请求参数
            data:{"username":username},
            //请求方式
            type:"POST",
            //响应数据形式
            dataType:"json",
            //请求成功后的回调函数
            success:function (data) {
                //将返回的数据显示到show的div
                let names = "";
                for(let i = 0; i < data.length; i++) {
                    names += "<div>"+data[i].name+"</div>";
                }
                $("#show").html(names);
                $("#show").show();
            }
        });
    });


</script>
</html>
```

上面50行之后的代码才是我们所需要的代码，之前的代码都是帮我们事先写好的样式控制。这里值得一提的是，由于我们传入的json的数据是由集合转换过来的，实际上我们在页面了处理响应的数据时，其会自动将json数据转换为一个集合，因此我们可以用集合来处理这个data对象，来具体控制我们想要其添加的位置和显示的地方

最后我们放上该案例的源码压缩包，需要自取

# 分页案例

最后我们来学习本章节的最后一个内容，分页案例。我们要用两种方式去实现，我们先来讲第一种方式，瀑布流

## 瀑布流

### 瀑布流分页效果和环境的介绍

所谓瀑布流，也就是指我们的分页可以不断往下显示内容，直到数据库中没有内容为止，只要用户往下拉就可以下载。在项目环境上，我们需要创建对应的实现类和查询语句，这些我们就先做好了，我们只需要实现分页效果以及令其正确显示到浏览器上即可

### 瀑布流分页前置知识点分析

那么在正式实现我们的案例之前，我们必须要学习一些前置的知识，这些知识就如下图所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6b00f103774be99b5917e1311b03b0f4.png)

### 瀑布流分页案例的实现

首先我们来看看我们的页面设置的步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe2b49b2761bf089963bb9a58051a3ba1.png)

然后来看看我们服务器的代码的设置步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEed99a6f094ef947ba43112d55a593c4d.png)

最后在实现之前，我们要说一下，我们这个案例是需要使用到分页助手和一大堆有的没的，这里我们就不再一一介绍了，我们只介绍重点的部分，那就是我们自己要构造的代码

那么我们可以构造服务器的代码如下

```
package com.itheima.controller;

import com.fasterxml.jackson.databind.ObjectMapper;
import com.github.pagehelper.Page;
import com.itheima.service.NewsService;
import com.itheima.service.impl.NewsServiceImpl;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
@WebServlet("/newsServlet")
public class NewsServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //设置请求和响应的编码
        req.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=UTF-8");

        //1.获取请求参数
        String start = req.getParameter("start");
        String pageSize = req.getParameter("pageSize");

        //2.根据当前页码和每页显示的条数来调用业务层的查询方法，得到分页Page对象
        NewsService service = new NewsServiceImpl();
        Page page = service.pageQuery(Integer.parseInt(start), Integer.parseInt(pageSize));

        //3.将得到的数据转为JSON
        String json = new ObjectMapper().writeValueAsString(page);

        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        //4.将数据响应给客户端
        resp.getWriter().write(json);
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req,resp);
    }
}

```

我们这里服务器端的代码逻辑是先获得请求对象的两个参数，然后调用对应的分页方法获得对应页码的数据，也就是Page，将该数据转为JSON格式之后响应给客户端

然后我们可以写入我们的页面代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>网站首页</title>
    <link rel="stylesheet" href="css/tt.css">
</head>
<body>
<div class="top">
    <span class="top-left">下载APP</span>
    <span class="top-left"> 北京         晴天</span>
    <span class="top-right">更多产品</span>
</div>

<div class="container">

    <div class="left">
        <a>
            <img src="img/logo.png"><br/>
        </a>

        <ul>
            <li>
                <a class="channel-item active" href="#">
                    <span>
                        推荐
                    </span>
                </a>
            </li>

            <li><a class="channel-item" href="#">
                <span>
                    视频
                </span>
            </a></li>

            <li><a class="channel-item" href="#">
                <span>
                    热点
                </span>
            </a></li>

            <li><a class="channel-item" href="#">
                <span>
                    直播
                </span>
            </a></li>

            <li><a class="channel-item" href="#">
                <span>
                    图片
                </span>
            </a></li>

            <li><a class="channel-item" href="#">
                <span>
                    娱乐
                </span>
            </a></li>

            <li><a class="channel-item" href="#">
                <span>
                    游戏
                </span>
            </a></li>

            <li><a class="channel-item" href="#">
                <span>
                    体育
                </span>
            </a></li>

        </ul>

    </div>
    <div class="center">
        <ul class="news_list">
            <li>
                <div class="title-box">
                    <a href="#" class="link">
                        奥巴马罕见介入美国2020大选，警告民主党参选人须“基于现实11”
                        <hr>
                    </a>
                </div>
            </li>
            <li>
                <div class="title-box">
                    <a href="#" class="link">
                        奥巴马罕见介入美国2020大选，警告民主党参选人须“基于现实22”
                        <hr>
                    </a>
                </div>
            </li>
            <li>
                <div class="title-box">
                    <a href="#" class="link">
                        奥巴马罕见介入美国2020大选，警告民主党参选人须“基于现实33”
                        <hr>
                    </a>
                </div>
            </li>
            <li>
                <div class="title-box">
                    <a href="#" class="link">
                        奥巴马罕见介入美国2020大选，警告民主党参选人须“基于现实44”
                        <hr>
                    </a>
                </div>
            </li>
            <li>
                <div class="title-box">
                    <a href="#" class="link">
                        奥巴马罕见介入美国2020大选，警告民主党参选人须“基于现实55”
                        <hr>
                    </a>
                </div>
            </li>
            <li>
                <div class="title-box">
                    <a href="#" class="link">
                        奥巴马罕见介入美国2020大选，警告民主党参选人须“基于现实66”
                        <hr>
                    </a>
                </div>
            </li>
            <li>
                <div class="title-box">
                    <a href="#" class="link">
                        奥巴马罕见介入美国2020大选，警告民主党参选人须“基于现实77”
                        <hr>
                    </a>
                </div>
            </li>
            <li>
                <div class="title-box">
                    <a href="#" class="link">
                        奥巴马罕见介入美国2020大选，警告民主党参选人须“基于现实88”
                        <hr>
                    </a>
                </div>
            </li>
            <li>
                <div class="title-box">
                    <a href="#" class="link">
                        奥巴马罕见介入美国2020大选，警告民主党参选人须“基于现实99”
                        <hr>
                    </a>
                </div>
            </li>
            <li>
                <div class="title-box">
                    <a href="#" class="link">
                        奥巴马罕见介入美国2020大选，警告民主党参选人须“基于现实1010”
                        <hr>
                    </a>
                </div>
            </li>
        </ul>

        <div class="loading" style="text-align: center; height: 80px">
            <img src="img/loading2.gif" height="100%">
        </div>

        <div class="content">
            <div class="pagination-holder clearfix">
                <div id="light-pagination" class="pagination"></div>
            </div>
        </div>

        <div id="no" style="text-align: center;color: red;font-size: 20px"></div>
    </div>
</div>
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script>
    //1.定义发送请求标记
    let send = true;

    //2.定义当前页码和每页显示的条数
    let start = 1;
    let pageSize = 10;

    //3.定义滚动条距底部的距离
    let bottom = 1;

    //4.设置页面加载事件
    $(function () {
        //5.为当前窗口绑定滚动条滚动事件
        $(window).scroll(function () {
            //6.获取必要信息，用于计算当前展示数据是否浏览完毕
            //当前窗口的高度
            let windowHeight = $(window).height();

            //滚动条从上到下滚动距离
            let scrollTop = $(window).scrollTop();

            //当前文档的高度
            let docHeight = $(document).height();

            //7.计算当前展示数据是否浏览完毕
            //当 滚动条距底部的距离 + 当前滚动条滚动的距离 + 当前窗口的高度 >= 当前文档的高度
            if((bottom + scrollTop + windowHeight) >= docHeight) {
                //8.判断请求标记是否为true
                if(send == true) {
                    //9.将请求标记置为false，当前异步操作完成前，不能重新发起请求。
                    send = false;
                    //10.根据当前页和每页显示的条数来 请求查询分页数据
                    queryByPage(start,pageSize);
                    //11.当前页码+1
                    start++;
                }
            }
        });
    });

    //定义查询分页数据的函数
    function queryByPage(start,pageSize){
        //加载动图显示
        $(".loading").show();
        //发起AJAX请求
        $.ajax({
            //请求的资源路径
            url:"newsServlet",
            //请求的参数
            data:{"start":start,"pageSize":pageSize},
            //请求的方式
            type:"POST",
            //响应数据形式
            dataType:"json",
            //请求成功后的回调函数
            success:function (data) {
                if(data.length == 0) {
                    $(".loading").hide();
                    $("#no").html("我也是有底线的...");
                    return;
                }
                //加载动图隐藏
                $(".loading").hide();
                //将数据显示
                let titles = "";
                for(let i = 0; i < data.length; i++) {
                    titles += "<li>\n" +
                        "                <div class=\"title-box\">\n" +
                        "                    <a href=\"#\" class=\"link\">\n" +
                        data[i].title +
                        "                        <hr>\n" +
                        "                    </a>\n" +
                        "                </div>\n" +
                        "            </li>";
                }

                //显示到页面
                $(".news_list").append(titles);
                //将请求标记设置为true
                send = true;
            }
        });
    }

</script>
</html>
```

这里174行之后的代码是我们所需要的代码，具体的注释都写在上面了，自己去看吧

## 按钮分页

### 按钮分页效果和环境的介绍

简单来说就是我们不下拉就更新了，而是我们选择页面才更新，就是我们的最简单的按钮分页效果

我们这里完成分页的效果需要用到前端的分页助手，我们这里同样不再继续分析写好的代码，直接写我们要写的代码

### 按钮分页案例的实现



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE52abbda3c2f5ce48724cf86c722825c0.png)

那么我们可以构造其处理请求的代码如下

```
package com.itheima.controller;

import com.fasterxml.jackson.databind.ObjectMapper;
import com.github.pagehelper.Page;
import com.github.pagehelper.PageInfo;
import com.itheima.bean.News;
import com.itheima.service.NewsService;
import com.itheima.service.impl.NewsServiceImpl;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.List;

@WebServlet("/newsServlet2")
public class NewsServlet2 extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //设置请求和响应的编码
        req.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=UTF-8");

        //1.获取请求参数
        String start = req.getParameter("start");
        String pageSize = req.getParameter("pageSize");

        //2.根据当前页码和每页显示的条数来调用业务层的查询方法，得到分页Page对象
        NewsService service = new NewsServiceImpl();
        Page page = service.pageQuery(Integer.parseInt(start), Integer.parseInt(pageSize));

        //3.封装PageInfo对象
        PageInfo<List<News>> info = new PageInfo<>(page);

        //4.将得到的数据转为JSON
        String json = new ObjectMapper().writeValueAsString(info);

        //5.将数据响应给客户端
        resp.getWriter().write(json);

    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req,resp);
    }
}

```

然后我们构造其网页处理的代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>网站首页</title>
    <link rel="stylesheet" href="css/tt.css">
    <link rel="stylesheet" href="css/simplePagination.css">
</head>
<body>
<div class="top">
    <span class="top-left">下载APP</span>
    <span class="top-left"> 北京         晴天</span>
    <span class="top-right">更多产品</span>
</div>

<div class="container">

    <div class="left">
        <a>
            <img src="img/logo.png"><br/>
        </a>

        <ul>
            <li>
                <a class="channel-item active" href="#">
                    <span>
                        推荐
                    </span>
                </a>
            </li>

            <li><a class="channel-item" href="#">
                <span>
                    视频
                </span>
            </a></li>

            <li><a class="channel-item" href="#">
                <span>
                    热点
                </span>
            </a></li>

            <li><a class="channel-item" href="#">
                <span>
                    直播
                </span>
            </a></li>

            <li><a class="channel-item" href="#">
                <span>
                    图片
                </span>
            </a></li>

            <li><a class="channel-item" href="#">
                <span>
                    娱乐
                </span>
            </a></li>

            <li><a class="channel-item" href="#">
                <span>
                    游戏
                </span>
            </a></li>

            <li><a class="channel-item" href="#">
                <span>
                    体育
                </span>
            </a></li>
        </ul>

    </div>
    <div class="center">
        <ul class="news_list">

        </ul>

        <div class="content">
            <div class="pagination-holder clearfix">
                <div id="light-pagination" class="pagination"></div>
            </div>
        </div>

    </div>
</div>
</body>
<script src="js/jquery-3.3.1.min.js"></script>
<script src="js/jquery.simplePagination.js"></script>
<script>
    //1.定义当前页码和每页显示的条数
    let start = 1;
    let pageSize = 10;

    //2.调用查询数据的方法
    queryByPage(start,pageSize);

    //3.定义请求查询分页数据的函数，发起AJAX异步请求，将数据显示到页面
    function queryByPage(start,pageSize) {
        $.ajax({
            //请求的资源路径
            url:"newsServlet2",
            //请求的参数
            data:{"start":start,"pageSize":pageSize},
            //请求的方式
            type:"POST",
            //响应数据形式
            dataType:"json",
            //请求成功后的回调函数
            success:function (pageInfo) {
                //将数据显示到页面
                let titles = "";
                for(let i = 0; i < pageInfo.list.length; i++) {
                    titles += "<li>\n" +
                        "                <div class=\"title-box\">\n" +
                        "                    <a href=\"#\" class=\"link\">\n" +
                        pageInfo.list[i].title +
                        "                        <hr>\n" +
                        "                    </a>\n" +
                        "                </div>\n" +
                        "            </li>";
                }
                $(".news_list").html(titles);

                //4.为分页按钮区域设置页数参数（总页数和当前页）
                $("#light-pagination").pagination({
                    pages:pageInfo.pages,
                    currentPage:pageInfo.pageNum
                });

                //5.为分页按钮绑定单击事件,完成上一页下一页查询功能
                $("#light-pagination .page-link").click(function () {
                    //获取点击按钮的文本内容
                    let page = $(this).html();
                    //如果点击的是Prev，调用查询方法，查询当前页的上一页数据
                    if(page == "Prev") {
                        queryByPage(pageInfo.pageNum - 1,pageSize);
                    }else if (page == "Next") {
                        //如果点击的是Next，调用查询方法，查询当前页的下一页数据
                        queryByPage(pageInfo.pageNum + 1,pageSize);
                    } else {
                        //调用查询方法，查询当前页的数据
                        queryByPage(page,pageSize);
                    }
                });
            }
        });
    }

</script>
</html>
```

这一节的内容我嫌烦我干脆是跳跃了的，直接放出了结论，复习的时候看不懂也没关系，反正一开始也没怎么认真看，瀑布流的倒是还算认真看过



