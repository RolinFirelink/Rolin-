那么在学习完请求对象之后，我们本章来学习响应对象，要学习响应对象，当然得先知道什么是响应对象，下面我们就正式开始关于响应对象的学习

- 响应对象的介绍

响应对象其实就是回馈结果给浏览器，其他的内容和请求的时候讲解的大差不差了，这里就不赘述了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf2bfdb518281a65389bbac2c7b5e7d1b.png)

- 常见状态码的介绍

其实状态码有很多，但是我们不需要记这么多，我们只需要记一些比较常用的就可以了，下面列出了常用的状态码

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE0a7a23290ca19114b577f531cc2bd14b.png)

首先是200，代表成功，这没啥好说的。接着是302，代表重定向，关于重定向的知识我们后面会讲解到，其跟请求转发很相似，不同的是其状态栏是会改变的。然后是304，代表请求的资源没有改变，直接使用缓存数据展示给用户。然后是400，其代表请求错误，这个常见于请求的参数错误。然后是404，这个可太熟悉了，这就代表请求资源没有找到。然后是405，其代表请求的方式不受服务器支持。最后是500，其代表服务器错误，服务器宕机了或者停机了都会报这个错误。

我们可以以数字开头来简单记忆这些状态码，如果是1为开头的，说明都是些简单的消息，我们不需要去理会。2开头的代表的就是响应成功。如果是以3开头的就代表是重定向或者是使用缓存。如果是以4为开头代表的是都是些客户端的错误。如果是以5开头的就代表是服务器的错误

最后我们可以看一份完整版的状态码

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE7d756a7c268f3602424a59854d36d452.png)

- 字节流响应消息

那要使用字节流响应消息，当然首先就得获得字节流，我们这里获得字节流的方式和请求里的大同小异，是通过响应对象获得字节流。同样是继承了普通字节流，同样不必手动关闭，会自动关闭

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE3947a932c5cfec8807772ea52d827a1d.png)

那么根据上文，我们可以构造测试代码如下

```
/*
    字节流响应消息
 */
@WebServlet("/servletDemo01")
public class ServletDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取字节输出流对象
        ServletOutputStream os = resp.getOutputStream();

        //定义一个消息用于响应
        String str = "你好";

        //3.通过字节流对象输出
        os.write(str.getBytes());

        //os.close
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

这里我们知道，我们windows采用的编码格式是gbk，而我们idea采用的编码格式是UTF-8，我们使用的又是中文，理论上我们响应的结果应该是一串乱码。但是我们实际测试之后会发现居然没有得到乱码，这是为什么呢？其实这是因为getBytes()方法如果没有指定编码格式，那么其就会默认将编码格式转换为契合我们操作系统的编码格式，换言之，这个方法自动将我们的UTF-8的编码格式改变为gbk了，因此不会报编码错误。

如果我们在getBytes里用字符串指定我们的编码格式为UTF-8，那么就会报出乱码了。那么我们应该要怎么解决这个乱码问题呢？一种简单的想法就是在浏览器中将编码格式转换为UTF-8，但这招不现实，因为我们写成的页面不能要求每一个人都要先操作浏览器转换编码格式之后再看是吧，所以显然我们要先处理好这个问题。这个时候就到我们的第二个方法出场的时候了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE273637710a0468d7048b5b2bf898bc3e.png)

我们第二个方法setContentType可以设置响应内容类型来解决中文乱码问题，我们只要在我们的代码开头加上

```
resp.setContentType("text/html;charset=UTF-8");
```

就可以解决乱码问题了。这里前面的text/html表示的是我们的响应的内容的类型，这里我们可以看到我们响应的内容是text文本串，而且是html的。后面的则是我们指定的编码格式。使用这个方法会让浏览器采用我们所指定的编码格式进行编码，其作用就相当于是告知浏览器要使用怎样的编码格式。

- 字符流响应消息

上面我们学习的字节流是万能响应流，其不但能响应文本，也能响应图片一类的数据。但如果我们单纯是想响应文本的话，我们也可以用上字符流来响应

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE68d64eaf286f617b106340aaf2f981c5.png)

不过这里值得一提的是，采用字符输出流来响应时，其并不会自动改变我们的编码格式，因此我们要在开头实现设定好浏览器的编码格式，最后我可以写入代码如下

```
/*
    字符流响应消息
 */
@WebServlet("/servletDemo02")
public class ServletDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html;charset=UTF-8");

        //1.获取字符输出流对象
        PrintWriter pw = resp.getWriter();

        //2.准备一个消息
        String str = "你好";

        //3.通过字符流输出
        pw.write(str);

        //pw.close();
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

- 响应图片

之前我们都是在响应文本，那么现在我们来试着响应一张图片回去。要响应图片当然我们得要使用字节流，我们还要事先指定一个字节输入流，字节输出流则由响应对象获得，我们来看看整体步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE24322d47f4305da2bd64224eea9bc57c.png)

但是这里还有一点是值得一提的，就是当我们关联图片读取路径时，我们需要关联我们的图片的在项目里的路径而不是虚拟路径，因为Tomcat服务器还会将我们的项目发布，一旦发布，那么我们图片的路径其实就变了，其会拥有一个另外的项目路径，这时候就需要绝对路径来指定这个文件了。如果我们不这样做，那么在我们的浏览器里会报500的服务器错误。我们就可以使用上我们在请求章节里学习的方法，那么最后我们构造代码如下

```
/*
    响应图片
 */
@WebServlet("/servletDemo03")
public class ServletDemo3 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //通过文件的相对路径获取绝对路径
        String realPath = getServletContext().getRealPath("/img/ruinkingl.webp");
        System.out.println(realPath);

        //创建字节输入流对象，关联图片路径
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream(realPath));

        //2.通过响应对象获取字节输出流对象
        ServletOutputStream os = resp.getOutputStream();

        //循环读写
        byte[] arr = new byte[1024];
        int len;
        while ((len=bis.read(arr))!=-1){
            os.write(arr,0,len);
        }

        //4.释放资源
        bis.close();
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

- 设置缓存时间

为了提高我们服务器的运行效率，我们可以设置缓存时间，让用户端在请求一次之后，一定时间之内会直接从缓存文件中获取想要的资源，而不是从服务器中再次请求获得。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE1de8d31fe14762c1bb138f920cbe84e6.png)

如果我们想要设置一小时的缓存时间，我们应该要指定一个字符串，是英文单词过期，不过实际上这个单词可以随意指定，并没有影响，我们这里特别指定为英文单词的过期主要是因为我们要考虑到其他人要读得懂，然后我们要先获取系统的当前时间，然后加上一小时的毫秒数，这样就可以构成其过期时间。值得一提的是，我们这里的方法是响应对象里的方法，最后我们来看看其代码

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE027035f6ec47b1100e3ee989f9507474.png)

- 设置刷新时间

我们可以设置刷新时间，让我们的用户在浏览器中点进入某个选项之后就会自动跳转到某个界面，这个我们见得多了，现在我们就来实现下

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb1010739e4e7513abae131a1dbfae3cc.png)

这里同样也是响应对象的方法，消息头名称可以随意设置，但是最好还是设置成刷新的英文单词，然后比较就讲究的是第二个参数value，value要设置的格式是s;URL=/xxx/xxx，这里s代表的是秒数，如果我们希望我们的界面三秒后刷新，那么我们就应该将其指定为3。同时我们要跳转的界面的地址应该是虚拟路径结合实际路径的方式，我猜测之所以这样设置的理由是其内部对字符串进行了截取，然后进行了特殊的处理。因此我们这样设置就可以了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb4a17a8a8af6553fc4c77349dde2965a.png)

- 请求重定向

那么现在我们就来学习请求重定向，所谓请求重定向的意思是，当客户端的一次请求到达之后，发现需要借助其他的Servlet来实现功能，此时可以使用重定向。这与请求转发的作用是非常类似的，但是不同的是，请求重定向时，浏览器地址栏会发生改变，共发生两次请求，请求域对象中不能共享数据且可以重定向到其他服务器。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE12108e23b2fea33a14d392080d7f4689.png)

重定向的实现原理时先设置响应状态码，然后设置响应的资源路径，同样要设置消息头和消息地址。不过在响应对象里提供了更加简便的重定向方法，其就是sendRedirect方法，该方法可以设置重定向，我们就使用这个方法来完成重定向。那么我们可以构造第一个代码如下

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf038eb3e3e492f4e06465080a4d9640f.png)

在这个代码里我们有一个执行的测试语句，然后我们在请求域里设置了共享数据，最后我们设置了重定向，这里我们设置的重定向的地址是我们的虚拟目录加上我们的指定的具体的类，我们这里是采用方法动态获取我们项目的虚拟目录的方式。然后是一个直接重定向到百度首页的一个语句，主要是用于测试的。然后我们来看看另外一份代码

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEcfa0d32691b0568cc9948000a06d5e9d.png)

这份代码里我们先测试其是否执行，然后尝试去获取共享数据，看看能否获取到。

最终我们的结果当然是两个都执行了，但是第二个地址无法获取到共享数据，这是因为重定向总共有两次请求，两次请求导致了请求域是发生变化了的，自然无法获取。同时由于是两次请求，所以两份代码的功能都会执行。最后由于是两次请求，因此我们的地址栏最后会变成第二个代码的地址，其本质过程是第一个代码执行了之后发生了重定向，向第二份代码发送了请求，此时浏览器跳转到第二份代码的地址执行第二份代码的功能

最后我们来解决一个问题，就是重定向和请求转发如此相似，那么我们应该怎么确定使用哪一个呢？我们只要看需不需要用得到共享数据就行了，如果用得上就用请求转发，用不上就用重定向

- 文件下载

接下来我们来学习我们响应对象的最后一个内容，就是文件下载，我们希望实现用户能够从我们的网址里下载我们想要的东西的功能。先来看看步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEab666fdcc10451fec6f295579931b469.png)

上面的六个步骤里，只有第二个和第三个步骤是不同的，而这两个步骤具体怎么实现，又有什么作用，这个等到我们去代码上进行讲解。最终我们可以构造我们的下载代码如下

```
/*
    下载文件
 */
@WebServlet("/servletDemo04")
public class ServletDemo4 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.创建字节输入流对象，关联读取的文件
        String realPath = getServletContext().getRealPath("/img/ruinkingl.webp");
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream(realPath));

        //2.设置响应头支持的类型
        /*
            Content-Type 消息头名称   支持的类型
            application/octet-stream 消息头参数  应用的类型为字节流
         */
        resp.setHeader("Content-Type","application/octet-stream");

        //3.设置响应头以下载方式打开附件
        /*
            Content-Disposition 消息头名称   处理的形式
            attachment;filename=pobaiwang.png 消息头参数 附件形式进行处理 指定下载文件的名称
         */
        resp.setHeader("Content-Disposition","attachment;filename=pobaiwang.png");

        //4.获取字节输出流对象
        ServletOutputStream os = resp.getOutputStream();

        //5.循环读写
        byte[] arr = new byte[1024];
        int len;
        while ((len = bis.read(arr))!=-1){
            os.write(arr,0,len);
        }

        //6.释放资源
        bis.close();
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

其实我们可以看到我们的第二个和第三个步骤依赖的都是setHeader这个方法，其实这两个方法的主要作用就是告知浏览器去下载的。当然其里面具体的内容还不清楚，暂且先记下来也可以。

最后我们可以做一个对响应对象的方法的总结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE453af1d8d7d3d8b8e7d5b320a1a0d0b2.png)

