- ServletConfig的介绍

首先ServletConfig是Servlet的配置参数对象，在Servlet的规范中，允许每一个Servlet都提供一些初始化的配置，因此每一个Servlet都有自己的一个ServletConfig。

其作用是在Servlet初始化时将一些配置信息传给Servlet，其传递的数据的方式是采用键值对的形式，而且其生命周期和Servlet相同

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE065ff536d8a55495677bb11e64da69b0.png)

- ServletConfig的配置方式

ServletConfig的配置方式是通过<init-param>标签类配置，其下有两个字标签，分别用于初始化参数的key和value。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE986bb383e2660cb99d3216f1793e6145.png)

假设我们创建一个ServletConfigDemo类并继承Http类，接着实现其doGet和dopost方法，那么接着我们想要配置ServletConfig的话就要到对应的配置文件中去配置，我们可以在配置文件中写入配置代码如下

```
<!--配置Servlet-->
<servlet>
    <servlet-name>servletConfigDemo</servlet-name>
    <servlet-class>com.itheima.servlet.ServletDemo</servlet-class>
    <!--配置ServletConfig-->
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
        <param-name>desc</param-name>
        <param-value>This is ServletConfig</param-value>
    </init-param>
</servlet>
<servlet-mapping>
    <servlet-name>servletConfigDemo</servlet-name>
    <url-pattern>/servletConfigDemo</url-pattern>
</servlet-mapping>
```

这里我们配置了两个ServletConfig， 需要注意的是如果我们要配置多个ServletConfig，那么我们就要配置写入多个</init-param>标签，并在标签中配置对应信息，其中<param-name>是参数名称，<param-value>是参数的值。还有就是ServletConfig是给Servlet提供配置信息的，因此我们的配置信息必须要写在<servlet>标签中

- ServletConfig的常用方法

ServletConfig的作用与他内部的方法息息相关，因此本章我们来学习下ServletConfig的常用方法，请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE3940fbfd22397c1bd6b879c3341b7037.png)

上面这四个方法里，第一个getInitParameter是可以根据参数的名称来获得参数的值。第二个getInitParameterNames方法可以获取所有参数名称并以枚举类型返回，我们可以将这个方法与第一个方法结合来获得ServletConfig中的所有参数的名字和其值。而getServletName方法就是获取Servlet的名字，最后一个getServletContext可以获取ServletContext对象，但是这个内容我们还没学习到，因此我们这里只做简单的了解

我们都知道Servlet对象创建时必然会调用doget或者dopost方法，因此我们可以在这两个方法里做文章，来测试ServletConfig的常用方法，但是测试其常用方法之前，我们必须处理一个问题，那就是我们必须获得ServletConfig对象，我们知道Servlet方法是必然要调用init方法来初始化的，我们就可以通过这个方法来获得我们的init对象

```
package com.itheima.servlet;

import javax.servlet.ServletConfig;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Enumeration;

/*
    ServletConfig的演示
 */
public class ServletDemo extends HttpServlet {

    //1.声明ServletConfig
    private ServletConfig config;

    //2.通过init方法，来对ServletConfig对象进行赋值
    @Override
    public void init(ServletConfig config) throws ServletException {
        this.config = config;
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //3.演示ServletConfig常用方法
        //根据key获取value
        String encodingValue = config.getInitParameter("encoding");
        System.out.println(encodingValue);

        //获取所有的key
        Enumeration<String> keys = config.getInitParameterNames();
        while (keys.hasMoreElements()){
            //获取每一个key
            String key = keys.nextElement();
            //根据key获取value
            String value = config.getInitParameter(key);

            System.out.println(key+","+value);
        }

        //获取Servlet的名称
        String servletName = config.getServletName();
        System.out.println(servletName);

        //获取ServletContext对象
        ServletContext servletContext = config.getServletContext();
        System.out.println(servletContext);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    }
}

```

最后我们通过测试也能够在控制台上获得我们想要的结果

