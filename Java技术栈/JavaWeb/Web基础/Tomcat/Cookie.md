那么现在我们就到了下一章节了，在这一章节里，我们一共要学习三个技术，分别是Cookie、Session和JSP。其中Cookie和Session是会话相关的技术，而JSP是界面相关的技术。我们本章学习Cookie

- 会话技术介绍

当然，在学习Cookie之前，我得先搞明白什么是会话技术。会话其实就是浏览器和服务器之间的多次请求和响应。为了实现一些功能，服务器和浏览器之间是可能产生多次请求和响应的，这期间产生的多次请求和响应加载一起就成为一次会话。

而会话过程中产生的一些数据，可以通过会话技术Cookie和Session保存，我个人觉得这似乎有点像缓存

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE84c42d6c8abd56978b1be6d3e918fdf8.png)

- Cookie介绍

首先Cookie是客户端的会话管理技术，其会将要共享的数据保存到客户端，客户端每次请求时，就将会话会话信息带到服务器端，从而实现多次请求的数据共享。其作用简单来说就是可以保存客户端访问网站的相关内容，从而保证每次访问时都先从本地文件中获取，以此来提升效率。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEadb2fa7ebb083785c2728c868a83699e.png)

下面我们来看看Cookie在帮助文档中的介绍

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE574bc73cee6ffb2c9627fb5d83296b4e.png)

这里我们要记住的是Cookie就是一个普通的java类，其次是一个cookie包含有名称和属性这两个最重要的值，其他的属性类似于路径、域名当然也有，但属于是可选内容，相对没那么重要。然后是浏览器将Cookie带回到服务器端利用的是HTTP协议中的请求消息头的方式，通过调用请求对象中的addcookie方法。通过HttpServletRequest对象的getCookies方法可以获取到当前网站中的全部Cookie。几个Cookie可能具有相同的名称，但是路径属性不同，换言之，确定唯一的Cookie不能只能名称

- Cookie属性

Cookie中的属性是非常重要的，因此我们来着重讲解

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc225dc4da5b4d5910fc5766786bda89e.png)

可以看到Cookie七种属性，分别是name（Cookie的名称）、value（Cookie的值（不支持中文））、path（Cookie的路径）、domain（Cookie的域名）、maxAge（Cookie的存活时间）、version、comment。其中name和value的属性是必须的，而path到maxAge的属性则是属于是很重要的属性，最好也要赋值。最后两个属性可有可无

我们也可以进入Cookie类中来进一步了解这些属性

```
public class Cookie implements Cloneable, Serializable {
    private static final CookieNameValidator validation;
    private static final long serialVersionUID = 1L;
    private final String name;
    private String value;
    private int version = 0;
    private String comment;
    private String domain;
    private int maxAge = -1;
    private String path;
    private boolean secure;
    private boolean httpOnly;
```

可以看到其实那些属性就是成员变量，这里我们要注意的name是被final修饰的，就意味着我们的Cookie属性一旦赋值就不可修改，其次是我们的值也是String属性的

- Cookie的方法的添加获取

学习完了属性之后，我们现在来学习Cookie类中对应的方法，请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb18eaad66b481e8c62a1e91cbbf77338.png)

我们可以进入到具体的类中来进一步了解这些方法

```
public Cookie(String name, String value) {
    validation.validate(name);
    this.name = name;
    this.value = value;
}

public void setComment(String purpose) {
    this.comment = purpose;
}

public String getComment() {
    return this.comment;
}

public void setDomain(String pattern) {
    this.domain = pattern.toLowerCase(Locale.ENGLISH);
}

public String getDomain() {
    return this.domain;
}

public void setMaxAge(int expiry) {
    this.maxAge = expiry;
}

public int getMaxAge() {
    return this.maxAge;
}

public void setPath(String uri) {
    this.path = uri;
}

public String getPath() {
    return this.path;
}

public void setSecure(boolean flag) {
    this.secure = flag;
}

public boolean getSecure() {
    return this.secure;
}

public String getName() {
    return this.name;
}

public void setValue(String newValue) {
    this.value = newValue;
}

public String getValue() {
    return this.value;
}

public int getVersion() {
    return this.version;
}

public void setVersion(int v) {
    this.version = v;
}
```

这里值得一提的是Cookie类中并没有提供无参的构造方法，只有一个有参的构造方法，如果要构造一个Cookie对象，就必须要传入name和value来创建。接着下面提供了各种属性对应的getter and setter方法，由于name是被final修饰的，因此name没有set方法，只有get方法

然后我们来学习Cookie的添加和获取方法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc98cfab5be6f25bec169fb4fb4aa434a.png)

其向客户端添加Cookie方法是在HttpServletResponse中利用addCookie方法，传一个Cookie对象就可以将该对象传到客户端。而获取Cookie方法是利用HttpServletRequest中的getCookies方法，获得一个Cookie数组，可以将客户端中所有的Cookie获取

其实这个也非常符合我们的一般想法，在服务器的响应中，我们将Cookie传给客户端，在服务器的请求中，我们获取里面的所有Cookie

- Cookie的使用

关于Cookie的使用，我们决议通过Cookie的一个案例实现来达到学习的目的。先来看看我们的需求以及我们的实现步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEaa8d3c55af57bf765bd4aac1a9534e63.png)

根据这个实现步骤，我们可以写入代码如下

```
/*
    Cookie的使用
 */
@WebServlet("/servletDemo01")
public class ServletDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.通过响应对象写出提示信息
        resp.setContentType("text/html;charset=UTF-8");
        PrintWriter pw = resp.getWriter();
        pw.write("欢迎访问本网站，您的最后访问时间为：<br>");

        //2.创建Cookie对象，用于记录最后访问时间
        Cookie cookie = new Cookie("time", System.currentTimeMillis() + "");

        //3.设置最大存活时间
        cookie.setMaxAge(3600);

        //4.将cookie对象添加到客户端
        resp.addCookie(cookie);

        //5.获取cookie
        Cookie[] arr = req.getCookies();
        for (Cookie c:arr) {
            if("time".equals(c.getName())){
                //6.获取cookie对象中的value，进行写出
                String value = c.getValue();
                SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
                pw.write(sdf.format(new Date(Long.parseLong(value))));
            }
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

上面的代码就是结合了我们之前学习的内容综合实现的，经过测试我们也发现这个程序没有问题，那么我们就算是成功实现了

- Cookie的使用的细节

学习完了Cookie的使用之后，我们接着来学习的Cookie的使用细节

首先Cookie是有数量限制，每个网站最多只能有20个Cookie，且大小不能超过4KB。所有网站的Cookie总数不能超过300个。

其次是Cookie的名称只能包含阿斯克码表的字母、数字字符。不能包含逗号、分号、空格，不能以$开头，且其值不支持中文

然后是关于setMaxAge()方法接受数字，如果接受的是负整数，则浏览器关闭时就会清除Cookie。如果是0就立刻清除，浏览器一接受到Cookie就立刻清除了。如果是正整数则是会以秒为单位设置存活时间

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE019041d927404eecdbf08dce951e7614.png)

最后是关于访问路径限制的细节，默认路径是取自第一次访问的资源路径前缀，后续的访问路径只要有这个路径开头就能访问到。比如说，我们映射了servlet/servletdemo01，并传入了Cookie。然后写入servlet/servletdemo02，获取cookie，然后写入servlet/aaa/servlet555，同样获取cookie，最后写入aaa/servlet888，同样是获取cookie。那么最后只有最后一个获取不到Cookie，其他的都可以获取到，这是因为最后一个没有吸入对应的第一次访问资源路径的前缀导致的

