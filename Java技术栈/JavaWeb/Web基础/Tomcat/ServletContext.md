在上一个章节里我们提到了ServletContext，这一章节我们来正式学习这个内容

- ServletContext的介绍

首先ServletContext是应用上下文对象，也被称为应用域对象，每一个应用中只有一个ServletContext对象。其可以配置和获得应用的全局初始化参数，还可以实现多个Servlet之间的数据共享。当应用加载时其被创建，当应用停止时其被销毁，这也是它的生命周期。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf2ddb4e2d14a674a6dd9f5306df26d03.png)

根据上图我们能看到三个Servlet中要共享数据时就是通过ServletContext进行共享的，这里我们称其为ServletContext域对象，那么什么是域对象呢？域对象其实指的是对象有作用域，也就是有作用范围。域对象可以实现数据的共享同时不同作用范围的域对象其共享数据的能力也不一样。在Servlet规范中一共有四个域对象，ServletContext就是其中的一个，其是web应用中最大的作用域，也叫application域，其可以实现整个应用之间的数据共享

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd201409b796bd1b76934db35f9e253c3.png)

注意ServletContext最重要的两个作用还是可以实现数据的共享和配置和获得应用的全局初始化参数，下面的内容我们主要是演示ServletContext的这两个功能

- ServletContext的配置方式

其实ServletContext的配置方式和我们之前学过的ServletConfig的配置方式是十分相似的，他们都是采用键值对的方式来传递数据，所以其标签下都有两个对应的字标签。不过需要注意的是，由于ServletContext是在全局下的， 因此其需要配置在<web-app>的标签中，而不是在Servlet标签中

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEac95c17353fc1301fad592cd74b572a3.png)

那么现在我们来正式配置ServletContext，首先我们创造一个ServletContextDemo类继承Http重写对应的两个方法

```
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class ServletContextDemo extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}
```

然后我们可以配置对应的配置信息如下

```
<!--配置Servlet-->
<servlet>
    <servlet-name>servletContextDemo</servlet-name>
    <servlet-class>com.itheima.servlet.ServletDemo</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>servletContextDemo</servlet-name>
    <url-pattern>/servletContextDemo</url-pattern>
</servlet-mapping>

<!--配置ServletContext-->
<context-param>
    <param-name>globalEncoding</param-name>
    <param-value>UTF-8</param-value>
</context-param>
<context-param>
    <param-name>globalDesc</param-name>
    <param-value>This is ServletContext</param-value>
</context-param>
```

可以看到我们这里先配置好了Servlet，然后我们再配置ServletContext，与ServletConfig不同的是，后者是放在Servlet标签下的，因为其是给对应的Servlet提供对应的配置信息的，因此当然要在对应的Servlet标签下。而前者则是放置于全局标签下的，因为其实对应于全局的Servlet而言的，而如果我们想要配置多个信息，同样也是要重新构造新的标签，而不可以在标签内重复定义。

- ServletContext的常用方法

再学习完了如何配置ServletContext之后，我们之后当然就是要学习如何使用ServletContext，因此本节我们来学习ServletContext的常用方法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8c483caebebf48fde3ff748b623c8264.png)

首先是getInitParameter方法，该方法需要传入一个全局配置参数的名称，其会返回全局配置的参数的值，然后是getContextPath方法，其可以获取当前应用的访问的虚拟目录，最后是getRealPath方法，该方法可以根据虚拟目录获取应用部署的磁盘的绝对路径。第二个第三个方法常常是配合使用的，通过这两个方法我们可以通过虚拟目录来获取我们需要的文件的绝对路径

下面我们就来演示下这三个方法，不过再演示这三个方法之前，我们得先解决几个问题，首先是，我们怎么获得ServletContext对象呢？我们之前说过，我们可以通过ServletConfig对象来获取ServletContext对象，但是这个似乎有些麻烦了，有没有更简单的方法呢？当然有，我们只要看看HttpServlet的源码就能发现了

首先在HttpServlet下的源码里，我们可以知道这个类继承了GenericServlet，在这个类我们能看到其有ServletConfig对象的成员变量

```
private transient ServletConfig config;
```

我们都知道，成员变量一般是初始化的时候进行的赋值的，那么我们往下查找能看到其初始化的ServletConfig的方法

```
public void init(ServletConfig config) throws ServletException {
    this.config = config;
    this.init();
}
```

然后我们继续看上面的方法，能找到这个方法

```
public ServletContext getServletContext() {
    return this.getServletConfig().getServletContext();
}
```

我们发现这个方法就是直接调用了ServletConfig对象然后返回一个ServletContext对象的，这不正好就是我们所需要的吗？而且由于其是继承关系，因此父类的方法子类也有，那么我们就不必这么大费周章了，直接调用这个方法就完了

我们这里还有科普一个内容，就是关于虚拟路径和绝对路径的分别。我们先假设我们分别在src、web、WEB-INF下分别创建了a.txt，b.txt，c.txt。虚拟目录我们都知道是什么，其实就是我们当初点击run，然后点击edit configruation里，我们给我们的项目设置的虚拟路径，其并不是实际存在的路径，只是我们Tomcat为了区分不同的项目所需要用的路径，实际上我们如果想要向我们的服务器发起请求，写入正确的虚拟路径也是十分必要的，不写的话Tomcat根本找不到。但是有时候，这些虚拟路径不是我们所需要的，因为只有Tomcat能够认识到，而其他时候我们则需要绝对路径，绝对路径是我们的项目发布的路径，我们使用第三种方法获取的路径就是项目发布的war包所在的路径，而不是其存在的路径。

有的同学的可能会觉得发布路径和存在路径不应该是一样的吗？其实是不一样的。我们在idea里直接查看的文件路径是文件的存在路径，但实际上，文件发布的路径其实是在文件的存在路径的上一级路径的out包下的artifacts包下的对应的war包下的，在里面我们也能够查看到和我们的项目几乎一模一样的路径布置，不过不同的是，创建在src下的文件，最终会被放到WEB-INF包下的classes包下

我们在idea中，我们总是能够轻易获得其虚拟路径，但是如果只有虚拟路径不方便，结合第二三个方法，我们能够轻易获得我们项目的文件发布的绝对路径，这就是第二三个方法的用法。

那么最终我们可以构造代码如下

```
package com.itheima.servlet;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class ServletContextDemo extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.获取ServletContext对象
        ServletContext context = getServletContext();

        //2.常用方法的演示
        //获取全局配置参数：getInitParameter(String key)  根据key获取value
        String value = context.getInitParameter("globalDesc");
        System.out.println(value);

        //获取应用的虚拟目录：getContextPath()
        String contextPath = context.getContextPath();
        System.out.println(contextPath);

        //根据虚拟目录获取绝对路径：getRealPath(String path)
        String realPath = context.getRealPath("/");
        System.out.println(realPath);

        String b = context.getRealPath("/b.txt");
        System.out.println(b);

        String c = context.getRealPath("/WEB-INF/c.txt");
        System.out.println(c);

        String a = context.getRealPath("/WEB-INF/classes/a.txt");
        System.out.println(a);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}

```

实际测试，也会发现这些方法都正确打印出了我们的文件的正确路径，就说明我们的代码和方法测试都没有问题

接下来我们来讲讲另外一个用于共享数据的重要方法，请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6ebe1bc8370a06771e808f0f96da6252.png)

这里setAttribute可以向应用域对象中存储数据，是采用键值对的方式。然后getAttribute则可以通过名称获取应用域中对象的数据，最后是removeAttribute则是可以通过名称移除应用域对象中的数据

我们来写入代码测试下这三个方法，我们现在ServletContextDemo类中写入如下代码

```
//设置共享数据
context.setAttribute("username","zhangsan");

//删除共享数据
context.removeAttribute("username");
```

然后我们在另一个类中写入如下代码

```
//获取共享数据
Object value = getServletContext().getAttribute("username");
System.out.println(value);
```

然后我们启动Tomcat，先请求前者后请求后者，由于前者设置了数据之后又删除了，因此后者得到的结果是null。实际测试也的确是如此