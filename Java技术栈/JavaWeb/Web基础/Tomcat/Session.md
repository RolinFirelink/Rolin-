学习完了Cookie之后，我们现在来学习Session，其也是一个会话技术。现在介绍下什么是Session吧

- HttpSession的介绍

首先HttpSession是服务器端的会话管理技术，我们之前学习的Cookie是客户端的会话管理技术。不过其本质也是采用客户端会话管理技术，只不过在客户端里保存的是一个特殊标识，共享的数据保存到服务器端的内存对象中。每次请求时就会将特殊标识带到服务器端并通过这个标识找到对应的内存空间，从而实现数据共享

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE3510d37e46e4dde5ced1952fae94f32c.png)

其也是Servlet规范中四大域对象之一的会话域对象，可以在当前会话范围之后实现数据共享。其本身也是一个接口，但是Tomcat会为我们提供实现类，我们直接用就可以了，且在其内部里也有很多我们以前学习过的对应方法，我们后续会对其进行讲解

- HttpSession的常用方法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa15fcdcac820aa364064ca8ed0198843.png)

上面的方法都很简单，而且很多我们都已经用过了，这里就不单独去演示了，就这样吧。

- HttpSession的获取

我们要获取到HttpSession，自然要了解其获取方法，我们不妨来看看

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE00eed9c66f4833f318308ca225662576.png)

这里有两个方法，第一个getSession方法的作用就是用于获取HttpSession对象，第二个getSession的重载方法是可以传入一个布尔类型的值，确定如果我们没有获取到对应的HttpSession对象时，要不要自动创建。

这里值得一提的是，第一个方法的默认值是为true的，也就是说，如果不特殊指定，那么其如果没有获取到对应的对象，是会自动创建一个对象的

要了解第二个方法的内部机制，我们需要来看流程图，请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf8c653a2f00f2bd67cf8aa1e78bb5bb6.png)

我们先从客户端中调用getSession方法获得其唯一标识，先寻找是否携带有JSESSIONID的值，如果有，即根据这个id在服务器端查找有无对应的HttpSession对象，如果有，那么直接在服务器端的内存空间中找到这个对象并继续使用

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb7754f930b5737addb83cfae2fa868f4.png)

接着我们讲没有找到对应对象的情况，如果没有找到的话，那么其就会创建新的HttpSession并分配唯一的表示JSESSIONID并放置于服务器端的内存空间中



![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd1844b0ee0c6c6cc53b4a10a78df36d9.png)

但如果一开始都查找不到携带JSESSIONID的值，那么其就会创建一个新的HttpSession对象并分贝唯一标识，然后将该对象放置于服务器端，并将这个唯一标识一并发送到客户端

- HttpSession的使用

我们这里同样是通过一个需求来学习HttpSession的使用，请看下图的需求分析和实现步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEdd2743de05d0495ba88224730adf3f51.png)

那么根据实现步骤我们可以构造代码如下

```
/*
    HttpSession的使用
 */
@WebServlet("/servletDemo01")
public class ServletDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.获取请求的用户名
        String username = req.getParameter("username");
        
        //2.获取HttpSession的对象
        HttpSession session = req.getSession();
        
        //3.将用户名信息添加到共享数据中
        session.setAttribute("username",username);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

上面是构造创建共享数据和Session的代码，下面是调用共享数据和Session的代码

```
/*
    HttpSession的使用
 */
@WebServlet("/servletDemo02")
public class ServletDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.获取HttpSession对象
        HttpSession session = req.getSession();

        //2.获取共享数据
        Object username = session.getAttribute("username");

        //3.将数据响应给浏览器
        resp.getWriter().write(String.valueOf(username));
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

经过实际测试，我们会发现，的确是可以的，我们第一次使用HttpSession session = req.getSession();这个方法，由于一开始没有对应的对象，因此其会创建出来并返回。然后第二次调用就我们就获得之前创建的对象，然后获得其共享数据

这里有个问题是，为什么其能够保证获得的对象和最初创建的对象就是一样的？这点暂时还搞不明白，但可以先放着，我个人的猜测是因为此时还在一次会话中，因此其得到的Session自然也是唯一的

- HttpSession的细节

那么最后我们照例来讲讲HttpSession在使用上的细节，首先是关于唯一标识的查看

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEcb80959897d410ec9928c959d60331fc.png)

我们可以在浏览器中的检查上的查看消息头的方式来查看我们的唯一表示，由于Session的本质也是Cookie，因此是在Cookie一栏上查看

其次是浏览器其实是可以禁用Cookie的，禁用Cookie之后会对正常使用造成影响，我们的一个解决方式就是判断用户有没有禁用，若禁用则弹出对应的提示信息，告知用户不要禁用。比如我们可以在服务器端获取Session的语句下，判断获取的Session是否为空，若为空则弹出如下的提示信息

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa353d13ce94105c64f8a5f029c8dd6e7.png)

一般来说是要用弹窗来提示的，但是我们还没学到这，因此这里就先这样吧

还有一种方式是拼接URL，不过那种方式不方便，这里就不展示了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE706184c11eace4c95ac06c1fc195c2af.png)

最后是关于钝化和活化，这个具体的内容自己看吧，这里不提了。