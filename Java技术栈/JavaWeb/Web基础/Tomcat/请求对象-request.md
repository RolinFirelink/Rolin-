- 请求对象的介绍

在介绍请求对象之前，我们得先搞明白什么是请求。所谓请求就是在BS架构中，客户端向服务器发出询问。而请求对象，则是在项目中用于发送请求的对象，对象中含有请求的数据，对象一般有两种，一种是ServletRequest，另一种是HttpServletRequest。前者是一个普通的请求对象，而后者是基于HTTP协议的请求对象

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa718cc237b84c9a324d16a048fe39175.png)

关于各种不同的请求对象，我们曾经是有一一讲述过的，现在我们不妨再来回顾一遍，请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE0967aab55d9630af10dd8be85f617dc7.png)

我们可以看到无论是在哪个接口或者是抽象类，其下都一定有请求和响应两个对象。Servlet接口下有两个请求和响应的接口，然后HttpServletRequest和HttpServletResponse这两个接口是分别继承了前者的两个接口的接口。既然是接口，那么我们在使用的时候就应该定义一个实现类去实现这个接口才能够使用，但是实际上我们并不需要这么做，这是因位在Tomcat服务器中，其会自动帮我们构造这个实现类然后实现接口的功能，对于我们程序员而言，我们只需要搞懂这个请求和响应的两个接口的内部方法就可以了

- 获取各种路径的方法

由于请求对象里的方法比较多，我们这里来分步介绍，并且分步测试。首先我们来讲解获取各种路径的方法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb6b88d41f571777bb43068cac817fe7b.png)

第一个getContextPath方法我们之前已经讲解过了，就是用于获取虚拟目录名称的。而getServletPath是用于获取Servlet映射路径的。getRemoteAddr方法可以获取访问者的ip地址。getQueryString可以获取请求的消息数据，也就是我们填写的表单项的数据。getRequestURI获取统一资源的标识符，而getRequestURL获取统一资源定位符。至于什么是资源标识符，什么是资源定位符，我们可以等会来举例的时候一起讲解

我们现在先创造一个ServletDemo01类，将虚拟目录在run edit上设置为request，然后我们在该类下写入如下代码

```
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/*
    获取路径的相关方法
 */
@WebServlet("/servletDemo01")
public class ServletDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.获取虚拟目录名称 getContextPath()
        String contextPath = req.getContextPath();
        System.out.println(contextPath);

        //2.获取Servlet映射路径 getServletPath()
        String servletPath = req.getServletPath();
        System.out.println(servletPath);

        //3.获取访问者ip getRemoteAddr
        String ip = req.getRemoteAddr();
        System.out.println(ip);

        //4.获取请求消息的数据 getQueryString()
        String queryString = req.getQueryString();
        System.out.println(queryString);

        //5.获取统一资源标识符 getRequestURI()
        String requestURI = req.getRequestURI();
        System.out.println(requestURI);

        //6.获取统一资源定位符 getRequestURL()
        StringBuffer requestURL = req.getRequestURL();
        System.out.println(requestURL);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

然后我们启动Tomcat，可以获得如下结果

```
        /request
        /servletDemo01
        0:0:0:0:0:0:0:1
        null
        /request/servletDemo01
        http://localhost:8080/request/servletDemo01
```

我们可以看到，获取的虚拟目录其实就是我们当初在edit上设置的虚拟目录，这没啥问题。然后获取的映射路径其实是我们的在网址里要找到的类时所输入的地址，只不过这里我们通过注解的形式所以输入的正好与类名一致，我猜想如果我们采用配置文件的方式，这个映射路径的方式还能变化。然后是ip地址，由于我们其实是自己访问自己，因此ip地址的显示得有些怪。接着是请求消息的数据，由于我们的请求里，什么数据都没有提供，所以这里什么都没有。如果我们在我们的网址里加上http://localhost:8080/request/servletDemo01?username=aaa&password=123，那么打印时其结果将会是username=aaa&password=123（当然这个方法的使用其实比较少，因为我们获得的是一个整体，而不能获得具体的数据，这里只做了解就好了）。接着是统一资源标识符，我们可以看到其实统一资源标识符就是我们的虚拟路径结合映射的一个地址，就是我们输入的网址的端口号后面的内容。最后是统一资源定位符，我们可以看到其实统一资源定位符就是我们当初输入的用于发送请求的网址。统一资源标识符和统一资源定位符能表示的范围，前者大，后者小。

- 获取请求头信息

接下来我们来学习获取请求头信息的方法，还记得这里主要是获得我们的请求里的请求头的值和名称。还记得什么是请求头吗？要记住我们的请求内部是分为请求行，请求头，请求空行和请求体多个部分的，其中，get请求方式没有请求体，其数据被存放于请求行，而POST请求方式则把数据保存在请求体。接下来我们来正式学习这些方法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE001b74987b3a5389801c876fa9394462.png)

首先是getHeader，其作用是根据请求头的名称获取一个值。而getHeaders则是根据请求头的名称，获取请求头中的多个值，并以枚举形式返回。最后是getHeaderNames方法，这个方法可以以枚举形式返回所有的请求头名称。光看方法我们都知道这些方法其实有些是要结合着使用的，这样才可以获取到我们想所需要的数据

为了便于我们的学习，我们这里直接给出一个请求头的数据，然后我们来构造代码来获取这些数据

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE55f9c25c869bef3a98ba4d09063211b5.png)

那么接下来我们就来一一测试这些方法，我们可以写入代码如下

```
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Enumeration;

/*
    获取请求头信息的相关方法
 */
@WebServlet("/servletDemo02")
public class ServletDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.根据请求头名称获取一个值
        String connection = req.getHeader("connection");
        System.out.println(connection);
        System.out.println("------------------------");

        //2.根据请求头名称获取多个值
        Enumeration<String> values = req.getHeaders("accept-encoding");
        while (values.hasMoreElements()){
            String value = values.nextElement();
            System.out.println(value);
        }
        System.out.println("-------------------------");

        //3.获取所有的请求头名称
        Enumeration<String> names = req.getHeaderNames();
        while (names.hasMoreElements()){
            String name = names.nextElement();
            String value = req.getHeader(name);
            System.out.println(name+","+value);
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

最终我们可以获得结果如下

```
        keep-alive
        ------------------------
        gzip, deflate, br
        -------------------------
        host,localhost:8080
        connection,keep-alive
        sec-ch-ua," Not A;Brand";v="99", "Chromium";v="99", "Google Chrome";v="99"
        sec-ch-ua-mobile,?0
        sec-ch-ua-platform,"Windows"
        upgrade-insecure-requests,1
        user-agent,Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.51 Safari/537.36
        accept,text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
        sec-fetch-site,none
        sec-fetch-mode,navigate
        sec-fetch-user,?1
        sec-fetch-dest,document
        accept-encoding,gzip, deflate, br
        accept-language,zh-CN,zh;q=0.9
        cookie,Idea-422b84f0=0e63fa28-b867-4066-9739-5c256c28d91f
```

可以看到我们第一个结果就是我们要的对应的请求头的值。第二个结果我们获取了某个请求头名称里的所有值。而第三个测试代码里，我们获取了所有的请求头名称，然后获取了所有请求头名称，并用其获取了所有请求头的值，总的来说就是获取了请求头的所有内容，实际上我们控制台里打印的也的确是请求头的所有内容。我们看到我们如果没有获取请求头的具体数据的名称的所有值的话，其不是返回第一个值，而是将所有值都返回给我们。而调用第二个方法就可以具体获得请求头名称的所有数据枚举形式，会将其自动分开以供我们使用

- 获取请求参数信息的方法

我们发送请求的时候往往会带有一些数据，那些数据其实就是参数信息，比方说，我们传入的账号密码那些的，我们当然也有获取这些参数信息的方法，请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb0eab6da8ef4ab2b3745daa9a703fa97.png)

首先是getParameter，其会根据名称获取传入的数据。然后是getParameterValues，其会根据名称获取所有数据，并以枚举类型返回。接着是getParameterNames方法，其会以枚举类型返回所有参数名称。最后是getParameterMap，其可以获取所有参数的键值对，第一个String保存参数名称，后面的String[]数组保存参数的值

那么我们接下来就来测试下这个方法，要测试这个方法，首先我们要构造一个传入参数的页面，我们可以构造该页面如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册页面</title>
</head>
<body>
    <!--提前通过Run Edit设置其虚拟目录为stu，此处先写入虚拟目录，然后写入构造的类，就可以将数据传入到指定的类-->
    <form action="/request/servletDemo03" method="get" autocomplete="off">
        姓名：<input type="text" name="username"> <br/>
        密码：<input type="password" name="password"> <br/>
        爱好：<input type="checkbox" name="hobby" value="study">学习
             <input type="checkbox" name="hobby" value="game">游戏<br>
        <button type="submit">注册</button>
    </form>
</body>
</html>
```

接着我们可以写入测试代码如下

```
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Enumeration;
import java.util.Map;

/*
    获取请求参数信息的相关方法
 */
@WebServlet("/servletDemo03")
public class ServletDemo3 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.根据名称获取数据 getParameter()
        String username = req.getParameter("username");
        System.out.println(username);
        String password = req.getParameter("password");
        System.out.println(password);
        System.out.println("--------------------");

        //2.根据名称获取所有数据 getParameterValues()
        String[] hobbies = req.getParameterValues("hobby");
        for (String hobby:hobbies) {
            System.out.println(hobby);
        }
        System.out.println("---------------------");

        //3.获取所有名称 getParameterNames()
        Enumeration<String> names = req.getParameterNames();
        while (names.hasMoreElements()){
            String name = names.nextElement();
            System.out.println(name);
        }

        //4.获取所有参数的键值对 getParameterMap()
        Map<String, String[]> map = req.getParameterMap();
        for (String key:map.keySet()) {
            String[] values = map.get(key);
            System.out.print(key+":");
            for (String value:values) {
                System.out.print(value+ " ");
            }
            System.out.println();
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

然后运行，我们可以获得测试结果为

```
        zhangsan
        123
        --------------------
        study
        game
        ---------------------
        username
        password
        hobby
        username:zhangsan
        password:123
        hobby:study game 

```

第一个方法我们成功获取到了我们的姓名和密码的数据。第二个方法我们成功获取了第三个数据的所有内容。然后在第三个方法的测试里，我们成功获取了我们的参数信息的所有内容。这就说明我们的测试已经成功了。这里要注意的是，我们比较常用的是第三个方法

- 获取参数手动封装对象

现在我们已经能够获取到我们的请求参数的数据了，如果我们要把参数传给其他类的话，我们当然可以构造一个函数将其各个数据传入，但是当数据量小时，这个方法才管用，如果数据量一多，那么这个方法就很麻烦了，那么我们有什么方法能够简化这个传入数据的过程吗？

当然有，我们可以创建一个对象，对象内部存放我们的参数数据，然后直接将这个对象传过去就行了。这个过程我们称之为获取参数并封装，封装一共有三种方式，分别是手动封装、反射封装和工具类封装。我们这里先讲第一种方式

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf74c38778fe3c1e25c2eba5f9675a22d.png)

我们首先应该先创造一个Student类用于存储我们的数据，注意其下定义的成员变量的名称应与我们数据提交时name的属性值里设置的名称保持一致。并且按照构造类的定义，我们给其提供了构造方法，setter and getter方法以及toString方法。最后我可以写入其代码如下

```
package com.itheima.servlet.bean;

import java.util.Arrays;

public class Student {
    private String username;
    private String password;
    private String[] hobby;

    @Override
    public String toString() {
        return "Student{" +
                "username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", hobby=" + Arrays.toString(hobby) +
                '}';
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String[] getHobby() {
        return hobby;
    }

    public void setHobby(String[] hobby) {
        this.hobby = hobby;
    }

    public Student() {
    }

    public Student(String username, String password, String[] hobby) {
        this.username = username;
        this.password = password;
        this.hobby = hobby;
    }
}
```

接着我们我们可以再类中写入代码如下

```
package com.itheima.servlet.Servlet;

import com.itheima.servlet.bean.Student;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Enumeration;
import java.util.Map;

/*
    封装对象---手动封装
 */
@WebServlet("/servletDemo04")
public class ServletDemo4 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.获取所有的数据
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String[] hobbies = req.getParameterValues("hobby");

        //2.封装学生对象
        Student stu = new Student(username,password,hobbies);

        //3.输出对象
        System.out.println(stu);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

最后我们启动Tomcat，其会在控制台上打印

```
Student{username='zhangsan', password='123', hobby=[study, game]}
```

这就说明我们的对象已经封装成功了

- 获取参数反射封装对象

但是我们容易观察到，每次获取数据我们调用一次获取数据的方法，这个属实是太麻烦了，所以我们应该要简化它，用反射来进行简化是最容易想到的方法。

利用反射改造代码，我们可以将代码改造如下

```
package com.itheima.servlet.Servlet;

import com.itheima.servlet.bean.Student;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.beans.IntrospectionException;
import java.beans.PropertyDescriptor;
import java.io.IOException;
import java.lang.reflect.Method;
import java.util.Map;

/*
    封装对象---反射方式
 */
@WebServlet("/servletDemo05")
public class ServletDemo5 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.获取所有的数据
        Map<String, String[]> map = req.getParameterMap();

        //2.封装学生对象
        Student stu = new Student();
        //2.1遍历集合
        for (String name:map.keySet()) {
            String[] value = map.get(name);
            try {
                //2.2获取Student对象的属性描述器
                PropertyDescriptor pd = new PropertyDescriptor(name, stu.getClass());
                //2.3获取对应的setXxx方法
                Method writeMethod = pd.getWriteMethod();
                //2.4执行方法
                if(value.length>1){
                    writeMethod.invoke(stu,(Object) value);
                }else {
                    writeMethod.invoke(stu,value);
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }

        //3.输出对象
        System.out.println(stu);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

这里我们通过反射的机制是依靠Java中的一个功能类，称为属性描述器，其名称为PropertyDescriptor，构造这个对象需要传入对应的属性和该属性所在的类的class对象。这个工具类的原理是通过获得其getterandsetter方法并封装于对象中，然后用户可以调用其方法完成对值的修改。该对象中有getWriteMethod方法，该方法可以返回一个Method对象，这个Method对象中又有invoke方法，调用该方法传入要修改的对象以及我们要修改的值就可以完成修改。这里要注意的是如果我们要修改的值是一个数组，那么我们要将其改为Object类型，这样才能正确完成赋值。至于原理可以自己看源码，这里就不赘述了

最后我们经过测试能够发现我们这个代码也是能够完成我们的需求的，其相对第一个方法而言就变得更加通用了

- 获取参数工具类封装对象

虽然说我们上面的方法变得更加通用了，但实际上如果每次都要重新构造一个，还是很麻烦的。一个简单的想法就是我们可以将这一整个流程构造成一个工具类，然后我们想要封装参数数据的时候直接调用这个工具类的方法就可以了。这个想法是很好的，我们本章也就是要学习这个方法

工具类这里我们就不用写了，我们已经写好了，将资料中的jar包导入然后add as a library，然后点击project 死抓克特，点击problem，点击fix，然后jar包也添加到tomcat中，我们接下来只要将代码改造成这样

```
package com.itheima.servlet.Servlet;

import com.itheima.servlet.bean.Student;
import org.apache.commons.beanutils.BeanUtils;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.beans.PropertyDescriptor;
import java.io.IOException;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.util.Map;

/*
    封装对象---工具类方式
 */
@WebServlet("/servletDemo05")
public class ServletDemo6 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.获取所有的数据
        Map<String, String[]> map = req.getParameterMap();

        //2.封装学生对象
        Student stu = new Student();
        try {
            BeanUtils.populate(stu,map);
        } catch (Exception e) {
            e.printStackTrace();
        }

        //3.输出对象
        System.out.println(stu);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

这真的是，简单太多了，一般我们只用两个字形容这种终极方法，无敌！

- 流对象获取数据

以前我们获取参数信息都是通过req对象的各种方法来获取的，但实际上我们还可以通过流对象来获取，但是要注意，这种方式是支持post方式，如果是get方式，则是不支持的。我猜测这是因为归根结底我们的流读取的还是方法体内的内容，而get方式是没有方法体的，自然就无法使用。

我们用流对象获取数据总共有两种方式

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2f349ce655f594a51f6219dd523ef76b.png)

这两种方式就对应我们以前IO流所学习的字符输入流和字节输入流，不过这里我们的字节输入流是ServletInputStream，其是继承了一般的字节输入流的输入流。同时由于这两个流都是直接从req对象中获得的，因此我们不必去关闭它，因为当我们的req对象销毁的时候，这两个流也会被自动关闭

如果要使用流对象获取数据，我们可以将代码改造如下

```
package com.itheima.servlet.Servlet;

import com.itheima.servlet.bean.Student;
import org.apache.commons.beanutils.BeanUtils;

import javax.servlet.ServletException;
import javax.servlet.ServletInputStream;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.BufferedReader;
import java.io.IOException;
import java.util.Map;

/*
    流对象获取数据
 */
@WebServlet("/servletDemo07")
public class ServletDemo7 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //字符流(必须是post方式)
/*        BufferedReader br = req.getReader();
        String line;
        while ((line = br.readLine()) != null){
            System.out.println(line);
        }*/
        //br.close();
        
        //字节流
        ServletInputStream is = req.getInputStream();
        byte[] arr = new byte[1024];
        int len;
        while ((len=is.read(arr)) != -1){
            System.out.println(new String(arr,0,len));
        }
        //is.close();
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

最后一点是，这个方式一般用于获取信息数据时没什么用，因为我们有更好的方法。但是这个方法用来获取一些用户上传的文件，是可以的。

- 中文乱码问题

我们如果输入中文，那么在控制台上会出现乱码，现在我们就来解决一下

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE062118178940cc89248540bc6a55d2cc.png)

虽然图上是这么说的，但实际上我们的idea无论是哪种方式都是乱码，干

如果采用post方式又需要控制台输出正确的代码，可以在类的起始位置写入req.setCharacterEncoding("UTF-8");将编码格式设置为UTF-8，当然这里之所以设置为UTF-8而不是gbk，这主要是因为我们的html文件里的字符串的格式就是UTF-8

- 请求域的介绍

请求域也就是request域，可以在一次请求范围内共享数据。一般用于请求转发的多个资源中共享数据。

其应用场景非常简单，比如说一个客户端向ServletA发送了一个请求，而ServletA无法实现这个请求，只有ServletB能实现，那么一个简单的想法就是让ServletA将请求发送给ServletB，然后让ServletB去实现这个请求。这种行为的专有名词描述是请求转发，而ServletB在响应客户的请求时可能要用上请求的数据，此时就需要共享数据了，这时候就要用上请求域，之所以是用请求域而不是用应用域的原因是，应用域太大了，请求域的大小就正适合。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEea45c4fb31b834225e564599247c924d.png)

其令对象获得和修改请求域中共享数据的方法和应用域的其实大同小异，这里就不再赘述了

- 请求转发

请求转发有以下几点是我们需要注意的：1、浏览器的地址栏不会变。2、域对象的数据不会丢失。3、负责转发的Servlet转发前后的响应正文会消失。4、由转发的目的地来响应客户端。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc1008738d682418b63620e958051d8dc.png)

获得共享数据的方法上一章已经介绍过了，这里我们来介绍将数据共享的方法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd306e7a27b33a63812f88c198db4f2d0.png)

将数据共享的方法主要有两个，一个是getRequestDispatcher方法，该方法用于获取请求调度对象，其实就是直接传入一个类，指定这个类会获得我们的共享内容。然后是forward(ServletRequest req,ServletResponse resp)，该方法的作用就是实现转发，直接将请求和响应对象传入就可以了，其会将该类的请求域内种的内容转发过去。

那么我们可以构造测试代码如下

```
package com.itheima.servlet.Servlet;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/*
    请求转发
 */
@WebServlet("/servletDemo09")
public class ServletDemo9 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //设置共享数据
        req.setAttribute("encoding","gbk");

        //获取请求调度对象
        RequestDispatcher rd = req.getRequestDispatcher("/servletDemo10");
        //实现转发功能
        rd.forward(req,resp);

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

上面是servletDemo09的测试代码，下面是servletDemo10的测试代码，也是前者的请求转发的指定对象

```
package com.itheima.servlet.Servlet;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/*
    请求转发
 */
@WebServlet("/servletDemo10")
public class ServletDemo10 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取共享数据
        Object encoding = req.getAttribute("encoding");
        System.out.println(encoding);

        System.out.println("servletDemo10执行了");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

经过实际测试我们发现这是确实可行的，我们在servletDemo10里获得了我们共享的数据

- 请求包含

什么是请求包含呢？其实请求包含的内容与请求转发有很大相似，但又有所不同。请求包含简单来说就是可以合并其他Servlet中的功能一起响应给客户端，比方说用户向ServletA发送了一个请求，但是这个请求ServletA无法独立完成，还需要ServletB来帮助完成一部分，这时候就需要用上请求包含。其实请求包含简单来说就是两个Servlet的功能都会执行一次，这是最简单的理解

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8ccfbe6abab44478658f4c4335f79750.png)

请求包含有三个特点，分别是1、浏览器地址栏不变。2、域对象的数据不丢失。3、被包含的Servlet响应头会丢失。与前面的请求转发一样，其也有对应的方法，我们来看看

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE92eb769f8797cd579770d92a320ec4ba.png)

获取请求调度对象的方法就不提了，这里我们实现包含的方法是include()，同样要传入请求和响应对象。那么我们可以构造代码如下

```
package com.itheima.servlet.Servlet;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/*
    请求包含
 */
@WebServlet("/servletDemo11")
public class ServletDemo11 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("servletDemo11执行了");

        //获取请求调度对象
        RequestDispatcher rd = req.getRequestDispatcher("/servletDemo12");
        //实现包含功能
        rd.include(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

上面是servletDemo11的代码，下面是12的代码

```
package com.itheima.servlet.Servlet;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/*
    请求包含
 */
@WebServlet("/servletDemo12")
public class ServletDemo12 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("servletDemo12执行了...");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

最后我们可以对我们本章学习的方法做一个总结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE046721ea2de04b6d4220ca49648e5890.png)

关于请求转发和请求包含的异同可以参考这个网址https://blog.csdn.net/qq_39740279/article/details/99631511

简单来说，请求转发由被转发的功能类完成响应，而请求包含则是被转发的功能类和被转发的功能类共同完成响应。至于什么时候用请求转发，什么时候用请求包含，这个目前还整不明白