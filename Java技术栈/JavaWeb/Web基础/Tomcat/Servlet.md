那么现在我们来正式学习Servlet，之前我们只是做过简单的学习，这一章节，我们要来正式学习Servlet，首先自然是对Servlet进行介绍。我们这里提供了JavaEE的帮助文档，我们直接翻阅帮助文档吧

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEbcc42530a9797b83a5f323475edf05bb.png)

可以看到都是英文哈，稍微有那么点痛苦，但无所谓，让痛重叠。

首先Servlet是一个接口，其是处于javax.servlet包下的，其下有两个子接口，分别是HttpJspPage和JspPage。我们要知道这里有两个主要实现类，分别是GenericServlet,HttpServlet类

接着分割线下面的内容是对于这个接口的说明，首先所有的Servlet类都必须实现这个接口。其次是Sevlet是运行在我们的Web服务器上的一个小程序，其核心作用是用于接受和响应我们的Web客户端的基于HTTP协议的请求

如果要实现Servlet功能，除了实现Servlet接口之外，我们还有其他两种方式，分别是继承javax.servlet.GenericServlet或者是继承javax.servlet.http.HttpServlet。总的来说，我们要实现Servlet功能有三种方式

在Servlet接口中我们定义了一些相关的功能，这里面包括一个核心的功能就是service方法，以及我们的初始化和销毁的方法。其实这些方法就介绍了我们的Servlet的一个生命周期，包括生成，调用以及销毁

在下面是一个优先级关系的一个说明，首先Servlet方法初始化时会调用init方法。其次是所有的请求都会通过service方法来进行处理。最后是当servlet从服务器中移除，也就是销毁时，会调用destroyed方法，最终会由垃圾回收器进行回收

最后这个接口还提供了getServletConfig方法，调用该方法可以返回该Servlet的配置信息。还有getServletInfo方法，该方法可以返回Servlet的基本信息，如作者和版权

以上就是本节的所有内容，最后我们来看看中英翻译版本的帮助文档

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE7570451e0abee3dce323b0236f8a424e.png)

最后我们可以做一个本章总结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE33b8d596b72b6830edb0df74269103cc.png)

- Servlet快速入门——继承GenericSevlet

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf0694bca71ee1eb12b7a7d6b813d7e41.png)

如同我们之前采用实现Servlet接口的方法来了解一样，这一次我们同样采用这种方式来进一步加深我们对Servlet的理解，只不过这一次换成继承GenericServlet的方式。在继承之前我们要先来看看GenericServlet的说明

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE939b2b55c70187437a4cc643b764823c.png)

可以看到其是一个抽象类，我们查看其方法会发现其方法特别多，不过其抽象方法只有一个，因此我们如果采用这种方式来实现Servlet，只需要重写其一个抽象方法就可以了，就像这样

```
package com.itheima.servlet;

import javax.servlet.*;
import java.io.IOException;

/*
    Servlet快速入门1
 */
public class ServletDemo01 extends GenericServlet {

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("Servlet方法执行了...");
    }
}
```

我们可以看到这个重写的service里有两个参数，这两个参数分别是ServletRequest和ServletResponse，分别是请求和响应，我们以后要实现service里响应客户端的请求，就要通过这两个对象，也就是请求和响应这两个对象来实现

同样我们也要更改我们的web.xml文件，令其关联对应的映射，写入如下关联代码

```
    <!--Servlet快速入门1的配置-->
    <servlet>
        <servlet-name>servletDemo01</servlet-name>
        <servlet-class>com.itheima.servlet.ServletDemo01</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>servletDemo01</servlet-name>
        <url-pattern>/servletDemo01</url-pattern>
    </servlet-mapping>
</web-app>
```

最后我们经过测试我们会发现每次刷新，也就是客户端每次发送请求时，service方法都会执行一次，那么我们的实现就没问题了

- Servlet执行过程

之前我们也讲过Servlet的执行过程，但其实那只是一个非常简易的执行过程，其实其实际的执行过程要更为复杂。首先，在讲解之前我们要先知道我们的全地址为localhost:8080/demo1/servletDemo1

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe198f6055994518eaa966fa279316d8e.png)

其执行过程一共分为五部分，我们的客户端浏览器，接着是Tomcat服务器，然后是servlet_demo1，也就是我们自己创建的项目，接着是web.xml，也就是我们的项目的配置文件，最后是ServletDemo01，这是我们自己编写的功能类

其执行过程的第一步是由客户端浏览器向Tomcat浏览器发起请求，当Tomcat接收到这个请求之后，Tomcat会解析这个请求地址，也就是URL，其地址是我们在浏览器中输入的demo1的位置的地址。第三步Tomcat会根据其对应的地址来找到对应的应用。当初我们在Tomcat配置文件里写入的虚拟目录是/demo1并绑定了一个war包，那么当这个地址传入时其就会根据这个地址找到这个war包的对应内容并打开

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6ac719e7b51f0f9dcbb2b39929a3c694.png)

第四步是找到web.xml的项目的配置文件，找到之后会解析请求的资源地址（URI），什么是请求的资源地址呢？其实就是我们在浏览器里写入的servletDemo01，之后发生的过程就正如我们上一节叙述的那样，通过我们在配置文件里写好的映射找到其对应的实现类，然后执行service方法了，这个方法会相应给客户端浏览器。当然我们这里的客户端浏览器什么都不显示，但其实是响应了的，因为我们的控制台里输出了对应的语句，而且如果地址不对，那么浏览器也会报错，既然没有报错也就说明一切ok

- Servlet关系视图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc3f5966eae75450af049cf7f2467cf27.png)

Servlet接口自己关联两个接口，这两个接口分别是ServletConfig和ServletContext。Servlet接口被GenericServlet抽象类部分实现，同时他们都实现了ServletRequest和ServletResponse接口，而HttpServlet抽象类则是继承GenericServlet抽象类的抽象类，其有两个接口，分别是HttpServletRequest和HttpServletResponse接口。最后这个三个类都是有service方法的，无非是传入的参数有所不同罢了

- Servlet实现方式之继承HttpServlet

Servlet实现的方式一共有三种，我们此前已经实现了前面两种，现在我们来实现第三种，继承HttpServlet抽象类的方式来实现Servlet

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE35f95f92d28ef2f538679130721832d5.png)

我们先来看看步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE7aed0dcf0e1335777baf3e9d65a15b61.png)

这个其实和我们之前做过的事差不多了，我们就不多提了，我直接进行一个类的写。但是当我们写完类继承了HttpServlet之后发现他还就那个不需要重写任何方法，而且步骤里也只要求我们重写doGet和doPost方法，为啥就不用重写Servlet方法呢？不是说所有的请求响应都要经过Servlet方法吗？别急，我们来看看HttpServlet类的service方法

```
public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
    HttpServletRequest request;
    HttpServletResponse response;
    try {
        request = (HttpServletRequest)req;
        response = (HttpServletResponse)res;
    } catch (ClassCastException var6) {
        throw new ServletException("non-HTTP request or response");
    }

    this.service(request, response);
}
```

可以看到这个方法先将ServletResponse和ServletRequest两个对象进行强制类型转换，然后调用另外一个重载的service方法，我们继续看这个重载的service方法

```
protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    String method = req.getMethod();
    long lastModified;
    if (method.equals("GET")) {
        lastModified = this.getLastModified(req);
        if (lastModified == -1L) {
            this.doGet(req, resp);
        } else {
            long ifModifiedSince;
            try {
                ifModifiedSince = req.getDateHeader("If-Modified-Since");
            } catch (IllegalArgumentException var9) {
                ifModifiedSince = -1L;
            }

            if (ifModifiedSince < lastModified / 1000L * 1000L) {
                this.maybeSetLastModified(resp, lastModified);
                this.doGet(req, resp);
            } else {
                resp.setStatus(304);
            }
        }
    } else if (method.equals("HEAD")) {
        lastModified = this.getLastModified(req);
        this.maybeSetLastModified(resp, lastModified);
        this.doHead(req, resp);
    } else if (method.equals("POST")) {
        this.doPost(req, resp);
    } else if (method.equals("PUT")) {
        this.doPut(req, resp);
    } else if (method.equals("DELETE")) {
        this.doDelete(req, resp);
    } else if (method.equals("OPTIONS")) {
        this.doOptions(req, resp);
    } else if (method.equals("TRACE")) {
        this.doTrace(req, resp);
    } else {
        String errMsg = lStrings.getString("http.method_not_implemented");
        Object[] errArgs = new Object[]{method};
        errMsg = MessageFormat.format(errMsg, errArgs);
        resp.sendError(501, errMsg);
    }

}
```

可以看到这个重载的方法先判断我们的的请求方式是什么方式，如果是GET的方式就调用doGet方法，如果是POST就调用doPost方法，还有其他很多的请求方式，但那些不是重点我们就不做讲解了。

从上我们可以知道其实自动调用service方法的，而service方法会调用doGet或doPost方法，因此我们只要重写这两个方法就可以了。那么我们可以创建一个ServletDemo02类继承HttpServlet并将方法重写如下

```
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/*
    Servlet快速入门2
 */
public class ServletDemo02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("方法执行了");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}

```

然后配置相应的配置文件

```
<!--Servlet快速入门2的配置-->
<servlet>
    <servlet-name>servletDemo02</servlet-name>
    <servlet-class>com.itheima.servlet.ServletDemo02</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>servletDemo02</servlet-name>
    <url-pattern>/servletDemo02</url-pattern>
</servlet-mapping>
```

最后我们经过测试会发现的确没有什么问题，这说明我们用这个方法创建Servlet对象已经成功了

最后我们要记住，第三种方式，这就是这个方式，是我们实现Servlet的最常用的方式

- Servlet生命周期

Servlet存在生命周期，所谓生命周期则是对象创建到销毁的过程。当请求第一次到达Servlet时，对象就会创建出来并初始化成功，然后给Servlet对象就会放置于内存中。服务器提供服务的整个过程中，该对象一直存在并且每次都是执行service方法，而当服务停止或者服务器宕机时，该对象会调用销毁方法，调用该方法会使对象死亡。我们可以看到Servlet对象只会创建一次，销毁一次。每次只有一个实例，且是应用中的唯一存在，我们称这种模式为单例模式

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE49d439d9c4a0b9b95c0c40248ba61c61.png)

我们如果要测试这个生命周期也很简单，我们已知Servlet对象创建会调用init方法，死亡会调用destroy方法，那么我们可以创建一个ServletDemo03类继承HttpServlet并将方法重写如下

```
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/*
    Servlet快速入门3
 */
public class ServletDemo03 extends HttpServlet {

    //对象出生的过程
    @Override
    public void init() throws ServletException {
        System.out.println("Servlet has born");
    }

    //对象服务的过程
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("jie shou dao le ke hu duan de qing qiu");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }

    //对象销毁的过程
    @Override
    public void destroy() {
        System.out.println("Servlet has destroy");
    }
}

```

接着配置相应的文件

```
<!--Servlet快速入门3的配置-->
<servlet>
    <servlet-name>servletDemo03</servlet-name>
    <servlet-class>com.itheima.servlet.ServletDemo03</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>servletDemo03</servlet-name>
    <url-pattern>/servletDemo03</url-pattern>
</servlet-mapping>
```

然后我们经过测试会发现的确也没有什么问题，说明的确其生命周期就与我们想得是一样的

- Servlet线程安全问题

前面我们说过Servlet是单例模式的，那么其实线程安全的吗？为了测试其是否是线程安全的，我们要来构造一个新的ServletDemo04并写入代码如下

```
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

/*
    Servlet线程安全的问题
 */
public class ServletDemo04 extends HttpServlet {
    //1.定义一个用户名的成员变量
    private String username;

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //2.获取用户名
        username = req.getParameter("username");

        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        //3.将用户名响应给浏览器
        PrintWriter pw = resp.getWriter();
        pw.println("welcome"+username);
        pw.close();
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}

```

上面的代码里我们写入了一个成员变量，然后获取其用户名，然后将该用户名打印在浏览器中。至于这个功能是怎么实现的，里面的方法到底是个什么原理，这个暂时不需要我们管

接着我们写入配置信息

```
<!--Servlet线程安全-->
<servlet>
    <servlet-name>servletDemo04</servlet-name>
    <servlet-class>com.itheima.servlet.ServletDemo04</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>servletDemo04</servlet-name>
    <url-pattern>/servletDemo04</url-pattern>
</servlet-mapping>
```

搞定了之后，启动tomcat，我们开启两个浏览器，分别填上http://localhost:8080/crm2/servletDemo04?username=aaa和http://localhost:8080/crm2/servletDemo04?username=bbb

我们期望的是其会在两个不同的浏览器上打印出aaa和bbb，但实际的结果，却是两个都是bbb。之所以会这样，是因为当我们的谷歌浏览器进入到我们的程序时，username的值被改为aaa，之后其进入休眠，然后火狐进入将其改为bbb，然后两个获取到的username就都是bbb了。从这个例子里我们也能够知道，tomcat是线程不安全的

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5eb08a52a5d4365e7b7e2103996981bd.png)

那么我们要怎么解决这个线程不安全的问题呢？一个简单的想法当然就是加入线程同步关键字，但是我们还有其他效率更高的想法，比如说我们可以将变量定义为局部变量而非成员变量，又或者是我们判断进入的线程是否会修改值，如果不会那么我们就没必要执行线程同步，会的话我们再执行线程同步

先来说说第一种将变量定义为局部的变量的方法，这个方法的内容很简单，将username定义在方法内就可以了

而使用synchronized关键字实现线程安全的方法也很简单，我们可以将代码改造如下

```
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

/*
    Servlet线程安全的问题
 */
public class ServletDemo04 extends HttpServlet {
    //1.定义一个用户名的成员变量
    private String username;

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        synchronized (this){
            //2.获取用户名
            username = req.getParameter("username");

            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            //3.将用户名响应给浏览器
            PrintWriter pw = resp.getWriter();
            pw.println("welcome"+username);
            pw.close();
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}

```

我们将所有可能修改内容的代码都加入线程同步中，然后锁对象就用这一个类的对象，其正好是唯一的

- Servlet不同映射方式

我们的Servlet其实一共有三种映射方式，请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEff6b0048a11f5860972e1f95397ba2a0.png)

我们之前一直都在使用第一种映射方式，接下来我们来尝试下后两种映射方式，先来第一种吧

```
<servlet>
    <servlet-name>servletDemo04</servlet-name>
    <servlet-class>com.itheima.servlet.ServletDemo04</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>servletDemo04</servlet-name>
    <url-pattern>/servletDemo04</url-pattern>
</servlet-mapping>
```

第一种映射方式其访问资源的路径必须和映射配置完全相同，其最精确同时优先级也最高，这也意为着我们如果使用这种方式来访问我们对应的功能类，那么我们必须要输入完整的类名路径，比方说我们这里想要访问servletDemo04，那么我们就要输入http://localhost:8080/crm2/servletDemo04，其中http://localhost:8080/crm2的路径是我们事前设置的虚拟路径，无论是在何种映射方式下都是不可缺少不可省略的

接下来我们来讲讲第二种映射方式，请看代码

```
<servlet>
    <servlet-name>servletDemo04</servlet-name>
    <servlet-class>com.itheima.servlet.ServletDemo04</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>servletDemo04</servlet-name>
    <url-pattern>/servlet/*</url-pattern>
</servlet-mapping>
```

有进行修改的只有第七行，我们将后面的改为*，*代表通配符，而前面的servlet则是我们自己定义的开头。这种方式的精确性不高，但是普适性好，采用这种映射方式只要用户前面有输入http://localhost:8080/crm2/servlet/这个地址，后面的地址随便写什么都是可以运行的，甚至可以不写

接着我们来讲第三种映射方式，请看代码

```
<!--Servlet线程安全-->
<servlet>
    <servlet-name>servletDemo04</servlet-name>
    <servlet-class>com.itheima.servlet.ServletDemo04</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>servletDemo04</servlet-name>
    <url-pattern>*.do</url-pattern>
</servlet-mapping>
```

这种方式的普适性最高，精确性最差，同时优先级也最低，同时使用这种方式不需要在映射前加入/，还算比较方便

- Servlet多路径映射

那么我们刚刚学习的映射在具体场景上有什么用呢？我们本章就来解决这个问题，请看下图所需的需求

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE02af79040c9f213e6812a89a76d36f44.png)

那么一个简单的想法就是使用第二种映射方式来实现这个需求，我们可以让服务器每次响应客户端的请求时判断最后截取的字符是vip还是vvip，通过这个来给商品进行对应的打折。那么我们首先可以构造该类的代码如下

```
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class ServletDemo06 extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.定义一个商品金额
        int money = 1000;
        
        //2.获取访问资源路径
        String path = req.getRequestURI();
        path = path.substring(path.lastIndexOf("/"));
        
        //3.条件判断
        if("/vip".equals(path)){
            System.out.println("商品原价为："+money+"优惠后是："+(money*0.9));
        }else if("/vvip".equals(path)){
            System.out.println("商品原价为："+money+"优惠后是："+(money*0.5));
        }else {
            System.out.println("商品原价为："+money);
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}

```

设置配置文件代码如下

```
<!--Servlet多映射的配置-->
<servlet>
    <servlet-name>servletDemo06</servlet-name>
    <servlet-class>com.itheima.servlet.ServletDemo06</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>servletDemo06</servlet-name>
    <url-pattern>/itheima/*</url-pattern>
</servlet-mapping>
```

- Servlet的创建时机

Servlet的创建时机其实有两个，一个是我们的之前使用的当服务器接收到客户端的请求的时候创建，第二种方式是在服务器加载好的时候就创建。前者能够减少内存浪费提供服务器启动效率，其弊端是如果要在加载时做初始化动作，那么是无法完成的。而后者是提前创建好对象，可以提高首次执行效率，也可以完成一些应用加载时就需要做的初始化操作，而其弊端是对服务器的内存占用会比较多，而且会影响服务器的启动效率

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE4be32eee4855f4ab9e62f667a0075a70.png)

如果我们想要修改Servlet的创建时机，可以在配置文件的<servlet>标间中添加<load-on-startup>标签，这个标签中间只能写数字，数字越小则代表优先级越高，如果写入负整数或者不写则默认代表是第一次访问时被创建。我们之前是没有写的，所以其默认是第一次访问时被创建

这个时候就有一个问题，我们的Servlet对象不是只有一个的吗？为什么还有优先级的说法呢？这是因为后面我们可能会创建多个Servlet的，如果他们都要在启动时被创建出来，那么我们就应该给其分配优先级

- 默认Servlet

Tomcat的conf目录中的web.xml中配置了一个默认的Servlet，供给与用户使用

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE1e72c1513f2f287b9136331b27be4867.png)

其映射路径是/，简单来说就是当用户发送请求时，会优先寻找用户配置的Servlet，如果找不到，那么就去寻找默认的Tomcat，这个默认的Tomcat会将没有找到对应的网页的信息返回给用户。这个知识只做了解