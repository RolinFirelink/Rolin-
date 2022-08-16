- 过滤器的介绍

其实Filter就是过滤器，那么什么是过滤器呢？过滤器又有什么作用呢？请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEdd5b4cb59c13fdd310f4b44af4547bce.png)

简单来说，就是客户端请求响应之前要先经过Filter过滤器了，更加具体的作用请看上图的解释

- Filter介绍

简单来说Filter是一个接口，其下有三种方法，分别是用于初始化、销毁和实现过滤的方法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6a8030488d6f29eb0b673ebe8b331246.png)

Filter的使用还需要配置，其有两种配置方式，一种是配置文件，另一种是注解

更加详细的解释请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb7280a7ba20487cd5c8117ef9d4e7b80.png)

再来看看其下方法的说明

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2e9b33eab0a1015de167d7ce4949f386.png)

- FilterChain的介绍

FilterChain是一个接口，其代表过滤器链对象。Servlet容器会自动提供实现类对象，因此我们直接使用就可以了。

过滤器可以定义多个，其会组成过滤器链，过滤器链FfilterChain本身作为参数要传入到Filter对象里的doFilter中，然而其自身也有一个doFilter方法，这个方法的作用是放行，简单来说就是我们的过滤器拦截了请求之后，还要将请求放出去，传给下一个过滤器或者是Servlet（如果有下一个过滤器的话），这个动作就叫做放行。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE46f4c2006d264dd29679f9eae09a3b0e.png)

关于FilterChain更加具体的解释请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8a2cf4af64aa39cc4cac3b131c3d4a81.png)

- Filter过滤器的使用

本节我们通过实现一个需求来学会如何使用Filter过滤器，请先来看看步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE216006ca1e98644ea60754fe602c797c.png)

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

- Filter过滤器的使用细节

首先我们的Filter过滤器有两种配置方式，注解方式我们前面已经演示过了，下图里演示了用配置文件进行配置的方式

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE3339bbde470d87b9379479763ca2dce0.png)

如果我们有多个过滤器的话，那么过滤器的先后取决于过滤器映射的先后的顺序（所谓映射，其实就是filter-mapping标签），请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE508ae9940b7b8fc2f526980b4645cff9.png)

我们这里01先映射，02后映射，因此我们的过滤器自然也是01先拦截，02后拦截

- Filter过滤器的生命周期

Filter的声明周期，其执行init初始化方法时被创建，其服务时执行doFilter方法，其销毁时执行destroy方法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8cbb3e1040c6b24f49dc1dccd76d8ca8.png)

测试代码就不构建了，这个知道就可以了

- FilterConfig过滤器配置对象的使用

我们先要解决一个问题，就是什么是FilterConfig，FilterConfig我们已经知道是在我们的Filter对象的init方法里所需要的参数了，其本身是一个接口，同样不需要我们去提供实现类，服务器会给我们提供。而FilterConfig是可以帮助我们去获取过滤器所初始化的一些参数的，至于初始化的参数应该如何配置，这个以后再说，我们现在假设我们的过滤器已经将所有参数都配置好了，只等待我们去用FilterConfig去获取。其下有四个用于获取过滤器参数的方法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE4c56ff5b467f6c5dfcb2367989063677.png)

我们可以在init方法里调用我们的FilterConfig的方法来获取过滤器的配置信息，我们可以构造测试代码如下

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEfd78617f6eea689a6c372efef7e5708a.png)

接着我们给予我们的过滤器一些配置

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEfbcef912fb5396bbd43e2cd414948b1d.png)

然后我们就可以获取到我们想要的结果了，我们的确获得了过滤器的名称，还获得了我们最开始配置的信息，利用username获得了zhangsan这个数据

关于FilterConfig方法的进一步解释请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa927ba8a1efe0227ef18be70d56f9a32.png)

- Filter过滤器五种拦截行为

Filter过滤器默认拦截直接向某个功能类的请求，但是在实际开发中，我们还有请求转发、请求包含、以及由服务器出发调用的全局错误界面，这些情况下我们的过滤器是不参与过滤的，如果我们希望在这些情况下过滤器仍然参与过滤，就需要我们进行相应的配置

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEfb6633b7d5ace95d70c35fe1e908c816.png)

配置的方式其实都已经写在图上面了，这里就不再赘述了。这里值得一提的是全局的错误页面，什么是全局错误页面，其又应该要怎么配置？请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8cc62c54e090e5a13455369e3bf1cdf9.png)

可以看到全局错误页面的配置就如上所示，我们先配置了两种情况，一种是出现任何的异常，另一种是出现了404的错误码，这两种情况我们都进行了配置，一旦页面出现了这种异常的任何一种，我们就令其跳转到我们指定的界面

接着我们不妨来看看我们的测试代码，加强一下我们对请求转发和请求包含的理解

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6ef187d2a9485744a85873aa7e8774b1.png)

最后我们来看看我们的配置文件里对我们的过滤器的配置信息，其表示在任何情况下我们的过滤器都会进行拦截，由于异步线程我们还没有学习，因此这里就不提了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf498a4b32e84f394ec60458b3c7e2782.png)

最后值得一提的是，第一个REQUEST是默认值，其代表任何直接指向功能类的请求会被对应的过滤器所拦截