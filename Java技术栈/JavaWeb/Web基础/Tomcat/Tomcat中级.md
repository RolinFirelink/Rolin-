# 请求对象-request

## 请求对象的介绍

在介绍请求对象之前，我们得先搞明白什么是请求。所谓请求就是在BS架构中，客户端向服务器发出询问。而请求对象，则是在项目中用于发送请求的对象，对象中含有请求的数据，对象一般有两种，一种是ServletRequest，另一种是HttpServletRequest。前者是一个普通的请求对象，而后者是基于HTTP协议的请求对象

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE001b74987b3a5389801c876fa9394462.png)

关于各种不同的请求对象，我们曾经是有一一讲述过的，现在我们不妨再来回顾一遍，请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0967aab55d9630af10dd8be85f617dc7.png)

我们可以看到无论是在哪个接口或者是抽象类，其下都一定有请求和响应两个对象。Servlet接口下有两个请求和响应的接口，然后HttpServletRequest和HttpServletResponse这两个接口是分别继承了前者的两个接口的接口。既然是接口，那么我们在使用的时候就应该定义一个实现类去实现这个接口才能够使用，但是实际上我们并不需要这么做，这是因位在Tomcat服务器中，其会自动帮我们构造这个实现类然后实现接口的功能，对于我们程序员而言，我们只需要搞懂这个请求和响应的两个接口的内部方法就可以了

## 获取各种路径的方法

由于请求对象里的方法比较多，我们这里来分步介绍，并且分步测试。首先我们来讲解获取各种路径的方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa718cc237b84c9a324d16a048fe39175.png)

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

## 获取请求头信息

接下来我们来学习获取请求头信息的方法，还记得这里主要是获得我们的请求里的请求头的值和名称。还记得什么是请求头吗？要记住我们的请求内部是分为请求行，请求头，请求空行和请求体多个部分的，其中，get请求方式没有请求体，其数据被存放于请求行，而POST请求方式则把数据保存在请求体。接下来我们来正式学习这些方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb6b88d41f571777bb43068cac817fe7b.png)

首先是getHeader，其作用是根据请求头的名称获取一个值。而getHeaders则是根据请求头的名称，获取请求头中的多个值，并以枚举形式返回。最后是getHeaderNames方法，这个方法可以以枚举形式返回所有的请求头名称。光看方法我们都知道这些方法其实有些是要结合着使用的，这样才可以获取到我们想所需要的数据

为了便于我们的学习，我们这里直接给出一个请求头的数据，然后我们来构造代码来获取这些数据

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb0eab6da8ef4ab2b3745daa9a703fa97.png)

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

## 获取请求参数信息的方法

我们发送请求的时候往往会带有一些数据，那些数据其实就是参数信息，比方说，我们传入的账号密码那些的，我们当然也有获取这些参数信息的方法，请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf74c38778fe3c1e25c2eba5f9675a22d.png)

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

## 获取参数手动封装对象

现在我们已经能够获取到我们的请求参数的数据了，如果我们要把参数传给其他类的话，我们当然可以构造一个函数将其各个数据传入，但是当数据量小时，这个方法才管用，如果数据量一多，那么这个方法就很麻烦了，那么我们有什么方法能够简化这个传入数据的过程吗？

当然有，我们可以创建一个对象，对象内部存放我们的参数数据，然后直接将这个对象传过去就行了。这个过程我们称之为获取参数并封装，封装一共有三种方式，分别是手动封装、反射封装和工具类封装。我们这里先讲第一种方式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2f349ce655f594a51f6219dd523ef76b.png)

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

## 获取参数反射封装对象

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

## 获取参数工具类封装对象

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

## 流对象获取数据

以前我们获取参数信息都是通过req对象的各种方法来获取的，但实际上我们还可以通过流对象来获取，但是要注意，这种方式是支持post方式，如果是get方式，则是不支持的。我猜测这是因为归根结底我们的流读取的还是方法体内的内容，而get方式是没有方法体的，自然就无法使用。

我们用流对象获取数据总共有两种方式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE55f9c25c869bef3a98ba4d09063211b5.png)

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

## 中文乱码问题

我们如果输入中文，那么在控制台上会出现乱码，现在我们就来解决一下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE062118178940cc89248540bc6a55d2cc.png)

虽然图上是这么说的，但实际上我们的idea无论是哪种方式都是乱码，干

如果采用post方式又需要控制台输出正确的代码，可以在类的起始位置写入req.setCharacterEncoding("UTF-8");将编码格式设置为UTF-8，当然这里之所以设置为UTF-8而不是gbk，这主要是因为我们的html文件里的字符串的格式就是UTF-8

## 请求域的介绍

请求域也就是request域，可以在一次请求范围内共享数据。一般用于请求转发的多个资源中共享数据。

其应用场景非常简单，比如说一个客户端向ServletA发送了一个请求，而ServletA无法实现这个请求，只有ServletB能实现，那么一个简单的想法就是让ServletA将请求发送给ServletB，然后让ServletB去实现这个请求。这种行为的专有名词描述是请求转发，而ServletB在响应客户的请求时可能要用上请求的数据，此时就需要共享数据了，这时候就要用上请求域，之所以是用请求域而不是用应用域的原因是，应用域太大了，请求域的大小就正适合。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEea45c4fb31b834225e564599247c924d.png)

其令对象获得和修改请求域中共享数据的方法和应用域的其实大同小异，这里就不再赘述了

## 请求转发

请求转发有以下几点是我们需要注意的：1、浏览器的地址栏不会变。2、域对象的数据不会丢失。3、负责转发的Servlet转发前后的响应正文会消失。4、由转发的目的地来响应客户端。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc1008738d682418b63620e958051d8dc.png)

获得共享数据的方法上一章已经介绍过了，这里我们来介绍将数据共享的方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd306e7a27b33a63812f88c198db4f2d0.png)

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

## 请求包含

什么是请求包含呢？其实请求包含的内容与请求转发有很大相似，但又有所不同。请求包含简单来说就是可以合并其他Servlet中的功能一起响应给客户端，比方说用户向ServletA发送了一个请求，但是这个请求ServletA无法独立完成，还需要ServletB来帮助完成一部分，这时候就需要用上请求包含。其实请求包含简单来说就是两个Servlet的功能都会执行一次，这是最简单的理解

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8ccfbe6abab44478658f4c4335f79750.png)

请求包含有三个特点，分别是1、浏览器地址栏不变。2、域对象的数据不丢失。3、被包含的Servlet响应头会丢失。与前面的请求转发一样，其也有对应的方法，我们来看看

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE046721ea2de04b6d4220ca49648e5890.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE92eb769f8797cd579770d92a320ec4ba.png)

关于请求转发和请求包含的异同可以参考这个网址https://blog.csdn.net/qq_39740279/article/details/99631511

简单来说，请求转发由被转发的功能类完成响应，而请求包含则是被转发的功能类和被转发的功能类共同完成响应。至于什么时候用请求转发，什么时候用请求包含，这个目前还整不明白

# 响应对象-response

那么在学习完请求对象之后，我们本章来学习响应对象，要学习响应对象，当然得先知道什么是响应对象，下面我们就正式开始关于响应对象的学习

## 响应对象的介绍

响应对象其实就是回馈结果给浏览器，其他的内容和请求的时候讲解的大差不差了，这里就不赘述了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf2bfdb518281a65389bbac2c7b5e7d1b.png)

## 常见状态码的介绍

其实状态码有很多，但是我们不需要记这么多，我们只需要记一些比较常用的就可以了，下面列出了常用的状态码

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0a7a23290ca19114b577f531cc2bd14b.png)

首先是200，代表成功，这没啥好说的。接着是302，代表重定向，关于重定向的知识我们后面会讲解到，其跟请求转发很相似，不同的是其状态栏是会改变的。然后是304，代表请求的资源没有改变，直接使用缓存数据展示给用户。然后是400，其代表请求错误，这个常见于请求的参数错误。然后是404，这个可太熟悉了，这就代表请求资源没有找到。然后是405，其代表请求的方式不受服务器支持。最后是500，其代表服务器错误，服务器宕机了或者停机了都会报这个错误。

我们可以以数字开头来简单记忆这些状态码，如果是1为开头的，说明都是些简单的消息，我们不需要去理会。2开头的代表的就是响应成功。如果是以3开头的就代表是重定向或者是使用缓存。如果是以4为开头代表的是都是些客户端的错误。如果是以5开头的就代表是服务器的错误

最后我们可以看一份完整版的状态码

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7d756a7c268f3602424a59854d36d452.png)

## 字节流响应消息

那要使用字节流响应消息，当然首先就得获得字节流，我们这里获得字节流的方式和请求里的大同小异，是通过响应对象获得字节流。同样是继承了普通字节流，同样不必手动关闭，会自动关闭

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3947a932c5cfec8807772ea52d827a1d.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE273637710a0468d7048b5b2bf898bc3e.png)

我们第二个方法setContentType可以设置响应内容类型来解决中文乱码问题，我们只要在我们的代码开头加上

```
resp.setContentType("text/html;charset=UTF-8");
```

就可以解决乱码问题了。这里前面的text/html表示的是我们的响应的内容的类型，这里我们可以看到我们响应的内容是text文本串，而且是html的。后面的则是我们指定的编码格式。使用这个方法会让浏览器采用我们所指定的编码格式进行编码，其作用就相当于是告知浏览器要使用怎样的编码格式。

## 字符流响应消息

上面我们学习的字节流是万能响应流，其不但能响应文本，也能响应图片一类的数据。但如果我们单纯是想响应文本的话，我们也可以用上字符流来响应

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE68d64eaf286f617b106340aaf2f981c5.png)

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

## 响应图片

之前我们都是在响应文本，那么现在我们来试着响应一张图片回去。要响应图片当然我们得要使用字节流，我们还要事先指定一个字节输入流，字节输出流则由响应对象获得，我们来看看整体步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE24322d47f4305da2bd64224eea9bc57c.png)

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

## 设置缓存时间

为了提高我们服务器的运行效率，我们可以设置缓存时间，让用户端在请求一次之后，一定时间之内会直接从缓存文件中获取想要的资源，而不是从服务器中再次请求获得。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1de8d31fe14762c1bb138f920cbe84e6.png)

如果我们想要设置一小时的缓存时间，我们应该要指定一个字符串，是英文单词过期，不过实际上这个单词可以随意指定，并没有影响，我们这里特别指定为英文单词的过期主要是因为我们要考虑到其他人要读得懂，然后我们要先获取系统的当前时间，然后加上一小时的毫秒数，这样就可以构成其过期时间。值得一提的是，我们这里的方法是响应对象里的方法，最后我们来看看其代码

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE027035f6ec47b1100e3ee989f9507474.png)

## 设置刷新时间

我们可以设置刷新时间，让我们的用户在浏览器中点进入某个选项之后就会自动跳转到某个界面，这个我们见得多了，现在我们就来实现下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb1010739e4e7513abae131a1dbfae3cc.png)

这里同样也是响应对象的方法，消息头名称可以随意设置，但是最好还是设置成刷新的英文单词，然后比较就讲究的是第二个参数value，value要设置的格式是s;URL=/xxx/xxx，这里s代表的是秒数，如果我们希望我们的界面三秒后刷新，那么我们就应该将其指定为3。同时我们要跳转的界面的地址应该是虚拟路径结合实际路径的方式，我猜测之所以这样设置的理由是其内部对字符串进行了截取，然后进行了特殊的处理。因此我们这样设置就可以了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb4a17a8a8af6553fc4c77349dde2965a.png)

## 请求重定向

那么现在我们就来学习请求重定向，所谓请求重定向的意思是，当客户端的一次请求到达之后，发现需要借助其他的Servlet来实现功能，此时可以使用重定向。这与请求转发的作用是非常类似的，但是不同的是，请求重定向时，浏览器地址栏会发生改变，共发生两次请求，请求域对象中不能共享数据且可以重定向到其他服务器。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE12108e23b2fea33a14d392080d7f4689.png)

重定向的实现原理时先设置响应状态码，然后设置响应的资源路径，同样要设置消息头和消息地址。不过在响应对象里提供了更加简便的重定向方法，其就是sendRedirect方法，该方法可以设置重定向，我们就使用这个方法来完成重定向。那么我们可以构造第一个代码如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEcfa0d32691b0568cc9948000a06d5e9d.png)

在这个代码里我们有一个执行的测试语句，然后我们在请求域里设置了共享数据，最后我们设置了重定向，这里我们设置的重定向的地址是我们的虚拟目录加上我们的指定的具体的类，我们这里是采用方法动态获取我们项目的虚拟目录的方式。然后是一个直接重定向到百度首页的一个语句，主要是用于测试的。然后我们来看看另外一份代码

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf038eb3e3e492f4e06465080a4d9640f.png)

这份代码里我们先测试其是否执行，然后尝试去获取共享数据，看看能否获取到。

最终我们的结果当然是两个都执行了，但是第二个地址无法获取到共享数据，这是因为重定向总共有两次请求，两次请求导致了请求域是发生变化了的，自然无法获取。同时由于是两次请求，所以两份代码的功能都会执行。最后由于是两次请求，因此我们的地址栏最后会变成第二个代码的地址，其本质过程是第一个代码执行了之后发生了重定向，向第二份代码发送了请求，此时浏览器跳转到第二份代码的地址执行第二份代码的功能

最后我们来解决一个问题，就是重定向和请求转发如此相似，那么我们应该怎么确定使用哪一个呢？我们只要看需不需要用得到共享数据就行了，如果用得上就用请求转发，用不上就用重定向

## 文件下载

接下来我们来学习我们响应对象的最后一个内容，就是文件下载，我们希望实现用户能够从我们的网址里下载我们想要的东西的功能。先来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEab666fdcc10451fec6f295579931b469.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE453af1d8d7d3d8b8e7d5b320a1a0d0b2.png)

# 学生管理系统(中)

到了这里，说明我们已经学习完了请求和响应了，那么接着我们继续来实现我们的学生管理系统

我们先来看看案例效果

## 案例效果

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE75ce11f4779fc3b95567798884ab0dc5.png)

我们的案例效果是上面这样的，首先我们的首页就是两个简单的超链接，然后当我们点击添加学生时，会进入到添加学生的界面，当我们添加完毕后会显示添加成功并返回首页，我们可以点击查看学生，其会显示对应的数据到界面上。由于我们目前还没有学习过将数据直接添加到Html中显示，因此我们这里的数据显示的方法是直接读取数据内容然后显示出来的

## 资源准备

那么确定了上面的内容之后，我们接下来来分步实现我们的学生管理系统，请看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE96b9ca195bb71afd96d12fc970ce83dd.png)

我们首先要创建一个首页的HTML文件，我们可以将其创建如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>学生管理系统首页</title>
</head>
<body>
    <a href="/stu/addStudent.html">添加学生</a>
    <a href="/stu/listStudentServlet.html">查看学生</a>
</body>
</html>
```

然后我们创建添加学生的页面，其代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>添加学生</title>
</head>
<body>
    <!--提前通过Run Edit设置其虚拟目录为stu，此处先写入虚拟目录，然后写入构造的类，就可以将数据传入到指定的类-->
    <form action="/stu/addStudentServlet" method="get" autocomplete="off">
        学生姓名：<input type="text" name="username"> <br/>
        学生年龄：<input type="number" name="age"> <br/>
        学生成绩：<input type="number" name="score"><br/>
        <button type="submit">保存</button>
    </form>
</body>
</html>
```

然后我们还要创建我们的学生类，给其提供对应的方法，其代码如下

```
package com.itheima.servlet.bean;

import java.util.Arrays;

public class Student {
    private String username;
    private int age;
    private int score;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public int getScore() {
        return score;
    }

    public void setScore(int score) {
        this.score = score;
    }

    public Student() {
    }

    public Student(String username, int age, int score) {
        this.username = username;
        this.age = age;
        this.score = score;
    }
}

```

那么上面我们就实现了最基本的首页的界面，接下来我们来实现添加的功能，先来看我们的实现步骤

## 添加功能实现

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbbf6ca7019a9b17b99b8465a709fc06e.png)

那么我们按照步骤，先创建AddStudentServlet类，然后可以写入代码如下

```
/*
    实现添加功能
 */
@WebServlet("/addStudentServlet")
public class AddStudentServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.获取表单中的数据
        Map<String, String[]> map = req.getParameterMap();

        //封装学生对象
        Student stu = new Student();
        try {
            BeanUtils.populate(stu,map);
        } catch (Exception e) {
            e.printStackTrace();
        }

        //3.将学生对象的数据保存到d:\\stu.txt文件中
        BufferedWriter bw = new BufferedWriter(new FileWriter("d:\\stu.txt",true));
        bw.write(stu.getUsername()+","+stu.getAge()+","+stu.getScore());
        bw.newLine();
        bw.close();

        //4.通过定时刷新功能响应给浏览器
        resp.setContentType("text/html;charset=UTF-8");
        resp.getWriter().write("添加成功，2秒后自动跳转到首页");
        resp.setHeader("Refresh","2;URL=/stu/index.html");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

我还就那个调用我牛逼的工具类，直接起飞。

## 查看功能实现

那么最后我们来实现我们的查找功能，由于我们还没有学到如何将文件中的数据以HTML界面的形式展示出来，因此我们这里采取展示数据的方案就是恩读取恩访问，请看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEcc87d270bb518b47181aaaedb816796e.png)

那么我们创建ListStudentServlet类，并写入代码如下

```
package com.itheima.servlet.Servlet;

import com.itheima.servlet.bean.Student;
import org.apache.commons.beanutils.BeanUtils;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.*;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;

/*
    实现查看功能
 */
@WebServlet("/listStudentServlet")
public class ListStudentServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.创建字符输入流对象，关联读取的文件
        BufferedReader br = new BufferedReader(new FileReader("d:\\stu.txt"));

        //2.创建集合对象，用于保存Student对象
        ArrayList<Student> list = new ArrayList<>();

        //3.循环读取文件中的数据，将数据封装到Student对象中。再把多个学生对象添加到集合中
        String line;
        while ((line = br.readLine()) != null){
            //张三,23,95
            Student stu = new Student();
            String[] arr = line.split(",");
            stu.setUsername(arr[0]);
            stu.setAge(Integer.parseInt(arr[1]));
            stu.setScore(Integer.parseInt(arr[2]));
            list.add(stu);
        }

        //4.遍历集合，将数据响应给浏览器
        resp.setContentType("text/html;charset=UTF-8");
        //获取输出流对象
        PrintWriter pw = resp.getWriter();
        for (Student s: list) {
            pw.write(s.getUsername() + "," + s.getAge() + "," + s.getScore());
            pw.write("<br>");
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

那么到此为止，我们学生管理系统的第二个部分就实现完毕了。

# Cookie

那么现在我们就到了下一章节了，在这一章节里，我们一共要学习三个技术，分别是Cookie、Session和JSP。其中Cookie和Session是会话相关的技术，而JSP是界面相关的技术。我们本章学习Cookie

## 会话技术介绍

当然，在学习Cookie之前，我得先搞明白什么是会话技术。会话其实就是浏览器和服务器之间的多次请求和响应。为了实现一些功能，服务器和浏览器之间是可能产生多次请求和响应的，这期间产生的多次请求和响应加载一起就成为一次会话。

而会话过程中产生的一些数据，可以通过会话技术Cookie和Session保存，我个人觉得这似乎有点像缓存

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE574bc73cee6ffb2c9627fb5d83296b4e.png)

## Cookie介绍

首先Cookie是客户端的会话管理技术，其会将要共享的数据保存到客户端，客户端每次请求时，就将会话会话信息带到服务器端，从而实现多次请求的数据共享。其作用简单来说就是可以保存客户端访问网站的相关内容，从而保证每次访问时都先从本地文件中获取，以此来提升效率。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE84c42d6c8abd56978b1be6d3e918fdf8.png)

下面我们来看看Cookie在帮助文档中的介绍

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb18eaad66b481e8c62a1e91cbbf77338.png)

这里我们要记住的是Cookie就是一个普通的java类，其次是一个cookie包含有名称和属性这两个最重要的值，其他的属性类似于路径、域名当然也有，但属于是可选内容，相对没那么重要。然后是浏览器将Cookie带回到服务器端利用的是HTTP协议中的请求消息头的方式，通过调用请求对象中的addcookie方法。通过HttpServletRequest对象的getCookies方法可以获取到当前网站中的全部Cookie。几个Cookie可能具有相同的名称，但是路径属性不同，换言之，确定唯一的Cookie不能只能名称

## Cookie属性

Cookie中的属性是非常重要的，因此我们来着重讲解

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEadb2fa7ebb083785c2728c868a83699e.png)

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

## Cookie的方法的添加获取

学习完了属性之后，我们现在来学习Cookie类中对应的方法，请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc98cfab5be6f25bec169fb4fb4aa434a.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc225dc4da5b4d5910fc5766786bda89e.png)

其向客户端添加Cookie方法是在HttpServletResponse中利用addCookie方法，传一个Cookie对象就可以将该对象传到客户端。而获取Cookie方法是利用HttpServletRequest中的getCookies方法，获得一个Cookie数组，可以将客户端中所有的Cookie获取

其实这个也非常符合我们的一般想法，在服务器的响应中，我们将Cookie传给客户端，在服务器的请求中，我们获取里面的所有Cookie

## Cookie的使用

关于Cookie的使用，我们决议通过Cookie的一个案例实现来达到学习的目的。先来看看我们的需求以及我们的实现步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE019041d927404eecdbf08dce951e7614.png)

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

## Cookie使用细节

学习完了Cookie的使用之后，我们接着来学习的Cookie的使用细节

首先Cookie是有数量限制，每个网站最多只能有20个Cookie，且大小不能超过4KB。所有网站的Cookie总数不能超过300个。

其次是Cookie的名称只能包含阿斯克码表的字母、数字字符。不能包含逗号、分号、空格，不能以$开头，且其值不支持中文

然后是关于setMaxAge()方法接受数字，如果接受的是负整数，则浏览器关闭时就会清除Cookie。如果是0就立刻清除，浏览器一接受到Cookie就立刻清除了。如果是正整数则是会以秒为单位设置存活时间

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3510d37e46e4dde5ced1952fae94f32c.png)

最后是关于访问路径限制的细节，默认路径是取自第一次访问的资源路径前缀，后续的访问路径只要有这个路径开头就能访问到。比如说，我们映射了servlet/servletdemo01，并传入了Cookie。然后写入servlet/servletdemo02，获取cookie，然后写入servlet/aaa/servlet555，同样获取cookie，最后写入aaa/servlet888，同样是获取cookie。那么最后只有最后一个获取不到Cookie，其他的都可以获取到，这是因为最后一个没有吸入对应的第一次访问资源路径的前缀导致的

# Sesssion

学习完了Cookie之后，我们现在来学习Session，其也是一个会话技术。现在介绍下什么是Session吧

## HttpSession的介绍

首先HttpSession是服务器端的会话管理技术，我们之前学习的Cookie是客户端的会话管理技术。不过其本质也是采用客户端会话管理技术，只不过在客户端里保存的是一个特殊标识，共享的数据保存到服务器端的内存对象中。每次请求时就会将特殊标识带到服务器端并通过这个标识找到对应的内存空间，从而实现数据共享

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEaa8d3c55af57bf765bd4aac1a9534e63.png)

其也是Servlet规范中四大域对象之一的会话域对象，可以在当前会话范围之后实现数据共享。其本身也是一个接口，但是Tomcat会为我们提供实现类，我们直接用就可以了，且在其内部里也有很多我们以前学习过的对应方法，我们后续会对其进行讲解

## HttpSession的常用方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa15fcdcac820aa364064ca8ed0198843.png)

上面的方法都很简单，而且很多我们都已经用过了，这里就不单独去演示了，就这样吧。

## HttpSession的获取

我们要获取到HttpSession，自然要了解其获取方法，我们不妨来看看

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf8c653a2f00f2bd67cf8aa1e78bb5bb6.png)

这里有两个方法，第一个getSession方法的作用就是用于获取HttpSession对象，第二个getSession的重载方法是可以传入一个布尔类型的值，确定如果我们没有获取到对应的HttpSession对象时，要不要自动创建。

这里值得一提的是，第一个方法的默认值是为true的，也就是说，如果不特殊指定，那么其如果没有获取到对应的对象，是会自动创建一个对象的

要了解第二个方法的内部机制，我们需要来看流程图，请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE00eed9c66f4833f318308ca225662576.png)

我们先从客户端中调用getSession方法获得其唯一标识，先寻找是否携带有JSESSIONID的值，如果有，即根据这个id在服务器端查找有无对应的HttpSession对象，如果有，那么直接在服务器端的内存空间中找到这个对象并继续使用

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEdd2743de05d0495ba88224730adf3f51.png)

接着我们讲没有找到对应对象的情况，如果没有找到的话，那么其就会创建新的HttpSession并分配唯一的表示JSESSIONID并放置于服务器端的内存空间中



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb7754f930b5737addb83cfae2fa868f4.png)

但如果一开始都查找不到携带JSESSIONID的值，那么其就会创建一个新的HttpSession对象并分贝唯一标识，然后将该对象放置于服务器端，并将这个唯一标识一并发送到客户端

## HttpSession的使用

我们这里同样是通过一个需求来学习HttpSession的使用，请看下图的需求分析和实现步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd1844b0ee0c6c6cc53b4a10a78df36d9.png)

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

## HttpSession的细节

那么最后我们照例来讲讲HttpSession在使用上的细节，首先是关于唯一标识的查看

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEcb80959897d410ec9928c959d60331fc.png)

我们可以在浏览器中的检查上的查看消息头的方式来查看我们的唯一表示，由于Session的本质也是Cookie，因此是在Cookie一栏上查看

其次是浏览器其实是可以禁用Cookie的，禁用Cookie之后会对正常使用造成影响，我们的一个解决方式就是判断用户有没有禁用，若禁用则弹出对应的提示信息，告知用户不要禁用。比如我们可以在服务器端获取Session的语句下，判断获取的Session是否为空，若为空则弹出如下的提示信息

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa353d13ce94105c64f8a5f029c8dd6e7.png)

一般来说是要用弹窗来提示的，但是我们还没学到这，因此这里就先这样吧

还有一种方式是拼接URL，不过那种方式不方便，这里就不展示了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE706184c11eace4c95ac06c1fc195c2af.png)

最后是关于钝化和活化，这个具体的内容自己看吧，这里不提了。

# JSP 

照例我们先来介绍下什么是JSP

## JSP的介绍

首先JSP的全称是Java Server Pages，是一种动态网页技术标准。其次是JSP部署在服务器上，其不但可以处理客户端发送的请求，并且还可以根据请求内容动态生成HTML、XML文件等Web网页再响应给客户端。我们之前学习过Servlet也是用于处理客户端发送的请求并返回给客户端的，而这个也可以，这说明了什么？这说明了JSP的本质，其实就是Servlet

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa266f4b3aa0a798a0f80890b33eb9142.png)

最后再上图里我们还复习了我们曾经学习过的技术和未来要学习的相关技术，自己去看吧

## JSP快速入门

接着我们就来学习我们的JSP，我们首先创建一个JSP的新项目，具体请看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa45119e8426e507c9cde410106e8498b.png)

其实我们一创建完我们就可以直接看到其已经帮我们生成了index.jsp文件了，其代码如下

```
<%--
  Created by IntelliJ IDEA.
  User: 22592
  Date: 2022/3/17
  Time: 13:37
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  $END$
  </body>
</html>

```

可以看到这个代码跟我们以前学习的HTML文件不能说十分相似，只能说一模一样。最上面的是注释信息，这个可以不用管。然后我们看到第8行，可以看到contentType="text/html;charset=UTF-8"，这个跟我们之前学习的指定编码格式的代码不谋而合，那么现在我们也应该能明白为什么我们指定编码格式的字符串总是统一格式了的，因为页面只能这样读取，我们的方法那样写最终会在页面将这个语句交给浏览器识别并执行

我们将我们的代码改造如下

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>JSP</title>
  </head>
  <body>
  我还就那个永失吾爱，举目破败
  </body>
</html>
```

如果是以前的话，我们可以直接通过浏览器来查看我们的HTML文件的效果，但是我们这个是JSP文件，要查看的话必须要通过服务器响应的方式来查看，因此我们要启动Tomcat服务器，然后打开这个文件来查看

最后查看我们发现的确有这个效果，那么我们的案例就成功了

## JSP执行过程

那么为什么我们的JSP文件不可以直接执行呢？而一定要通过Tomcat服务器发布文件的形式来可以查看到呢？要解决这个问题，我们就必须分析JSP的执行过程了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc593aab869a5e82d91ee7979cb6c553e.png)

首先，我们的客户端浏览器向Tomcat服务器发起请求，服务器接收到请求之后会解析请求地址然后找到具体应用，然后其会找到jsp文件，但是jsp文件本身啥也干不了，因此其会将jsp文件翻译成java文件，然后运行该java文件生成字节码文件，接着将该字节码文件响应给客户端浏览器，最终我们就可以浏览器上看到我们的页面效果。这里翻译和编译过程，都是Tomcat帮我们做的，我们不用去理会。

## JSP生成的文件内容介绍

为了更近一步的去了解JSP，我们现在来查看下其翻译的JSP的java文件的内容，一打开对应的java文件，我们就可以看到下面这个内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa7b49334333a922fa9695fc5af271363.png)

可以看到我们翻译之后的jsp文件是继承了一个叫HttpJspBase的类的，我们接着再来查看下这个类

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe75d1b943db79440f0c89dedcd5d795f.png)

我们可以轻易地发现这个类其实是继承了HttpServlet类的，所以说其实我们的JSP本质上还是一个Servlet，因为其也继承了HttpServlet，下面当然也有HttpServlet的方法，而且还要有一些它自己定义的方法。不但如此，我们回到原来的java类中继续往下看，还可以看到其也有初始化，摧毁以及service方法，这更加说明了其本质就是一个Servlet

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc6c18cdff73f0ac9397efde5c3a6cb4e.png)

当然上面的不算太重要，真正重量级的还是下面的内容，我们一起来看

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5d78d4af5693922b15e96c3550a14844.png)

我们可以看到，这个代码里，先指定了编码格式，然后获得了各种域对象，接着其获得了一个输出流，直接向浏览器内响应了当初我们写出的内容，这就是为什么我们能够在浏览器里看到我们的写入的内容的原因，因为其帮我们响应过去了

最后我们可以做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8fd250107934ec1efc9500eb3a184201.png)

## JSP的语法

接下来我们来学习JSP的语法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE99d5219750a75429d04527df3587ff72.png)

我们可以构造测试代码如下

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>JSP</title>
  </head>
  <body>
    <%--
        1.这是注释
    --%>

    <%--
        2.java代码块
        System.out.println("Hello JSP");普通输出语句，输出在控制台
        out.println("Hello JSP");
            out是JspWriter对象，输出在页面上
        如果想要在页面上实现换行，使用Printlin语句是没有用的，必须在字符串
        结尾处加上<br>
    --%>
  <%
    System.out.println("Hello JSP<br>");
    out.println("hello JSP<br>");
    String str = "hello<br>";
    out.println(str);
  %>

  <%--
      3.jsp表达式
      <%="Hello"%>  相当于 out.println("Hello")
  --%>
  <%="Hello<br>"%>

  <%--
      4.jsp中的声明(变量或方法)
      如果加！代表的是声明的是成员变量
      如果不加！代表的是声明的是局部变量
      方法必须定义在！中，否则则是试图在方法内部定义方法，编译无法通过
  --%>
  <%!String s = "abc";%>
  <%String s = "abc";%>
  <%=s%>
  <%! public void getSum(){}%>
  </body>
</html>

```

知识点的相关内容都写在注释里了，因此这里就不再赘述了

## JSP的指令

学习完了JSP的语法之后，我们现在来学习JSP的指令，具体请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9dcc8b65a1d7f76205bf0f44ed2e61f8.png)

可以看到有点小多啊，但是没关系，因为大多数指令我们的服务器都给赋予一个初始的值，我们哪怕不关心也没有问题，不过我们这里还是演示两个代码，一个是errorPage，一个是import

那么我们可以构造测试代码如下

```
<%--
    1.page指令
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" errorPage="/error.jsp" import="java.util.ArrayList" %>
<html>
  <head>
    <title>JSP</title>
  </head>
  <body>
      <% int result = 1 / 0; %>
      <%ArrayList list = new ArrayList<>();%>
  </body>
</html>

```

这个error.jsp我们已经预先创建了，不过没写什么代码，这里不再放一遍了，因为没什么意义。

接着还有一个include指令和taglib指令，前者的作用是引入其他界面，然后可以调用其他界面定义的变量。后者是可以引入外部标签库，然后可以使用哪些标签。这里我们就不做演示了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbb863202fe96456102335a4edda4dcdd.png)

## JSP的使用细节

JSP的使用里有九大隐式对象，他们的名称及其实际代表对象如下图所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5dcf22f2d9a56b5e3c1612d4ff7d55f5.png)

这里我们要着重讲下pageContext隐式对象，因为他是四大域里面的最后一个，就是页面域。

首先PageContxt是JSP中独有的，且不但可以操作其他三个域对象中的属性，还可以获取其他八个隐式对象。其生命周期随着JSP的创建而存在，也随着JSP的结束而消失，并且每一个JSP页面都有一个PageContext对象

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc30807bc587092289896fa5943c51ff2.png)

最后我们可以来看看PageContxt对象的方法详解

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd62391aa2d5af11c3fd3c18c2f4c3c3f.png)

## 四大域对象的复习

接下来我们对我们四大域进行一个复习。我们的四大域共有PageContxt（页面域，范围最小）、ServletRequest（请求域）、HttpSession（会话域）、ServletContext（应用域）。这四大域的区别和级别都在下图中有讲解了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE081949f6cfcd0b2a29620e20f9e7e2d9.png)

## MVC模型的介绍

接下来我们介绍一种比较经典的一种业务开发模型，MVC模型。所谓MVC模型，M指的是Model，也就是模型，用于封装数据模型。而V代表view，意为视图，用于显示数据，动态资源用JSP，静态则是用HTML。最后是C，其为Controller，意为控制器，用于处理请求和响应，例如Servlet

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa3e59cab469e4469feb96c57596d15e1.png)

上图中我们的DB代表的是数据库，MVC的流程是先从客户端浏览器向控制器发起请求，然后控制器将请求通过模型进行业务处理，然后模型通过数据库进行数据处理，然后数据库返回数据给模型，接着模型返回数据模型给控制器，控制器会通过数据模型选择合适的视图来展示模型，最后返回响应给客户端浏览器。

这个模型的最大好处当然就是降低耦合度了，这个大家都懂，不多说了











