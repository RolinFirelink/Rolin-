# SpringMVC的入门案例

本章节我们来学习我们的三层框架里的最后一个内容，SpringMVC。

## SpringMVC概述

首先我们来先讲下什么是SrpingMVC，这就要先从我们的三层架构中讲起了，我们传统的开发里有三层架构，分别是数据层、业务层以及表现层

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9e4ff858e762f21827b0b9473ddf0344.png)

我们的数据层的技术，我们学习过的有JDBC、MyBatis以及Spring，业务层目前则只学习过Spring，而在表现层中，我们则有Servlet、HTML...以及Spring

而今天我们要学习的SrpingMVC，其就是表现层的一种技术。首先我们来介绍下什么是MVC

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf610b63fc5c96a1f7334152b42800136.png)

首先MVC是Model View Controller的简称，是一种用于设计和创建web应用程序表现层的模式。其中M，代表Model，也就是模型，其是一种数据模型，用于封装数据。而V则代表View，意为页面视图，用于展示数据。而C则代表Controller，其为控制器，是处理用户交互的调度器，用于根据需求处理程序逻辑。

我们一般的需求响应的过程很简单，浏览器向客户端发送请求erquest，然后在客户端中，模型和视图由Controller进行组装并形成一个具有对应数据的视图然后进一步组装成响应对象response返回给浏览器。我们之前是使用Servlet帮我们完成这一项工作的，而现在我们学习的SpringMVC就是帮助我们做这一项工作的。

最后我们来看看其优点以及其简单的介绍

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd1cb7edf108103f352f0d08deade172c.png)

**注意SpringMVC是基于Java实现的一个轻量级的框架，不是这三者内容的简单相加**

## 入门案例制作

经过了好多天的摸鱼之后，终于重新开始学习了，这次摸鱼真的玩爽了，不过也大概有整整一星期没有对技术栈进行学习了吧。只能说这次五一假期是我又爽又痛苦的一次经历，另外这次五一的假期，足足让我花了一千有余，以后如果还想继续过这样的舒服日子，肯定要有足够多的钱，而这些钱都只能靠未来的我去自己赚来，所以我们更加是要去努力学习，好好读书，这样以后自己能赚大把大把钱，然后我们才有能力继续去过这种我理想中的好生活

然后这里我们之所以卡顿，有一点的原因是因为我们在导入依赖时 ，我们的依赖总是无法正确导入或者被正确使用而产生的问题，最后经过排查发现问题在于我们在一个项目中引入了太多的模块，而他们又是使用同一个资源库的，这就会不可避免地产生依赖冲突的问题，解决方案也很简单，新创建一个工程就完了。

那么在正式学习我们的入门案例之前，我们先来介绍下我们的入门案例的环境，首先我们来看看我们的pom文件的依赖

```
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>org.example</groupId>
  <artifactId>Test</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <name>Test Maven Webapp</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>

  <dependencies>
    <!-- servlet3.1规范的坐标 -->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
    </dependency>
    <!--jsp坐标-->
    <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>jsp-api</artifactId>
      <version>2.1</version>
      <scope>provided</scope>
    </dependency>
    <!--spring的坐标-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.1.9.RELEASE</version>
    </dependency>
    <!--spring web的坐标-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>5.1.9.RELEASE</version>
    </dependency>
    <!--springmvc的坐标-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.1.9.RELEASE</version>
    </dependency>

  </dependencies>

  <!--构建-->
  <build>
    <!--设置插件-->
    <plugins>
      <!--具体的插件配置-->
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.1</version>
        <configuration>
          <port>80</port>
          <path>/</path>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>

```

在我们的pom文件中，我们引入了我们的工程所需要的依赖，分别是servlet的，这些是必要的，而后面的一些则是我们制作案例所需要用得上依赖，分别是spring、springweb和springmvc的依赖，后续我们还引入了一个插件，这个插件就是tomcat7的插件，在该插件里，我们设置了对应的服务器的端口号80以及其虚拟路径，然后我们要使用该tomcat的话，就要用maven中的插件来使用tomcat服务，而不能直接用我们配置的tomcat服务

## Maven中使用Tomcat

如果我们想要使用maven中的tomcat服务的话，我们有两种方式，第一种方式是直接配置，我们先点击上面的启动方式的下选栏目，然后选择edit选项，然后进入对应的窗口，选择左上角的+号然后我们选择其中的maven，接着在对应的Command里选择我们需要启动的功能，这里我们选择启动tomcat服务器，就是tomcat7:run

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3db4557c7d576af73720ba91a99f5be5.png)

然后我们选择ok或者是apply之后就可以直接通过上面的播放符号开启对应的maven服务，来开启我们的tomcat服务了，非常方便。

我们的第二种方式是通过右边的maven，点击之后选择我们对应的模块，然后点击plugin然后选择对应的启动tomcat的功能就可以了。

然后我们创建了一个Uservlet类，该类完成的功能是接受一个请求，并打印一句话，然后往响应里添加一个新的页面地址，接着返回这个响应。这里是牵扯到了很久之前我们学习过的servlet的对应知识，实话我也忘得七七八八了，具体有什么不懂的回去重新复习吧

## 具体实现

```
package com.itheima.web;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class UserServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("user servlet is running...");
        req.getRequestDispatcher("success.jsp").forward(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}

```

我们顺便来看看我们的响应的界面

```
<%@page pageEncoding="UTF-8" language="java" contentType="text/html;UTF-8" %>
<html>
<body>
<h1>第一个spring-mvc页面</h1>
</body>
</html>
```

再来看看我们的默认界面

```
<html>
<body>
<h2>Hello World!</h2>
</body>
</html>

```

然后我们当然需要在对应的xml配置文件下进行对应的配置以及映射，具体代码如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE630e29e046ddd4b9491a5197e2331c1d.png)

然后我们启动tomcat服务，来看看效果，会发现我们的服务启动地非常成功，此时就说明我们的应用配置好了。然后我们就来进行我们的案例的制作了

首先我们创建一个UserController类并写入一个save方法，方法里就写入一个打印语句运行就执行这语句就完了，然后我们接下来就要将我们的之前UserServlet类中的功能搬到这里去并用springMVC来进行实现，当然我们这里也避免不了要进行对应的资源配置，首先我们要做的配置是让我们的扫描加载器所有的控制类，这里的内容其实跟我们之前的案例环境的代码是一样，实际上不做改动也可以，但我们的做springmvc时我们总是推荐将springmvc和spring分开，因此我们会创建一个springmvc.xml的文件，同时我们写入其代码如下

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">
    <!--扫描加载所有的控制类类-->
    <context:component-scan base-package="com.itheima"/>

</beans>
```

然后我们既然扫描了这里的所有控制类，接着我们要做的事情就是将我们要加载的类放置于springmvc容器中，所以我们在我们刚刚创建的UserController类中我们可以写入component注解，当然，我们这里为了合乎规范，我们这里使用其衍生注解Controller，作用一样，名字不同而已

接着我们要思考的事情是，我们将我们的类放置到springmvc里了，接着就要做对于请求的处理了，我们在之前的servlet中我们是通过令其实现HttpServlet的接口的方式以及通过配置文件的方式结合起来完成的处理，我们在springmvc中也是这样做，首先我们先来对我们的配置文件进行处理，这里我们处理的位置是webapp文件夹下的web.xml文件，我们写入其代码如下

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
        http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         version="3.0">

  <servlet>
    <servlet-name>DispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath*:spring-mvc.xml</param-value>
    </init-param>
  </servlet>
  <servlet-mapping>
    <servlet-name>DispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>

</web-app>

```

我们这里配置servlet标签，传入的class文件是我们的DispatcherServlet的全路径，这是我们的引入的依赖的某一个类的路径，然后我们取名为DispatcherServlet，接着我们在映射里也配置对应的名字，接着我们配置拦截的路径，我们这里配置全拦截，也就是所有请求都会经过这个拦截并执行我们所配置的方法，同时我们这里需要加载我们的springmvc-xml的文件，因此我们这里还配置了初始化的代码，具体体现在第11-14行

然后我们既然已经配置了对应的核心配置的代码，接着我们还有配置对应请求的具体处理的代码，这里我们直接进入实现类中，在对应的方法上加入RequestMapping注解，其代表我们的请求的地址，然后在括号内添加对应的访问名称，也就是我们在浏览器中输入的地址的名称，名称可以随便取，但是我们一般会添加和方法名一样的名字

```
package com.itheima.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
//设置当前类为Spring的控制器类
@Controller
public class UserController {
    //设定当前方法的访问映射地址
    @RequestMapping("/save")
    //设置当前方法返回值类型为String，用于指定请求完成后跳转的页面
    public String save(){
        System.out.println("user mvc controller is running ...");
        //设定具体跳转的页面
        return "success.jsp";
    }
}

```

然后我们要令其跳转到我们指定的页面，我们这里令我们的方法返回一个字符串，这个字符串就是我们的要跳转页面的名字，其会根据这个名字查到对应要跳转的页面并进入该页面

然后我们经过测试会发现这个程序是没问题的，这里值得一提的是，我们这里使用的服务器启动了是不会自动弹窗的，所以我们要手动进入对应的网址

那么到此为止我们的案例就制作好了，最后我们来讲解下这个案例的原理。

当我们的服务器启动的时候，其首先会先读取我们的web.xml，也就是核心配置文件，首先其会加载DispatcherServlet，其为springmvc的核心控制类，该类需要传入一个springmvc的配置文件才可以使用，关于这个类的原理我们先按下不表，我们在该配置文件里加载了所有的springmvc的bean类。

然后当其接受到一个save请求时，由核心配置文件下的mapping映射配置中知道其要过滤所有请求，那么其就会取走DispatcherServlet对象，然后去配置文件也就是spring-mvc.xml中配置的路径中去扫描看看哪个类的哪个被RequestMapping标注的方法下有对应的操作名，有的话就执行该操作名下的方法，然后按照return回来的字符串来寻找要通过要返回的页面，将该页面作为响应返回到用户的浏览器中。简单来说就是通过DispatcherServlet来进行控制，对所有请求进行拦截，然后结合RequestMapping注解来执行对应的请求处理

到此为止，我们的入门案例就制作完了，最后我们可以做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa2b1c0798fc68b77fcc2c8ec673801b1.png)

## SpringMVC技术架构图

接着我们就来具体讲讲我们SpringMVC的技术架构，关于这个架构，我们其实可以通过看病来进行类比，首先我们来看看看病的过程

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8091d1ed7fcdb22bf7bbe2cffc1b2226.png)

现在我们来讲解下springmvc的具体流程，首先是我们的请求发出到我们的前端控制器DispatcherServelt，前端控制器只负责指挥，其自身并不做具体的工作，真正执行工作的是我们的Handler，也就是处理器。我们的前端控制器接受到请求后想处理器映射器发送一个请求查询，查询该请求具体由哪个Handler来处理，然后我们的之前在请求中也学习过类似于过滤的操作，因此其会返回一个处理器链，将操作排成一排来让我们的过滤操作正确执行，实际上到了处理器的环节之后也可能存在其他操作需要处理，这时也依赖于我们的处理器链。然后我们前端控制器会向处理器适配器也就是HandlerAdapter发送一个请求执行，处理器适配器不是最终负责执行处理的内容，其工作是分配我们的请求到对应的处理器上去执行处理。然后其会返回两个由数据和视图组成的ModelAndView对象，到处理器适配器，适配器会继续返回到前端控制器。前端控制器接受到该对象后会将其发送到视图解析器ViewResolver，该对象会将对象解析成View对象，但是对象，用户是不能直接看的，最后还是由它来渲染视图，最后生成返回的jsp文件，然后通过响应返回给用户

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE07bd330f40df06b1c352dd8d50032deb.png)

最后我们可以将上面的图简化中我们的重点来，分别是前端控制器（DispatcherServlet）、处理器映射器（HandlerMapping）、处理器（Handler）、处理器适配器（HandlAdapter）、视图解析器（View Resolver）、view（视图）。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf937bae08248cc4d315737f8b78001a3.png)

最后我们这里只有我们的处理器映射器、处理器以及对应的视图是需要我们自己编写的，其他的内容我们都可以直接拿来用

# SpringMVC基本配置

那么学习完了入门案例之后，接着我们来学习我们对springmvc的的配置。

## Controller加载控制

首先我们要学习的是对我们的Controller的加载控制，我们如果要SpringMVC的处理器值按照规范的格式开发，我们就要避免加入无效的bean，这一点我们可以用过bean的加载过滤器来进行对应的包含设定或者是排除设定，因为我们表现层的bean标注通常设定为Contrller注解，因此我们可以通过包含注解来进行排除，我们这里进行排除代码设置的位置是在我们的核心配置文件spring-mvc.xml中，我们其实还有注解方式可以用于设置排除，但我们这里先演示使用配置文件来设置的方式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8020374b71ce3f38eb9f0c7e7d95e848.png)

我们可以写入排除的代码如下，直接在对应的扫描加载的控制类的代码下写其对应的子标签就行了，我们这里使用包含性过滤，因为用排除过滤的话要设置太多东西了

```
<!--扫描加载所有的控制类类-->
<context:component-scan base-package="com.itheima">
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

最后我们来看看其设置的步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe4fd9b0fb4a68e68f47c96fd071182a1.png)

然后我们接着要解决的是关于静态资源的加载问题，由于我们最开始设置的拦截是将所有请求都拦截，这样的话我们如果想要加载图片都不行，因为也会被拦截，因此我们可以设置对应的放行代码，将我们的图片资源一类的静态资源进行放行

## 静态资源加载控制

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa626b4dd5970a3d4e1083102acaca711.png)

那么按照这个思路，我们可以写入其放行代码如下

```
<mvc:resources mapping="/img/**" location="/img/"/>
<mvc:resources mapping="/js/**" location="/img/"/>
<mvc:resources mapping="/css/**" location="/img/"/>
```

但实际上我们还有更加简单的方法就可以对我们所有的静态资源进行放行，只需要写入下面这一行代码到核心配置文件中

```
<mvc:default-servlet-handler/>
```

这个了解下就行了，以后我们肯定都是直接写上这一行代码就完了的。当然，不要忘了我们这里也需要加入对应的命名空间。所以最后我们放出整个配置的所有代码

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <!--扫描加载所有的控制类类-->
    <context:component-scan base-package="com.itheima">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

<!--    <mvc:resources mapping="/img/**" location="/img/"/>
    <mvc:resources mapping="/js/**" location="/img/"/>
    <mvc:resources mapping="/css/**" location="/img/"/>-->

    <mvc:default-servlet-handler/>

</beans>
```

最后我们要解决中文的乱码问题，这个没啥特别值得说的，我们只需要配置一次就一劳永逸了

## 中文乱码处理

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEcb5a5b5c82360311495f8e64f470c365.png)

那么我们可以写入我们的中文过滤的代码如下

```
<filter>
  <filter-name>CharacterEncodingFilter</filter-name>
  <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
  <init-param>
    <param-name>encoding</param-name>
    <param-value>UTF-8</param-value>
  </init-param>
</filter>
<filter-mapping>
  <filter-name>CharacterEncodingFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```

## 注解驱动

那么接着我们就来做我们的注解驱动，简单来说就是使用纯注解形式来做我们的案例。这个我们了解下就可以了，因为我们实际开发的时候都是使用注解结合配置文件的方式的，很少使用纯注解来进行开发

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa738cceb49d6b0f99ecc0b057af3f443.png)

首先我们创建一个核心配置类，该配置下我们写入其头文件注解Configuration，然后我们写入其扫描路径注解ComponentScan，接着写入对应的过滤设置

```
package com.itheima.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.FilterType;
import org.springframework.stereotype.Controller;
import org.springframework.web.servlet.config.annotation.DefaultServletHandlerConfigurer;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
@ComponentScan(value = "com.itheima",includeFilters =
        @ComponentScan.Filter(type = FilterType.ANNOTATION,classes = {Controller.class})
    )
public class SpringMVCConfiguration implements WebMvcConfigurer {

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry){
        registry.addResourceHandler("/img/**").addResourceLocations("/img/");
        registry.addResourceHandler("/js/**").addResourceLocations("/js/");
        registry.addResourceHandler("/css/**").addResourceLocations("/css/");
    }

    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
    }
}

```

接着我们要实现对应的放开代码，这里我们要实现这个目的就要令其实现WebMvcConfigurer接口，其下的方法都可以不用去实现，但是我们如果要放开某些情况，我们就要通过重写其方法来达成目的。我们可以重写addResourceHandlers来进行独立的开放设置，也可以重写configureDefaultServletHandling进行将所有的静态资源都开放的设置，这两个保留其中一个就可以了

我们设置好这个就相当于是将spring-mvc.xml给取代了，接着我们要将web.xml给取代，那么我们可以新创建一个配置类，令其继承AbstractDispatcherServletInitializer类，该类就是对应我们的配置文件的实现类，我们写入其代码如下

```
package com.itheima.config;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.web.context.WebApplicationContext;
import org.springframework.web.context.support.AnnotationConfigWebApplicationContext;
import org.springframework.web.filter.CharacterEncodingFilter;
import org.springframework.web.servlet.support.AbstractDispatcherServletInitializer;

import javax.servlet.DispatcherType;
import javax.servlet.FilterRegistration;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import java.util.EnumSet;

public class ServletContainersInitConfig extends AbstractDispatcherServletInitializer {

    @Override
    protected WebApplicationContext createServletApplicationContext() {
        AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
        ctx.register(SpringMVCConfiguration.class);
        return ctx;
    }

    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

    @Override
    protected WebApplicationContext createRootApplicationContext() {
        return null;
    }

    @Override
    public void onStartup(ServletContext servletContext) throws ServletException {
        super.onStartup(servletContext);

        CharacterEncodingFilter cef = new CharacterEncodingFilter();
        cef.setEncoding("UTF-8");

        FilterRegistration.Dynamic registration = servletContext.addFilter("characterEncodingFilter", cef);

        registration.addMappingForUrlPatterns(EnumSet.of(DispatcherType.REQUEST,DispatcherType.FORWARD,DispatcherType.INCLUDE),false,"/*");
    }
}

```

第一个重写的方法就相当于是之前我们在servlet标签中配置我们的核心控制器类，然后中间引入了我们的另一个在注解配置文件并将其加载，然后返回加载好的对象到springmvc的容器中。

而第二个重写的方法就是指定我们要拦截的方法，由于我们要拦截所有方法，所以我们直接返回/就完了。

最后一个重写的方法是用于规定我们的对应的字符集的，用于解决中文乱码的，这个记住就挖了，以后都是一样的内容的

最后我们经过测试会发现其会报编码UTF-8的不可映射字符的警告，这个警告具体体现在我们的yongshiwuaijumupobai图片根本加载不出来，一下子找不到解决方法，反正后续也不一定还会用，这里就先放着吧

# 请求

本章节我们来学习springmvc的请求的内容，首先我们要学习的是在springmvc中如果要接收传入过来的参数我们应该怎么做

## 普通类型参数传参

首先我们来学习普通类型的参数传参的接收方法，如果我们想要接受对应的参数的话，只需要在对应方法内填入对应的传入参数，然后指定用户在地址栏里传入的参数名为当前的方法的参数的传入名即可

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE690e5b1a68a8bc42cb82d57e5f7cda35.png)

那么我们可以构造其代码如下

```
//方法传递普通类型参数，数量任意，类型必须匹配
//http://localhost/requestParam1?name=itheima
//http://localhost/requestParam1?name=itheima&age=14
@RequestMapping("/requestParam1")
public String requestParam1(String name,int age){
    System.out.println(name+","+age);
    return "page.jsp";
}
```

最上面还有该处理类的类名和设置其为spring的控制类的注解，这里为了版面省略掉了。

如果我们希望我们的参数即使在用户传入的参数名不同的情况下，我们设置的参数名仍然能正确获得对应的值，此时就可以使用RequestParam注解来完成目的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd9408778fe5b6f96e19452e21740317b.png)

那么我们根据上图，可以将我们的代码改造如下

```
@RequestMapping("/requestParam1")
public String requestParam1(@RequestParam(value = "userName",required = true) String name, int age){
    System.out.println(name+","+age);
    return "page.jsp";
}
```

这里我们在对应的参数参数中使用@RequestParam参数，我们可以设置value值为我们另外指定的参数名字，这样我们的名字即使不同，只要其和我们指定的另一个名字相同，其也会正确赋值，同时其下还有一个require值，该值默认为真，为真时则说明该参数不能为空，否则就报错

## POJO类型参数传参

所谓POJO类型参数，其实就是引用类型参数。我们要传入引用类型的参数的方式也很简单，我们只需要创建对应的实体类，当然我们的参数顺序要保持一致，然后传入给类就完了，当用户往里面传入指定的数据其会自动将这些数据封装成一个数据对象，我们只需要打印该对象即可

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4cdc1e43f16215c989928182c7c9ab92.png)

那么我们可以写入其代码如下

```
//方法传递POJO类型参数，URL地址中的参数作为POJO的属性直接传入对象
//http://localhost/requestParam3?name=Jock&age=39
@RequestMapping("/requestParam3")
public String requestParam3(User user){
    System.out.println(user);
    return "page.jsp";
}
```

利用该代码传入对应的参数就可以直接接收到我们所需要的User的数据对象了。

然后值得一提的是，如果我们的参数发生了冲突，即是当我们的传入参数中又有对应的属性同时有对应的引用数据类型且他们还重名的时候，引用数据类型和其他同名属性将会被同时赋值

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0f2b14bd43c74fd1cba45f01e758d2c5.png)

具体的解决方法就是使用RequestParam注解来进行区分，这种情况一般比较少出现，而即使出现了，一般也能通过对应的注解来进行解决

还有一种情况是当我们的POJO类型参数为复杂POJO类型参数时的处理，也就是当我们的POJO类型参数中还引用了其他的POJO类型参数时我们的赋值方式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6203def9d6c6a457a17e8dd2f400b028.png)

其实这个也很简单，在传值只需要直接通过给其对应的POJO类型参数通过.的形式进行赋值就可以了，原类型中的引用数据类型里的成员变量需要通过.的形式（也就是Address），而第一个引用数据类型的变量则不需要（也就是User），直接传就完了

```
//使用对象属性名.属性名的对象层次结构可以为POJO中的POJO类型参数属性赋值
//http://localhost/requestParam5?address.city=beijing
@RequestMapping("/requestParam5")
public String requestParam5(User user){
    System.out.println(user.getAddress().getCity());
    return "page.jsp";
}
```

当然，这里不能忘了，我们对应的实体类里也要做对应的修改，比如我们应该要新创建一个地址类并将该地址类成为用户类的一个成员变量，并且要提供对应的构造方法和set和get方法（这个后续就不再赘述了）

接着我们来演示POJO类型参数的集合形式，这个最简单的应用场景就在于一个人可能有多个外号，我们可以用一个集合去承载这些外号，对应的修改就是我们可以在对应的类中添加一个List保存字符串的集合，然后传值时这个值可以承载其所有的其他名字就可以了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2c2b4109922257515d079a2bbd84068c.png)

那么我们可以写入其代码如下

```
//通过URL地址中同名参数，可以为POJO中的集合属性进行赋值，集合属性要求保存简单数据
//http://localhost/requestParam6?nick=Jock1&nick=Jockme&nick=zahc
@RequestMapping("/requestParam6")
public String requestParam6(User user){
    System.out.println(user);
    return "page.jsp";
}
```

上面的代码里，我们打印之后会发现User其他的类型都是默认的初始值，但只有代表list集合的nick，是有我们插入的值的

接着我们还要讲另外一种集合形式，就是我们的集合内部本身可能是存放另一个POJO类型参数的，最简单的例子是外卖送餐时，可能要存放多个地址，此时一个外卖员下就应该一个存放地址的集合，其集合内有多个要送餐的人员的地址。此时我们需要对该集合进行赋值，就要使用对应的坐标.的形式来进行赋值，具体请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe3586be42c2c8bdf866c4a62b2471de7.png)

那么根据上图，我们可以构造我们的代码如下

```
//POJO中List对象保存POJO的对象属性赋值，使用[数字]的格式指定为集合中第几个对象的属性赋值
//http://localhost/requestParam7?addresses[0].city=beijing&addresses[1].province=hebei
@RequestMapping("/requestParam7")
public String requestParam7(User user){
    System.out.println(user.getAddresses());
    return "page.jsp";
}
```

最后还有一个Map集合的传入方式，使用该方式可以保证我们的输入对应的地址名就可以得到我们需要的具体地址，比如说输入home就只要家庭的地址。那么需要对其进行传值的话，我们修改的方式也就只是将[0]里的内容改为对应的字符串罢了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5fa8f8e3ce34559a330398940737a22d.png)

那么我们可以构造其代码如下

```
//POJO中Map对象保存POJO的对象属性赋值，使用[key]的格式指定为Map中的对象属性赋值
//http://localhost/requestParam8?addressMap['job'].city=beijing&addressMap['home'].province=henan
@RequestMapping("/requestParam8")
public String requestParam8(User user){
    System.out.println(user.getAddressMap());
    return "page.jsp";
}
```

## 数组与集合类型传参

如果我们要给数组类型参数传参，那么我们就只需要传入一个数组类型就可以了，地址栏上就用类似于集合的方式继续传就可以了，同时我们这里不需要在对象中特别声明一个成员变量

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa1c9a3a08f1e50604c39fe160987d2d3.png)

那么我们可以构造其代码如下

```
//方法传递普通类型的数组参数，URL地址中使用同名变量为数组赋值
//http://localhost/requestParam9?nick=Jockme&nick=zahc
@RequestMapping("/requestParam9")
public String requestParam9(String[] nick){
    System.out.println(nick[0]+","+nick[1]);
    return "page.jsp";
}
```

接着我们要将集合类型参数的内容，如果我们往参数里传入一个集合参数内容，如果我们传入的是接口，那么其报初始化错误，如果传入的是集合的实现类，那么就无法得到任何结果。之所以会如此是因为springMVC是默认将集合当做对象的属性处理的，因此其会先创建属性对象，因为接口无法创建对象，所以会报初始化异常。其次是我们传入实现类时即使不报出初始化异常，然而其仍然会将集合当做对象的属性进行处理，进行一个赋值操作，然而我们这里没有对应的属性，因此什么都不会有。如果我们希望告知springmvc我们现在要处理的内容是数据而不是属性，就需要使用@RequestParam注解并指定其名字为我们的传入参数名，此时其会通过该注解将大于一个的参数打包成参数数组给springMVC识别，然后springmvc才能做对应的相关动作

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd7fbdd72ce58a9e9c6b9b81bbd52123c.png)

那么我们可以构造其代码如下

```
//方法传递保存普通类型的List集合时，无法直接为其赋值，需要使用@RequestParam参数对参数名称进行转换
//http://localhost/requestParam10?nick=Jockme&nick=zahc
@RequestMapping("/requestParam10")
public String requestParam10(@RequestParam("nick") List<String> nick){
    System.out.println(nick);
    return "page.jsp";
}
```

## 类型转换器

本节我们来讲解我们的类型转换器，回顾我们的上一节内容，我们有说我们要给我们的指定的格式传入指定的内容，如果我们传入的内容是不合乎规范的，比如往int类型里传字符串，那么就会报出类型转换异常。实际上内部的众多的类型转换的工作，是由springMVC中的Converter接口来帮助实现的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd749e1bf26e696661909f7f7c02c04c8.png)

其下有许多转换器的实现类，这个我们知道就可以了。

假设我们现在要使用Date的数据类型，那么我们可以写入其代码如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE85bcfa856cc275380c48c26dc6fcb2a0.png)

然而使用这一份代码，我们就只能按照其规定的格式输入日期，否则会报类型转换异常。规定的格式就是使用/来进行对日期字符串的分割，实际上我们还可以用自己定义的转换类来进行对某一个对应的类型的转换，我们要实现这个目的，就要到springmvc的配置文件中去，写入代码如下

```
<mvc:annotation-driven conversion-service="conversionService" />

<bean id="conversionService" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
    <property name="formatters">
        <set>
            <bean class="org.springframework.format.datetime.DateFormatter">
                <property name="pattern" value="yyyy-MM-dd"/>
            </bean>
        </set>
    </property>
</bean>
```

我们这里指定了对应的工厂对应然后给其日期格式转换的属性赋值，赋值要采用set标签，因此我们使用set标签，赋值需要一个对应的日期转换类，因此我们在其下再使用bean标签生成一个日期转换类，然后给对应的日期转换类下的属性赋值，其实就是指定其格式。其他的还有许许多多的属性可以赋值，但是我们这里不做改动，好让他能按照默认值以继续使用。然后在上面开启注解驱动，使用我们的自定义的类型转换器，其名称就是我们下面的所指定的id

不过值得一提的是，一旦我们使用了自己定义的格式，那么之前的/的格式就不能再使用了，使用的话同样会报类型转换异常。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE624927926a00455b49bcbcbba00f8642.png)

## 日期格式类型

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd9babc765e56b491d243ab44f541337e.png)

当然，每次使用这种方式来进行日期格式的转换也太麻烦了，我们可以使用注解来让这件事情变得简单，这个注解就是@DateTimeFormat，在对应的date传入参数前直接接上这个注解然后在括号内指定其日期格式就可以了，具体的格式如下图所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6cd0bfc1b5d3ca63de31b526a384c288.png)

但是需要在配置文件里开启注解驱动支持，这一点不要忘了，最后我们可以写入其代码如下

```
//数据类型转换，使用自定义格式化器或@DateTimeFormat注解设定日期格式
//两种方式都依赖springmvc的注解启动才能运行
//http://localhost/requestParam11?date=1999-09-09
@RequestMapping("/requestParam11")
public String requestParam11(@DateTimeFormat(pattern = "yyyy-MM-dd") Date date){
    System.out.println(date);
    return "page.jsp";
}
```

其效果和我们之前是一模一样的，以后我们使用这个就可以了，之前配置文件的就不用使用了，那些是原理性的东西来的，我们了解就好。

## 自定义类型转换器

本节我们来学习如何自定义类型转换器，我们仍然是以日期类型转换器来举例，同时本节内容只做了解，并非我们学习的重点。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEad2002be7122f5dd0885a1b46a353e31.png)

首先我们要创建一个实现类，令其继承Converter接口，该接口使用泛型，左边是传入的数据类型，右边是要转换的数据类型，我们在里面做的处理是通过调用其他的方法令其转换为我们想要的日期格式，注意这里我们绝不可将异常抛出，必须自己处理，这是我们的规范，因为我们的类型转换器是不监控异常也不处理的，因此有异常我们要自己处理

那么我们可以写入其代码如下

```
package com.itheima.converter;



import org.springframework.core.convert.converter.Converter;

import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class MyDateConverter implements Converter<String,Date>{


    @Override
    public Date convert(String source) {
        DateFormat df = new SimpleDateFormat("yyyy-MM-dd");
        Date date = null;
        try {
            date = df.parse(source);
        }catch (ParseException e){
            e.printStackTrace();
        }
        return date;
    }
}

```

然后我们要对我们的自定义转换器进行一个注册，这个过程其实和我们上一节的差不多这里就不赘述了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE72530fe511c6ae1ca25a0c694c89956e.png)

具体的代码如下图所示

```
<mvc:annotation-driven conversion-service="conversionService"/>
<bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
    <property name="converters">
        <set>
            <bean class="com.itheima.converter.MyDateConverter"/>
        </set>
    </property>
</bean>
```

最后我们经过测试会发现我们的自定义转换器类是可以正常工作的，是没有问题的

## 请求映射

最后我们来讲讲我们一开始最先使用的RequestMapping的映射注解，我们目前位置都是将其使用在我们的方法上，但实际上，这个注解还可以用于类上

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe3fa979a5f548a902b4833090e9ac416.png)

但是这里值得注意的是如果我们用于类上的话，那么我们都在访问对应的映射路径时，要在访问路径上先加上对应的类注解上写入的路径。同时我们的页面，也就是jsp文件，也要放在对应的文件夹下，具体表现在我们需要在web目录下新创建一个类注解上指定的路径的包，然后将页面放到里面，然后其才能通过对应的字符串寻找到对应的响应页面

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5d34aab8d318174d40c206b5bc89bb09.png)

最后值得一提的是，如果我们的方法路径上不加/的话，那么其仍然是从根路径下开始访问的，而不是从我们类中的文件夹开始访问的

# 响应

学习完了请求之后，我们接着来学习响应的相关内容。这也是springmvc中比较重要的一章，因此我们要认真学

## 页面跳转

我们先来回顾下我们的回顾的页面跳转，我们是通过返回值类型来进行页面的跳转的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEda3656fc96bebf3a12680183eef82036.png)

我们之前在Tomcat的章节中学习过，我们的页面跳转分为两种，一种是转发，另一种是重定向，那我们现在使用的这种方式的到底是重定向还是转发呢？以及我们要如何使用重定向以及转发呢？这是我们接下来的要解决的事情。

在正式学习之前我们要先搞清楚转发和重定向的区别，他们两者最简单的区别在于，使用转发时，我们的地址栏会是我们一开始输入的地址栏，而使用重定向的话，我们的地址栏最终会变为我们定向的位置的地址栏，我们默认的跳转页面的方式是转发方式。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE93826ff694d1d671ed56f9f6bc6ca1b5.png)

如果我们要使用转发方式的话，我们就在其返回的字符串前加入forward:，如果想要使用重定向，则用redirect:，注意我们这里在:的后面其实也可以继续追加我们的/符号的，就当没用过这个时候的用法来用就可以了

最后我们值得一提的是，转发的形式是可以访问我们的WEB-INF文件夹下的创建的页面的，而重定向不可以，因此如果我们想问访问该文件夹下的页面，应该要使用转发的方式来实现页面的跳转。同时如果我们要访问该文件夹下的页面，需要在返回的地址上添加其存放的子文件夹的路径，否则我们springmvc会无法找到。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6cce9f9231ead7ab6030e203ee506efd.png)

当然每次都要写一次这么长的路径未免太麻烦了，实际上我们也存在快捷设定，这个设定需要在springmvc的xml文件中去设置，这里需要使用到InternalResourceViewResolver类，利用该类添加我们的前缀和后缀，这样我们只需要返回对应的文件名称就完了，其实际执行的时候会自动将前缀和后缀拼接起来再去寻找

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb0c1200ca1cb515f34fb14af16b469d6.png)

那么我们只需要在对应的配置文件上写入代码如下

```
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/page/"/>
    <property name="suffix" value=".jsp"/>
</bean>
```

最后我们可以写入我们的代码如下

```
//页面简化配置格式，使用前缀+页面名称+后缀的形式进行，类似于字符串拼接
@RequestMapping("/showPage3")
public String showPage3() {
    System.out.println("user mvc controller is running ...");
    return "page";
}
```

这里值得一提的是，我们一旦使用的这种格式，那么我们默认就只能使用转发，而且我们不能在返回的字符串里去指定我们想要的页面跳转的方式

最后我们来讲讲最后一个跳转方式，如果我们将方法设置会无返回值，那么其就会默认使用访问路径作为页面地址的名字并拼接前缀后缀去查找，具体内容看图吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE676803a8ec0bf8fe0db67dd1d17e858e.png)

## 带数据页面跳转

那么接着我们就要学习带着页面跳转的方法，我们的一个简单方法就是使用Servlet里的请求对象，设定对应的数据接受并返回到页面中

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1f95fba78ec39a1cf60e3dc682696382.png)

那么我们可以构造我们的设置代码如下，同样类中还有注解，但是这里省略了

```
@RequestMapping("/showPageAndData1")
public String showPageAndData1(HttpServletRequest request) {
    request.setAttribute("name","itheima");
    return "page";
}
```

然后我们设置我们的页面响应代码如下，其实就是获取了对应的数据而已

```
<%@page pageEncoding="UTF-8" language="java" contentType="text/html;UTF-8" %>
<html>
<body>
<h1>页面跳转测试中...sdfasdfa</h1>
info:${name}
</body>
</html>
```

最后我们经过测试会发现这个方法的确能够运行并使用

但是这种方式不是我们所推荐的，我们尽量就不要使用原来学习过的方式，因此这里我们要学习新的方式

我们的第二种跳转方式是使用Model类型的形参来进行数据传递，其内部使用的就是Request对象的方法，其不仅可以放简单类型的数据，还可以放引用类型的数据

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE72c2fc25178fa1a61ffd6bd622e0a060.png)

那么我们按照上图的演示我们可以构造其代码如下

```
@RequestMapping("/showPageAndData2")
public String showPageAndData2(Model model) {
    model.addAttribute("name","Jock");
    Book book = new Book();
    book.setName("SpringMVC入门案例");
    book.setPrice(66.66d);
    model.addAttribute("book",book);
    return "page";
}
```

同时我们这里为了演示其还可以设置一个对象，因此我们这里要创建一个对应的对象的实体类，然后我们这里new一个对象并设置对应的值，然后我们在页面代码上填入我们的代码如下

```
<%@page pageEncoding="UTF-8" language="java" contentType="text/html;UTF-8" %>
<html>
<body>
<h1>页面跳转测试中...sdfasdfa</h1>
info的名字:${name}<br/>
book:${book.toString()}
</body>
</html>
```

除了对象里的中文会乱码之外一切都好，这就说明我们的案例已经成功了

但是实际中这种方法仍然不是我们springmvc中最推荐的，因此我们还要学习第三种方法

第三种方式我们是使用ModelAndView的对象进行数据传递，这个对象跟我们以前学习架构的时候出现的对象结构一模一样，同样具有上一个方式的所有方法，那么model可以理解，但是view是什么呢？其实view就是视图，我们这里的对象是将视图也一并封装进去到对象里了，因此我们这里返回的对象也是直接返回该对象，因为该对象就包括视图，而不是返回某个字符串再进行查询到对应的视图然后再返回

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe51de141b93086cacf64c365ed10eefd.png)

那么根据上图我们可以写入我们的代码如下

```
@RequestMapping("/showPageAndData3")
public ModelAndView showPageAndData2(ModelAndView modelAndView) {
    Book book = new Book();
    book.setName("SpringMVC入门案例");
    book.setPrice(66.66d);

    modelAndView.addObject("book",book);
    modelAndView.addObject("name","Jockme");

    modelAndView.setViewName("page");

    return modelAndView;
}
```

这里我们可以看到我们的数据是添加的，而视图是设置的，如果我们设置多个视图，那么就会以最后一个为准。我个人猜测这里我们传入page名称，其也会根据前后缀查询到对应的视图并将该视图的内容设置到该对象中并返回给用户用于展示。

如果我们希望返回使用重定向的话，那么也是跟之前的一样设定就好了，首先不使用前后缀的简写方式，然后自己在地址中写入想要的方式和对应页面的全路径名，接着就可以实现转发了，同样的，重定向也是无法重定向到WEB-INF文件夹内的页面的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE650097cf72b4f085cd5a73731f414ede.png)

最后我们可以设置其代码如下

```
//ModelAndView对象支持转发的手工设定，该设定不会启用前缀后缀的页面拼接格式
@RequestMapping("/showPageAndData4")
public ModelAndView showPageAndData4(ModelAndView modelAndView) {
    modelAndView.setViewName("forward:/WEB-INF/page/page.jsp");
    return modelAndView;
}

//ModelAndView对象支持重定向的手工设定，该设定不会启用前缀后缀的页面拼接格式
@RequestMapping("/showPageAndData5")
public ModelAndView showPageAndData6(ModelAndView modelAndView) {
    modelAndView.setViewName("redirect:page.jsp");
    return modelAndView;
}
```

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE23bac7369d4783652b42c57436b3b2c2.png)

## 返回json数据

学习完了前两种方式之后，接着我们来讲我们的最重要的一节内容，返回json数据的方式来返回页面。

我们的第一种最简单的返回页面的方式自然是通过响应对象直接返回了，具体请看代码

```
@RequestMapping("/showData1")
public void showData1(HttpServletResponse response) throws IOException {
    response.getWriter().write("message");
}
```

但同样的，这是方式是我们所不推荐额，然后我们来看看我们的直接返回信息的方式，由于直接返回信息会找不到对应的返回页面，因此我们这里要额外加入ResponseBody注解，表示我们返回的信息会自己生成一个页面对象并直接返回用于展示

```
@RequestMapping("/showData2")
@ResponseBody
public String showData2() {
    return "message";
}
```

然后我们正式来学习整一个json的的字符串类型的数据，我们自己new一个ObjectMapper对象，传入对应的对象，其会自动生成一个json格式的字符串给我们

```
@RequestMapping("/showData3")
@ResponseBody
public String showData3() throws JsonProcessingException {
    Book book = new Book();
    book.setName("SpringMVC入门案例");
    book.setPrice(66.66d);

    ObjectMapper om = new ObjectMapper();
    return om.writeValueAsString(book);
}
```

当然，每次都要自己放进去就显得太麻烦了，而且还有乱码，我们以后肯定是希望由工具类帮助我们完成json格式数据的展示的，因此我们这里要引入json工具类的依赖

```
<!--json相关坐标3个-->
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-core</artifactId>
  <version>2.9.0</version>
</dependency>

<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
  <version>2.9.0</version>
</dependency>

<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-annotations</artifactId>
  <version>2.9.0</version>
</dependency>
```

然后我们开启对应的注解驱动，注意这是在spring-mvc配置文件中的代码，这点不要忘了，不启动对应的注解驱动连注解都用不了

```
<mvc:annotation-driven/>
```

然后我们可以写入其代码如下

```
@RequestMapping("/showData4")
@ResponseBody
public Book showData4() {
    Book book = new Book();
    book.setName("SpringMVC入门案例");
    book.setPrice(66.66d);

    return book;
}
```

接着在对应的页面上就会展示出json格式的字符串了，这里我们传入了对象，而我们又引入了对应的json格式的依赖，因此我们的json的工具类会发挥其作用，将我们的对象转换为我们想要的字符串格式，Book转成String是我们的json导入的依赖工具类里所完成的事

当然，我们也可以展示集合

```
@RequestMapping("/showData5")
@ResponseBody
public List<Book> showData5() {
    Book book1 = new Book();
    book1.setName("SpringMVC入门案例");
    book1.setPrice(66.66d);

    Book book2 = new Book();
    book2.setName("SpringMVC入门案例");
    book2.setPrice(66.66d);


    List<Book> list = new ArrayList<>();
    list.add(book1);
    list.add(book2);
    return list;
}

```

最后我们可以来看一些总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEfb91388420f7bfc6b0735a54a82779b7.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc2ccb4b01729fd4680c257b061a5d996.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5f4128dccb5866672bdfe6b783a66012.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf3a9e3d04d02178065a73c6b5be32313.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE226f0b94361345e59975649a5aeaced8.png)

## Servlet相关接口

本节我们来了解springMVC中的有关于Servlet相关接口的方法，首先在springmvc中提供了原始的Servlet接口API的功能，我们可以通过形参直接声明并调用来使用这些方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE549d1595714adffa8ea3159d6c41acc6.png)

那么我们可以构造其代码如下

```
//获取request,response,session对象的原生接口
@RequestMapping("/servletApi")
public String servletApi(HttpServletRequest request, HttpServletResponse response, HttpSession session){
    System.out.println(request);
    System.out.println(response);
    System.out.println(session);
    return "page";
}
```

但是我们都知道，直接操纵Servlet的对应方法不是springmvc所推荐的，因此在springmvc中提供了RequestHeader的注解，用于获取请求头的消息内容，只需要在传入参数中使用该注解并传入对应的请求头的字符串即可，但是记得我们需要开启注解驱动，否则该注解无法发挥作用

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE84b4036f6f2b83e2cb77a84a928f92d7.png)

那么根据上图我们可以写入代码如下

```
//获取head数据的快捷操作方式
@RequestMapping("/headApi")
public String headApi(@RequestHeader("Accept-Encoding") String headMsg){
    System.out.println(headMsg);
    return "page";
}
```

如果我们想要获取Cookie的缓存中的参数的话，则需要使用CookieValue注解，具体请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE19bf859fb6c4a59583b564dd5bdf53c8.png)

那么我们可以构造我们的代码如下，这里我们是获取JSESSIONID的id

```
//获取cookie数据的快捷操作方式
@RequestMapping("/cookieApi")
public String cookieApi(@CookieValue("JSESSIONID") String jsessionid){
    System.out.println(jsessionid);
    return "page";
}
```

然后我们可以可以获取Session共享域中的数据，不过对于这种共享域，我们拿到里面的数据之前要先往里面设定对应的数据才可以，否则会报错，我们这里设置数据就直接调用对应的Session的set方法来设置了，虽然不合规范，但是能用就行

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEef56ba98e94060485eadc71938c810a0.png)

那么我们可以构造设置的代码如下，获取的演示代码我们后面一起演示

```
//测试用方法，为下面的试验服务，用于在session中放入数据
@RequestMapping("/setSessionData")
public String setSessionData(HttpSession session){
    session.setAttribute("name","itheima");
    return "page";
}
```

实际上我们数据设置还有一种方式，就是用SessionAttributes注解，具体方式是在对应类上面加入该注解并在括号中指定我们要添加的数据的名字，这样我们后面添加数据时，只要用了相同的名字，其就可以通过SessionAttribute注解进行获取。同时使用该注解时，我们要设置数据是要使用model对象来设置的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8f8b24b19cc2bc7fdd428515c8f41ce8.png)

首先我们往我们的类中加入我们开头注解并设定对应的数据名

```
//设定当前类中名称为age和gender的变量放入session范围，不常用，了解即可
@SessionAttributes(names = {"age","gender"})
```

然后我们写入对应的设置参数的方法

```
//配合@SessionAttributes(names = {"age","gender"})使用
//将数据放入session存储范围，通过Model对象实现数据set，通过@SessionAttributes注解实现范围设定
@RequestMapping("/setSessionData2")
public String setSessionDate2(Model model) {
    model.addAttribute("age",39);
    model.addAttribute("gender","男");
    return "page";
}
```

最后我们再写入我们获取对应的数据的注解到形参中，然后在方法中对对应的数据进行打印

```
//获取session数据的快捷操作方式
@RequestMapping("/sessionApi")
public String sessionApi(@SessionAttribute("name") String name,
                         @SessionAttribute("age") int age,
                         @SessionAttribute("gender") String gender){
    System.out.println(name);
    System.out.println(age);
    System.out.println(gender);
    return "page";
}
```

实际测试之后我们发现这个是可以使用的，是没有问题的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa96535f0b77ccfb7d59bd2e4a391f681.png)

最后我们来说说我们的注解式参数封装原理，其实他的本质还是调用我们的最初的Servlet的方法，只不过是对其进行了封装，然后我们就无法直接调用其最本质的方法，只能调用外面的方法了，同时其是使用顶层接口HandlerMethodArgumentResolver及其其下的许许多多的实现类来达成对应的目的的

# 异步调用

为了这一节，我甚至不得去重新学习JS、jQuery以及AJAX，想当初我是因为觉得他们用不上所以跳过的，现在又给补回来了，只能说欠下的债，总有一天是要还的

然后本章我们来学习SpringMVC的异步调用

## 接收异步请求参数

我们先来介绍下我们的案例环境，我们的springmvc配置文件的代码如下

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <!--扫描加载所有的控制类类-->
    <context:component-scan base-package="com.itheima"/>
    
    <mvc:resources mapping="/js/**" location="/js/"/>

    <mvc:annotation-driven/>
</beans>
```

可以看到我们这里只做了简单的扫包以及开启注解驱动，还有我们放开了js的静态资源，因为我们这里会用上对应的类库，因此我们要对其进行对应的放开，否则都无法调用对应的方法

然后我们创建了一个对应的User的实体类，然后我们在web包下创建一个ajax的jsp文件并写入其代码如下

```
<%@page pageEncoding="UTF-8" language="java" contentType="text/html;UTF-8" %>

<a href="javascript:void(0);" id="testAjax">访问springmvc后台controller</a><br/>
<a href="javascript:void(0);" id="testAjaxPojo">访问springmvc后台controller，传递Json格式POJO</a><br/>
<a href="javascript:void(0);" id="testAjaxList">访问springmvc后台controller，传递Json格式List</a><br/>
<a href="javascript:void(0);" id="testAjaxReturnString">访问springmvc后台controller，返回字符串数据</a><br/>
<a href="javascript:void(0);" id="testAjaxReturnJson">访问springmvc后台controller，返回Json数据</a><br/>
<a href="javascript:void(0);" id="testAjaxReturnJsonList">访问springmvc后台controller，返回Json数组数据</a><br/>
<br/>
<a href="javascript:void(0);" id="testCross">跨域访问</a><br/>

<script type="text/javascript" src="${pageContext.request.contextPath}/js/jquery-3.3.1.min.js"></script>

<script type="text/javascript">
    $(function () {
        //为id="testAjax"的组件绑定点击事件
        $("#testAjax").click(function(){
            //发送异步调用
            $.ajax({
                //请求方式：POST请求
                type:"POST",
                //请求的地址
                url:"ajaxController",
                //请求参数（也就是请求内容）
                data:'ajax message',
                //响应正文类型
                dataType:"text",
                //请求正文的MIME类型
                contentType:"application/text",
            });
        });

        //为id="testAjaxPojo"的组件绑定点击事件
        $("#testAjaxPojo").click(function(){
            $.ajax({
                type:"POST",
                url:"ajaxPojoToController",
                data:'{"name":"Jock","age":39}',
                dataType:"text",
                contentType:"application/json",
            });
        });

        //为id="testAjaxList"的组件绑定点击事件
        $("#testAjaxList").click(function(){
            $.ajax({
                type:"POST",
                url:"ajaxListToController",
                data:'[{"name":"Jock","age":39},{"name":"Jockme","age":40}]',
                dataType:"text",
                contentType:"application/json",
            });
        });


        //为id="testAjaxReturnString"的组件绑定点击事件
        $("#testAjaxReturnString").click(function(){
            //发送异步调用
            $.ajax({
                type:"POST",
                url:"ajaxReturnString",
                //回调函数
                success:function(data){
                    //打印返回结果
                    alert(data);
                }
            });
        });

        //为id="testAjaxReturnJson"的组件绑定点击事件
        $("#testAjaxReturnJson").click(function(){
            //发送异步调用
            $.ajax({
                type:"POST",
                url:"ajaxReturnJson",
                //回调函数
                success:function(data){
                    alert(data);
                    alert(data['name']+" ,  "+data['age']);
                }
            });
        });

        //为id="testAjaxReturnJsonList"的组件绑定点击事件
        $("#testAjaxReturnJsonList").click(function(){
            //发送异步调用
            $.ajax({
                type:"POST",
                url:"ajaxReturnJsonList",
                //回调函数
                success:function(data){
                    alert(data);
                    alert(data.length);
                    alert(data[0]["name"]);
                    alert(data[1]["age"]);
                }
            });
        });

        //为id="testCross"的组件绑定点击事件
        $("#testCross").click(function(){
            //发送异步调用
            $.ajax({
                type:"POST",
                url:"http://www.jock.com/cross",
                //回调函数
                success:function(data){
                    alert("跨域调用信息反馈："+data['name']+" ,  "+data['age']);
                }
            });
        });
    });
</script>
```

我们上面的代码里所干的事情就是加了对应的方法，然后向客户端发送了对应的请求，有些还会传入一些参数到客户端上

## 发送异步请求

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbef38d29ea5b4175809c034b431613e7.png)

最后我们创建一个Ajax的实现类来实现我们的异步调用，我们真正要完成的代码也是在这里完成

我们先来看看我们的第一个事件的向服务器请求的内容，可以看到第11行，我们这里使用了一个data来发送了一个字符串参数过去

```
$(function () {
    //为id="testAjax"的组件绑定点击事件
    $("#testAjax").click(function(){
        //发送异步调用
        $.ajax({
            //请求方式：POST请求
            type:"POST",
            //请求的地址
            url:"ajaxController",
            //请求参数（也就是请求内容）
            data:'ajax message',
            //响应正文类型
            dataType:"text",
            //请求正文的MIME类型
            contentType:"application/text",
        });
    });
```

如果这时我们想要在服务器上接受的到这个参数，那我们应该要怎么做呢？我们的一个简单的想法就是直接在我们的响应内容中加入一个形参来接受这个参数，但这样实际上是接受不了的。这是因为我们之前是是通过参数的方法直接发送去的，而我们如果使用AJAX的方式来发送请求，那么这里面一并发送的参数是整个请求正文，直接用形参是接受不了的，此时我们就要使用RequestBody注解，该注解可以将请求正文里的参数自动赋值到我们的形参中

```
@RequestMapping("/ajaxController")
//使用@RequestBody注解，可以将请求体内容封装到指定参数中
public String ajaxController(@RequestBody String message){
    System.out.println("ajax request is running..."+message);
    return "page.jsp";
}
```

后续的内容都是依葫芦画瓢，这里就不多提了

```
@RequestMapping("/ajaxPojoToController")
//如果处理参数是POJO，且页面发送的请求数据格式与POJO中的属性对应，@RequestBody注解可以自动映射对应请求数据到POJO中
//注意：POJO中的属性如果请求数据中没有，属性值为null，POJO中没有的属性如果请求数据中有，不进行映射
public String  ajaxPojoToController(@RequestBody User user){
    System.out.println("controller pojo :"+user);
    return "page.jsp";
}

@RequestMapping("/ajaxListToController")
//如果处理参数是List集合且封装了POJO，且页面发送的数据是JSON格式的对象数组，数据将自动映射到集合参数中
public String  ajaxListToController(@RequestBody List<User> userList){
    System.out.println("controller list :"+userList);
    return "page.jsp";
}
```

值得一提的是，之前我们说过如果我们的集合里放着的是我们的自定义对象，我们是要用另外一个对象来做的，但是这里仍然是只用一个RequestBody注解就完了，属于是非常方便了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE745d9d372a4ee737c07a21df9949691b.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6011d9b7ba6abee22c1a2485d77bc7ce.png)

## 异步请求接受响应数据

接着我们来讲我们异步请求如何接受响应数据，简单来说，我们的页面要如何接收到我们的在服务器端返回的页面对象的数据

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE793d3c4948cb11ce70575193259efbd6.png)

具体我们来看看我们页面里的这十四行的代码，里面获得响应的数据并打印

```
//为id="testAjaxReturnString"的组件绑定点击事件
$("#testAjaxReturnString").click(function(){
    //发送异步调用
    $.ajax({
        type:"POST",
        url:"ajaxReturnString",
        //回调函数
        success:function(data){
            //打印返回结果
            alert(data);
        }
    });
});

```

首先，如果我们的页面完全不做改动，那么最终我们会返回一个page.jsp的代码，其返回这个是没问题的，因为一般我们都是用这个放到页面上，只是这里我们将其传给我们的页面了，因此打印的内容就是里面的编码。但是这并不是我们在这个案例中想要的，我们希望页面得到的不是这一个编码内容，而是这一个字符串，那么此时我们应该要怎么做呢？这时就需要用到ResponseBody注解

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEad194411722eee90cea84e37c866a627.png)

在对应方法上用该注解可以告知我们的springMVC，将我们的返回值作为一个响应正文来处理，而不要对其进行相应的视图解析，能够达到我们写入什么就返回什么的效果。那么最后我们可以构造其代码如下

```
    //使用注解@ResponseBody可以将返回的页面不进行解析，直接返回字符串，该注解可以添加到方法上方或返回值前面
    @RequestMapping("/ajaxReturnString")
//    @ResponseBody
    public @ResponseBody String ajaxReturnString(){
        System.out.println("controller return string ...");
        return "page.jsp";
    }
```

最后值得一提的是，我们的该注解不但可以放在我们的方法上，也可以放到我们返回值前面，这就相当于是用该注解修改我们的返回值，令其不会解析。这个很好理解，不过为了防止我们的方法名过长的情况所以我们一般不这样做，还是正常写在方法之上

然后如果我们要返回一个Pojo对象时也是同样的方法，同时如果返回值为Pojo，那么其会自动封装数据成json对象数据

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa8bc9b6e154b3c64e584dea6e2a16e93.png)

那么我们可以写入代码如下

```
@RequestMapping("/ajaxReturnJson")
@ResponseBody
//基于jackon技术，使用@ResponseBody注解可以将返回的POJO对象转成json格式数据
public User ajaxReturnJson(){
    System.out.println("controller return json pojo...");
    User user = new User();
    user.setName("Jockme");
    user.setAge(39);
    return user;
}
```

值得一提的是，json数据回到页面时又会被自动封装成一个对象，所以如果我们想要打印对应的内容的话，应该要在页面的打印代码上做相应的改动，如下所示

```
//为id="testAjaxReturnJson"的组件绑定点击事件
$("#testAjaxReturnJson").click(function(){
    //发送异步调用
    $.ajax({
        type:"POST",
        url:"ajaxReturnJson",
        //回调函数
        success:function(data){
            alert(data);
            alert(data['name']+" ,  "+data['age']);
        }
    });
});
```

返回值为List的情况也是一样的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEfc84ce921ee955e0ce1832dd103a52b1.png)

我们可以构造其代码如下

```
@RequestMapping("/ajaxReturnJsonList")
@ResponseBody
//基于jackon技术，使用@ResponseBody注解可以将返回的保存POJO对象的集合转成json数组格式数据
public List ajaxReturnJsonList(){
    System.out.println("controller return json list...");
    User user1 = new User();
    user1.setName("Tom");
    user1.setAge(3);

    User user2 = new User();
    user2.setName("Jerry");
    user2.setAge(5);

    ArrayList al = new ArrayList();
    al.add(user1);
    al.add(user2);

    return al;
}
```

## 跨域访问

接着我们来学习异步调用的最后一个内容，跨域访问，这个知识是作为我们以后学习微服务的一个学习准备。我们先来介绍下什么是跨域访问

我们之前的访问都是一个服务器去访问他自己的东西的，这不涉及跨域访问，但如果我们要一个服务器连接另一个服务器的话，那么就涉及到跨域访问了，我们跨域访问的连接的一个最简单的方式是通过ip地址连接，但是这个方法太low了，我们一般是让我们的不同的服务器申请一个域名，可以通过域名解析到一个ip，然后通过域名来访问的。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE54a66796171abb0d5491978af8147bb0.png)

所谓跨域访问，就是当通过域名A下的操作访问域名B下的资源时，成为跨域访问。那么怎么样才叫两个不同的域呢？这里分别有请求协议不同、ip地址不同、端口号不同，这三个只要有一个不同，那么其就是两个不同的域，最后，还有一点，域名不同也是不同的域，因为其解析之后会得到两个服务器的地址。

按照上面知道的信息，我们可以在我们的本机上打造出两个域来实现跨域访问，只要在我们的本机上搞两个域名不同的应用就行，即使他们的ip地址相同，但是由于其服务器不同，所以其还是跨域访问

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf8e39e8aefb3c2418a7073ef50fadc28.png)

那么接着我们就要来搭建我们的跨域环境，我们要在C盘中找到这么一个文件

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE23f5b6defa0f6de20091ee3a7c0af018.png)

然后我们将其内容修改为

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd20358b1a1c0773040879fb1b57c9f87.png)

然后直接写入其代码如下，其中127.0.0.1代表的是本机的ip地址，任何电脑都是一样的，而后面的域名则是我们自己给本机设置的域名

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb4a027dcd1dd200276fdd54c11fcb8bf.png)

修改这个文件需要管理员权限，这个不要忘了，然后我们修改之后要用cmd命令窗口进行更进一步的设置

首先打开cmd窗口，然后输入命令如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE76473bfa83674465432d11cf5a83fcdc.png)

然后输入命令如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbd298785555361e12f6d0589d8d31b0c.png)

然后我们开启对应的服务器，接着我们打开对应的网页，首先进入对应的页面，然后我们用我们的本机ip的地址也去访问我们的服务器下的内容，输入网址如下http://www.jock.com/ajax.jsp，然后我们进行测试，点击跨域访问会发现两个都可以正确打印我们的内容。一个是我们的jock的域名，一个是我们的localhost的域名

然后我们将我们的页面的代码稍作改动如下

```
$("#testCross").click(function(){
    //发送异步调用
    $.ajax({
        type:"POST",
        url:"http://www.jock.com/cross",
        //回调函数
        success:function(data){
            alert("跨域调用信息反馈："+data['name']+" ,  "+data['age']);
        }
    });
});
```

简单来说，就是将我们的原本的cross改为http://www.jock.com/cross，相当于是从直接访问cross变为要去访问另一个域名上的cross，此时我们再次开启我们服务器，我们会发现jock域名可以正常工作，但是localhost不行，我们能够在下方看到其对应的提示说不支持跨域访问，因为我们没有开启，此时我们要开启跨域访问就需要用到CrossOrigin注解

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa17c1bd938984e7ae18fc067d9900fc0.png)

那么我们可以将我们的代码修改如下

```
@RequestMapping("/cross")
@ResponseBody
//使用@CrossOrigin开启跨域访问
//标注在处理器方法上方表示该方法支持跨域访问
//标注在处理器类上方表示该处理器类中的所有处理器方法均支持跨域访问
@CrossOrigin
public User cross(HttpServletRequest request){
    System.out.println("controller cross..."+request.getRequestURL());
    User user = new User();
    user.setName("Jockme");
    user.setAge(39);
    return user;
}

```

我们只要加入该注解，我们就会发现其就支持跨域访问了

# 拦截器

那么接着我们来学习SpringMVC中的拦截器，这里我们要注意的是，我们的拦截器这一章，是非常重要的，所以我们要认真学，务必要学会搞懂。

## 拦截器简介

来解析下我们的处理过程，我们的客户端浏览器向服务器发送请求，该请求首先被tomcat获取到并进行处理，其中所有的静态资源tomcat都会获得并帮我们处理好。然后是动态资源，这些动态资源要经过一层层的过滤器，最终到达spring中，然后进入spring中的springmvc里进行处理，当然其处理是经过Handler对象进行处理的，此时出现的一个问题就在于，使用Handler对象进行处理，可能会需要相应的权限，如果我们在每一个Handler对象里都做赋予权限这种事情，这未免效率就有些太低了，于是我们就在其进入该对象之前，设定了一个拦截器，该拦截器可以赋予一些内容对应的权限，也可以将一些不具有权限的内容或者方法拦截下来，最后当其处理完之后，有些权限是一次性的，用过了就要归还，此时就需要用到后面的第二个拦截器，用于归还对应的权限

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa459665ebb6af83ab823730ec2183b72.png)

其实这个过程简单来说就好像去过安保上班，进三道门过对应的验证来传对应的衣服，出去的时候又将对应的衣服归还，大概是这个流程。

我们要知道，拦截器是一种动态拦截方法调用的机制，其核心原理是AOP思想，不过其与AOP不同，就是其更加专一，AOP几乎无所不包，而拦截器是负责一部分功能，就是专门做请求和响应的功能。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE48be3e4e5a3da14ba90396642c5ecc4a.png)

拦截器链指的则是多个拦截器按照一定的顺序，对原始被调用功能进行增强的功能。同时我们现在这样听来，似乎拦截器的作用和过滤器相差无几，那么它们两者之间又有什么区别呢？

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8605af1ace883bf1b4750306b5c91c73.png)

其具体区别在于两点，分别是归属不同以及拦截内容不同，过滤器属于Servlet技术，而拦截器则属于SpringMVC技术，同时过滤器可以对所有访问进行增强，而拦截器只能只针对所有对SpringMVC的访问进行增强

最后我们要提一点是，拦截器的最大作用，就是对对应的对SpringMVC的访问进行增强。

## 自定义拦截器开发过程

然后我们来自定义我们的拦截器开发，其实这个过程跟我们的之前的开发我们的AOP的时候差不多，不过也是有所不同的，所以我们这里再演示一遍

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE47af4da8981704b2fde7b36e5736edaa.png)

这里我们值得一提的是，我们的springMVC中是有预先配置好的拦截器的，我们可以直接使用，我们自定义的时候是意味着其原来的拦截器功能已经不适用于我们的项目的时候，我们要做的事

我们先来介绍我们的案例环境吧，首先我们的pom文件还是跟以前一样，然后我们的xml文件就做了扫描控制类和加载注解驱动，然后我们在控制类里写入了如下的代码

```
@Controller
public class InterceptorController {
    @RequestMapping("handlerRun")
    public String handlerRun() {
        System.out.println("业务处理器运行-------------main");
        return "page.jsp";
    }
}

```

接着我们要自定义拦截器类，那我们就需要实现我们的HandlerInterceptor接口

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1084b32c94c11ad7fd45cb8543aa9d8e.png)

该接口的三个抽象方法是有其默认实现的，该方法也是拦截器的核心方法，那么我们可以重写这三个并写入其代码如下

```
public class MyInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("前置运行----a1");
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("后置运行----b1");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("成功运行----c1");
    }
}

```

这里我们可以看到我们实现的方法有三个，分别是preHandle、postHandler、afterCompletion，其中第一个方法是用于拦截对应的方法的，如果我们返回的是false，则后两个方法不会执行，反之则会。第二个方法是在我们的功能执行完毕之后才允许的拦截方法，而最后一个方法是只要我们的类方法执行就一定会在最后执行的一个方法

我们写好了对应的方法之后，还要给拦截器加入对应的配置，否则拦截器就不会工作

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE464b5b20266d8432831f273b5f9377ce.png)

我们这里写入的path是我们要拦截的具体方法，然后往其中加入一个bean的class类，该类是我们自定义的拦截器类。当然实际我们也可以另外创建一个bean，然后在此处引入对应的bean的id，我们这里贪方便就直接创建了

```
<mvc:interceptors>
    <mvc:interceptor>
        <mvc:mapping path="/handlerRun"/>
        <bean class="com.itheima.interceptor.MyInterceptor"/>
    </mvc:interceptor>
</mvc:interceptors>
```

我们开启我们的服务器，然后刷新对应的页面就相当于是请求了服务器，此时就会在控制台上正确打印我们写在拦截器上的语句以及我们的方法本身的语句，此时就说明我们的案例已经成功了

最后我们来看看有拦截器和无拦截器时我们的方法的执行流程

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE81cf6f3b15dbd56dce5b773e8885859b.png)

## 拦截器配置与方法参数

接着我们来讲下我们的拦截器的具体配置和里面的形参，首先是我们的前置处理方法preHandle。里面有最基本的请求和响应对象，有了这两个对象我们就可以再原始方法运行之前进行一些对请求和响应对象的增强，而后面的handler对象则是一个封装了原始的方法的对象，被封装的方法是Method对象，我们可以直接把它当做Method对象来使，都差不多好用，该对象可以用于对方法本身做一些增强

而返回的值是true还是false则是决定了后面的方法要不要执行，同时也决定了原始方法要不要执行。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc86e337230c7c04661ee6e4ca904c737.png)

然后我们来讲讲我们的后置处理方法postHandler，该方法执行的前提是原始方法没有被拦截，其下还多了一个ModelAndView对象，这个是视图对象，这个视图对象内部就是封装了我们的服务器响应给我们的视图。即使服务器给我们的响应的是一个字符串，其仍然会被封装为一个视图对象并发送到客户端，只不过就是字符串封装的数据中，然后视图本身就是空的，因为服务器没给一个具体响应页面过来，所以为空。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE92362aafb760f20dbe8c2c42ea54fb99.png)

最后我们来看看我们的完成处理方法，该方法是拦截器最后执行的方法，这个方法存在的意义就是如果处理器执行过程中出现异常对象，那么我们就可以针对异常请看进行单独处理

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE51ddb25c6397647ee81d8b5935a32885.png)

然后我来看看我们的拦截器的配置项

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa332483887ea49a239a14e6d9941edc2.png)

这里我们要记住的是，我们的path标签可以设置多个，同时其下还有多种设置格式可以用于设置我们要拦截的内容，最常用的是/*表示拦截所有，而/get*则表示拦截所有以get开头的方法，其他格式的当然还有，但是我们一般只使用这两个

同时其下还有一个exclude和include的属性，该属性其实也很简单，就是用于排除某些方法或者是包括某些方法的设置

那么最后我们可以构造其代码如下

```
<!--开启拦截器使用-->
<mvc:interceptors>
    <!--开启具体的拦截器的使用，可以配置多个-->
    <mvc:interceptor>
        <!--设置拦截器的拦截路径，支持*通配-->
        <!--/**         表示拦截所有映射-->
        <!--/*          表示拦截所有/开头的映射-->
        <!--/user/*     表示拦截所有/user/开头的映射-->
        <!--/user/add*  表示拦截所有/user/开头，且具体映射名称以add开头的映射-->
        <!--/user/*All  表示拦截所有/user/开头，且具体映射名称以All结尾的映射-->
        <mvc:mapping path="/*"/>
        <mvc:mapping path="/**"/>
        <mvc:mapping path="/handleRun*"/>
        <!--设置拦截排除的路径，配置/**或/*，达到快速配置的目的-->
        <mvc:exclude-mapping path="/b*"/>
        <!--指定具体的拦截器类-->
        <bean class="MyInterceptor"/>
    </mvc:interceptor>
    <!--配置多个拦截器，配置顺序即为最终运行顺序-->
    <mvc:interceptor>
        <mvc:mapping path="/*"/>
        <bean class="MyInterceptor2"/>
    </mvc:interceptor>
    <mvc:interceptor>
        <mvc:mapping path="/*"/>
        <bean class="MyInterceptor3"/>
    </mvc:interceptor>
</mvc:interceptors>
```

## 多拦截器配置

接着我们来学习本章的最后一个内容，也就是多拦截器的配置。对于这一节，我们最好就想象我们进工厂过检查，每过一道增加一道防护的过程，这样易于我们对本章内容的理解。

首先，我们的拦截器链的运行顺序是参照配置的先后顺序，谁先配置谁先运行，同时后面的方法则是谁后运行谁就先执行对应的方法，这就相当于我们的最后过一道检查传防护眼镜，最后我们出去也是先返还防护眼镜的过程一样

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd14dcd45968994430caad175e06ac69c.png)

最后值得一提的是，我们的拦截器链里，一旦有任何一个进行了拦截，那么原始方法都不会执行，同时如果我们的拦截器链中中间的拦截链拦截了，那么其后的拦截链（包括他自己）不会执行，而前面的拦截工序里最后的执行完成的方法仍然会执行，这也跟过检查门一样，他都不要你穿衣服了，你也不用还，后面的装备没拿到，所以你也没必要还，准确来说都进不去，出去的时候就要返回鞋子给第一个检查门，非常好理解

最后我们这样的行为模式其实是一种责任链模式，其是一种行为模式，对于这种行为模式的具体介绍请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2eb1fda2b70d6c27a19537504e5722b3.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2229314e686400d745fbcf9be8e3512f.png)

# 异常处理

本节我们来学习SpringMVC中的异常处理，之前我们的异常处理都是使用我们的try...catch来做的，但是这种做法实在是太烂了，难看就算了，还不算适用，因此SpringMVC提供了对应的异常处理接口给我们，我们本章就来学习如何利用该接口进行对应的异常处理

## 异常处理器

在进行我们的案例的学习之前，我们先来看看我们的项目环境。

首先我们创建了一个ajax.jsp的页面文件，并写入代码如下

```
<%@page pageEncoding="UTF-8" language="java" contentType="text/html;UTF-8" %>

<a href="javascript:void(0);" id="testException">访问springmvc后台controller，传递Json格式POJO</a><br/>

<script type="text/javascript" src="${pageContext.request.contextPath}/js/jquery-3.3.1.min.js"></script>

<script type="text/javascript">
    $(function () {
        $("#testException").click(function(){
            $.ajax({
                contentType:"application/json",
                type:"POST",
                url:"save",
                data:'{"name":"Jock","age":"39"}',
                dataType:"text",
                //回调函数
                success:function(data){
                    alert(data);
                }
            });
        });
    });
</script>
```

我们这里做的事情就是用弹窗打印服务器返回给我们的对象的文本内容而已

web.xml不做改变，spring-mvc除了重新引入jQuery的对应库之外也不做改动，然后我们创建对应的User对象

接着我们写入我们的控制层的代码如下

```
package com.itheima.controller;

import com.itheima.domain.User;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

import java.util.ArrayList;
import java.util.List;

@Controller
public class UserController {

    @RequestMapping("/save")
    @ResponseBody
    public List<User> save(@RequestBody User user) {
        System.out.println("user controller save is running ...");

        //模拟业务层发起调用产生了异常
        int i = 1/0;

        User u1 = new User();
        u1.setName("Tom");
        u1.setAge(3);

        User u2 = new User();
        u2.setName("Jerry");
        u2.setAge(5);

        List<User> al = new ArrayList<>();

        al.add(u1);
        al.add(u2);

        return al;
    }
}

```

可以看到我们这里给予了一个异常，我们这样做是为了模拟业务层接口抛出异常的情况

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2682a8019e98508203f2a52cb30ba857.png)

然后我们要创建一个自己的异常处理类并令其实现HandlerExceptionResolver接口

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd2fc0dad4b729e599ad445c82e67b913.png)

那么我们可以写入代码如下

```
package com.itheima.exception;

import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerExceptionResolver;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Component
public class ExceptionResolver implements HandlerExceptionResolver {

    @Override
    public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        System.out.println("my exception is running ...");

        ModelAndView modelAndView = new ModelAndView();

        modelAndView.addObject("msg","出错了");

        modelAndView.setViewName("error.jsp");

        return modelAndView;
    }
}

```

我们细看这个方法，会发现这个方法和我们之前的拦截器里的第三个执行完成的方法非常像，事实上，他们的确是几乎一模一样，无非是有无返回值的区别罢了。我们这里之所以有返回值是因为我们对异常进行了处理之后，我们就应该返回一个新的页面，一般是一个错误的提示页面

当然不要忘了在这个类上加入对应的Component注解，令其加入到spring容器中，有同学可能会问spring容器怎么知道这个类是专门用于处理异常的类呢？很简单，因为这个类实现了HandlerExceptionResolver接口，这个接口本身也可以作为一个标识

最后我们的只要出现了异常都会进入到这个异常处理类中，我们还可以对我们的异常处理类做一个改动，令其能够实现对不同异常的不同处理，具体代码请看下图

## 异常分类管理

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE84c4cee56fb1cc44eb37dd3662c676e9.png)

那么我们可以写入我们的代码如下

```
package com.itheima.exception;

import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerExceptionResolver;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

//@Component
public class ExceptionResolver implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest request,
                                         HttpServletResponse response,
                                         Object handler,
                                         Exception ex) {
        System.out.println("my exception is running ...."+ex);
        ModelAndView modelAndView = new ModelAndView();
        if( ex instanceof NullPointerException){
            modelAndView.addObject("msg","空指针异常");
        }else if ( ex instanceof  ArithmeticException){
            modelAndView.addObject("msg","算数运算异常");
        }else{
            modelAndView.addObject("msg","未知的异常");
        }
        modelAndView.setViewName("error.jsp");
        return modelAndView;
    }
}

```

## 注解开发异常处理器

本节我们来学习使用注解来开发我们的异常处理器，我们这里要使用的注解是ControllerAdvice，其作用是设置当前类为异常处理类。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEfe529b50d9116e35c56e6f3cfd5be4fc.png)

那么我们可以新创建一个注解异常类并写入其对应的注解

然后我们接着要使用ExceptionHandler实现异常的分类管理，该注解可以设置指定的异常的处理方式。我们要设置一个对应的方法，然后在其上加入该注解，接着在括号内传入我们希望其进行处理的异常的class文件，注意，这种方法可以设置多个，以此来实现对不同异常的不同处理

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE728591d07b3c1ef391616662b1937037.png)

那么我们可以写入其代码如下

```
package com.itheima.exception;

import org.springframework.stereotype.Component;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.servlet.ModelAndView;

@Component
@ControllerAdvice
public class ExceptionAdvice {

    @ExceptionHandler(NullPointerException.class)
    //@ResponseBody
    public String doNullException(Exception e) {
        System.out.println("null exception...");
        return "error.jsp";
    }

}

```

这里我们要注意，如果我们想要得到其异常对象，可以再传入参数里加入一个异常的形参来获得，但是不可以加入其他的，否则该方法会直接失效。同时如果我们希望其返回一个页面，我们就返回对应的页面字符串就可以了。如果希望其返回我们自己的字符串创建的页面，那么就要加入ResponseBody注解

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0ba8ebf0a13dcb47207affd8eb4da6f1.png)

最后这注解处理器和非注解处理器的不同在于，前者加载得快，其可以拦截到页面传入的数据的类型转换异常，而非注解处理器则不能。

我们以后都是尽可能使用注解处理器来完成我们的需求的

## 异常处理解决方案

最后我们来学习我们的异常解决方案，我们之前已经做了对应的异常的处理了，但很不幸的是，这些处理方式都是没有用的，实际业务开发的时候我们肯定不可能采用这种方式的。

实际业务开发时，我们是要对我们的异常进行分类并设置对应的解决方式的，具体请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE014f4f856cfc016f9dcb3506e9a9a017.png)

然后我们可以自定义两个异常，并将我们的几乎所有的异常都往这两个异常中转换，然后对其做对应的处理。如果有意料之外的异常，那么我们就将该异常捕获记录，以后我们处理这个异常的时候还是要将这种异常转换为我们的自定义的异常的一种

## 自定义异常

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE451b26c56a196e6c02bdd59404737a92.png)

那么按照上面的思路，首先我们创建三个自定义的异常类，首先是我们的业务层的异常

```
package com.itheima.exception;

public class BusinessException extends RuntimeException{
    public BusinessException() {
    }

    public BusinessException(String message) {
        super(message);
    }

    public BusinessException(String message, Throwable cause) {
        super(message, cause);
    }

    public BusinessException(Throwable cause) {
        super(cause);
    }

    public BusinessException(String message, Throwable cause, boolean enableSuppression, boolean writableStackTrace) {
        super(message, cause, enableSuppression, writableStackTrace);
    }
}

```

我们做的事情就是令其继承运行时异常然后实现其内部的构造方法，其实也还是跟父类的方法差不多，没啥变化

依葫芦画瓢构建系统异常

```
package com.itheima.exception;

public class SystemException extends RuntimeException{
    public SystemException() {
    }

    public SystemException(String message) {
        super(message);
    }

    public SystemException(String message, Throwable cause) {
        super(message, cause);
    }

    public SystemException(Throwable cause) {
        super(cause);
    }

    public SystemException(String message, Throwable cause, boolean enableSuppression, boolean writableStackTrace) {
        super(message, cause, enableSuppression, writableStackTrace);
    }
}

```

最后我们创建对应的能够将我们的异常封装成我们的自定义异常的类

```
package com.itheima.service.impl;

import com.itheima.exception.SystemException;

import java.sql.SQLException;

public class UserServiceImpl {
    public void save() {
        try {
            throw new SQLException();
        } catch (SQLException e) {
            throw new SystemException("数据库连接超时!",e);
        }
    }
}

```

可以看到我们上面做的事情就是捕获我们的异常，然后写入一个字符串，接着将对应的异常传入到我们的自定义异常中，最后再返回这个新创建的对象

这就是一个将异常封装为我们的自定义异常对象的一个简单方法，按照这个思路，我们可以写入其他的将异常封装为我们的自定义的异常如下

```
package com.itheima.controller;

import com.itheima.domain.User;
import com.itheima.exception.BusinessException;
import com.itheima.exception.SystemException;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

import java.util.ArrayList;
import java.util.List;

@Controller
public class UserController {

    @RequestMapping("/save")
    @ResponseBody
    public List<User> save(@RequestBody User user) {
        System.out.println("user controller save is running ...");

        //模拟业务层发起调用产生了异常
        //int i = 1/0;
//        String str = null;
//        str.length();

        if(user.getName().trim().length() < 8){
            throw new BusinessException("对不起，用户名长度不满足要求，请重新输入");
        }
        if(user.getAge() < 0){
            throw new BusinessException("年龄须在0-100");
        }
        if(user.getAge() > 100){
            throw new SystemException("服务器连接失败，快叫人来维护");
        }

        User u1 = new User();
        u1.setName("Tom");
        u1.setAge(3);

        User u2 = new User();
        u2.setName("Jerry");
        u2.setAge(5);

        List<User> al = new ArrayList<>();

        al.add(u1);
        al.add(u2);

        return al;
    }
}

```

可以看到我们这里的情况是规定了发生什么情况就返回怎么样的我们的自定义的异常对象，并赋予了对应的异常信息

然后我们在异常处理类中写入对应的处理代码如下

```
package com.itheima.exception;

import org.springframework.stereotype.Component;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseBody;

@Component
@ControllerAdvice
public class ProjectExceptionAdvice {

    @ExceptionHandler(BusinessException.class)
    public String doNullException(Exception ex, Model m) {
        m.addAttribute("msg",ex.getMessage());
        return "error.jsp";
    }

    @ExceptionHandler(SystemException.class)
    public String doSystemException(Exception ex, Model m) {
        m.addAttribute("msg","服务器出现问题，你去找管理员吧");
        //非用户的规范问题不能传送给用户，要发送给运维人员，发给用户安抚性信息就可以了
        return "error.jsp";
    }

    @ExceptionHandler(Exception.class)
    public String doException(Exception ex, Model m) {
        m.addAttribute("msg",ex.getMessage());
        //需要记录日志，将ex对象保存起来
        return "error.jsp";
    }
}
```

最后我们的异常就可以被正常捕获到了，实际经过测试也发现这个代码的确没有什么问题，此时我们的案例就算是成功了，在本节中，我们最重要要学习的是异常处理的思想。

其思想就是自定义异常类，并将实际的异常转化为我们的自定义异常然后进行对应的处理

# 实用技术

那么到这里，我们的SpringMVC也快学习完了，我们最后的内容则没有怎么多的理论上的东西了，都是偏实用的。

## 文件上传

首先我们来讲讲我们的上传文件过程，首先，我们平时的上传首先是创建一个表单项，然后用户选择上传图片时，是将该部分的内容转换为流数据传给Tomcat，然后tomcat内部对这些数据进行对应的处理最后生成一个对应的文件。而SpringMVC则知道你们要干什么了，所以其将这一部分内容整出了一个接口出来用于实现，那么接下来我们就来重点讲解这个MutipartResolver这个接口

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE44552acb71f5b86013769d451c69d241.png)

首先该接口定义了文件上传过程中的相关操作，并对通用性操作进行了封装，其底层的实现类是CommonsMulitipartResovler。值得一提的是，SpringMVC并没有自主实现文件上传下载对应的功能其实调用apache的文件上传下载组件完成的功能的，因此我们在使用这个接口之前要将引入我们对应的apache的组件依赖

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE228e55778541118b8a22341633656f41.png)

那么我们可以写入其引入依赖的代码如下

```
<dependency>
  <groupId>commons-fileupload</groupId>
  <artifactId>commons-fileupload</artifactId>
  <version>1.4</version>
</dependency>
```

接着我们来正式实现其文件的上传下载，首先我们需要来介绍下我们当前的案例环境，我们这里没有使用jQuery的库，其他几乎没有什么特别的变动

然后我们写入我们的表单提交项如下，我们这里给我们的上传的LOGO的名字设置为file，同时我们点击上传，就回去寻找fileupload的方法并执行

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>

<form action="/fileupload" method="post" enctype="multipart/form-data">
    上传LOGO:<input type="file" name="file"/><br/>
    上传照片:<input type="file"/><br/>
    上传任意文件:<input type="file"/><br/>
    <input type="submit" value="上传"/>
</form>

```

然后我们来配置对应的上传类，我们这里采用配置bean的方式来实现，指定的实现类是CommonsMultipartResolver，其下有许多设置方法，比如最经典的一个设置上传文件的大小的方法，我们有需要就自己设置就可以了。同时由于这里是不支持设置运算符的，因此到底要多大还得自己计算一个合适的值然后设置进去，单位是kb

最后我们要记住的是，为了让我们的springMVC能够知道这个类是用于控制上传的类，因此其id只能为multipartResolver，写其他的都会令其失效

```
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <property name="maxUploadSize" value="1024000000"/>
</bean>
```

最后我们写入我们的控制层的代码如下

```
package com.itheima.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.multipart.MultipartFile;

import java.io.File;
import java.io.IOException;

@Controller
public class FileUploadController {

    @RequestMapping(value = "/fileupload")
    public String fileupload(MultipartFile file) throws IOException {
        System.out.println("file upload is running ..."+file);
        file.transferTo(new File("abc.png"));
        return "page.jsp";
    }
}
```

我们这里将我们的映射路径设置为对应的fileupload，令其能寻找到该方法，然后我们往其中加入了MultipartFile作为形参，并命名为file，这个file文件里就保存这我们上传的文件信息，我们可以通过调用该对象的对应的方法来令其保存到对应的路径或者是利用这些信息来创建一个指定的文件。

注意，这里我们页面表单设置的名字和我们的形参设置的名字一定要保持一致，否则将无法正确获取到上传的信息

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc57e16e7f46f7474cf7730e3f20cd24b.png)

经过上面的步骤，我们就实现了在SprnigMVC中利用其提供的接口来实现上传了，这可比之前我们学习的方法方便多了

## 文件上传注意事项

接着我们来学习文件上传时的注意事项，文件上传时，最常出现的问题有文件名命名问题、保存路径问题等。而这些问题在我们的MultipartFile对象里都有对应的方法提供给我们使用，我们可以利用这些方法来解决这些问题

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE44fd2fa0fb3b5bba68d088eb5f6f2d2d.png)

那么为了解决对应的问题，我们可以写入我们的代码如下

```
package com.itheima.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.multipart.MultipartFile;

import javax.servlet.http.HttpServletRequest;
import java.io.File;
import java.io.IOException;

@Controller
public class FileUploadController {

    @RequestMapping(value = "/fileupload")
    //参数中定义MultipartFile参数，用于接收页面提交的type=file类型的表单，要求表单名称与参数名相同
    public String fileupload(MultipartFile file,MultipartFile file1,MultipartFile file2, HttpServletRequest request) throws IOException {
        System.out.println("file upload is running ..."+file);
//        MultipartFile参数中封装了上传的文件的相关信息
//        System.out.println(file.getSize());
//        System.out.println(file.getBytes().length);
//        System.out.println(file.getContentType());
//        System.out.println(file.getName());
//        System.out.println(file.getOriginalFilename());
//        System.out.println(file.isEmpty());
        //首先判断是否是空文件，也就是存储空间占用为0的文件
        if(!file.isEmpty()){
            //如果大小在范围要求内正常处理，否则抛出自定义异常告知用户（未实现）
            //获取原始上传的文件名，可以作为当前文件的真实名称保存到数据库中备用
            String fileName = file.getOriginalFilename();
            //设置保存的路径
            String realPath = request.getServletContext().getRealPath("/images");
            //保存文件的方法，指定保存的位置和文件名即可，通常文件名使用随机生成策略产生，避免文件名冲突问题
            file.transferTo(new File(realPath,file.getOriginalFilename()));
        }
        //测试一次性上传多个文件
        if(!file1.isEmpty()){
            String fileName = file1.getOriginalFilename();
            //可以根据需要，对不同种类的文件做不同的存储路径的区分，修改对应的保存位置即可
            String realPath = request.getServletContext().getRealPath("/images");
            file1.transferTo(new File(realPath,file1.getOriginalFilename()));
        }
        if(!file2.isEmpty()){
            String fileName = file2.getOriginalFilename();
            String realPath = request.getServletContext().getRealPath("/images");
            file2.transferTo(new File(realPath,file2.getOriginalFilename()));
        }
        return "page.jsp";
    }
}

```

我们上面的代码就完成了第文件的处理，将上传的文件存放在我们指定的位置，并且获得其最开始的后缀来赋予其后缀，同时获得其初始的名字赋予其原来的名字

同时注意这里我们可以设置新的路径的前提是我们确实创建了这么一个路径，否则是没法用的

最后我们来提一下同名问题的解决，实际上，我们可以用数据库来保存文件的名字，而实际传送过来的文件的名字则用我们自己生成的规则字符串来代替，这样就能避免重名问题，同样也能不失去文件的原来的名字，获取名字的简单方法是使用UUID，这个我们用到了的时候再具体讲吧

## Restful

本节我们来学习Restful，其实一种网络资源的访问风格，注意是风格而不是规范，这是一个伏笔。我们传统风格访问我们的资源时，我们总是要加上地址和不能够写入我们的具体的传入的数据，非常麻烦，而且会被别人看到，不安全。此时我们的Rest风格访问路径就出现了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2c4ed49ae8147a73caaf2c84d8177725.png)

Rest风格的访问路径是直接在对应的页面后面加上我们要传入的数据，只有数据，所以即使看到了也整不明白这是干啥的，而Restful则是按照这个风格来访问网络资源的方式的统称

Rest行为的约定方式有四种，分别是负责查询的GET方式、负责保存的POST方式、负责更新的PUT方式以及负责删除的DELETE方式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0b9d1d89fdcf938477441f4bd8112af1.png)

值得一提的是，上述的行为都是约定方式，也就是说，如果你非要利用GET做保存，也是可以的，只是我们不推荐。

那么我们现在就来正式在SpringMVC中来开发我们的Restful

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEdbe57add42f06cc206428d211d26f6a5.png)

我们的项目环境本身没什么变化，所以这里就不再介绍一遍了，然后我们创建对应的控制类并写入代码如下

```
//rest风格访问路径完整书写方式
@RequestMapping("/user/{id}")
//使用@PathVariable注解获取路径上配置的具名变量，该配置可以使用多次
public String restLocation(@PathVariable Integer id){
    System.out.println("restful is running ....");
    return "success.jsp";
}
```

我们这里采用的就是rest风格的路径的完整书写方式，直接写我们要访问的路径并写入通过/分割，然后最后用大括号写入我们要传入的数据的变量名，在方法中则是以相同类型的参数名获得这个数据，注意这里形参名和映射中的名字一定要相同，否则会无法获得。同时我们这里要使用PathVariable注解，该注解能够让我们的形参在数据传入时获得对应的值，加了这个注解我们的形参才可以获得正确的值

这里值得一提的是，我们写入的user同时也是我们的路径名称，因此如果我们写入上面的代码，要令其正确跳转到我们的页面，我们就需要在webapp文件夹中创建一个user文件夹然后将success.jsp移动到这里去才可以。

那么我们在地址栏中输入如下地址就可以访问我们的服务器

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd3441e5081c1b7e8ba829bbcbecf10ad.png)

同时我们也可以不必每次都指定路径，我们可以将指定路径的动作放到类中，使用RequestMapping注解，在括号内中填入我们想要进入的路径就行了，然后其下的方法就都默认要使用这个路径来进入了，我们后续的方法就只需要直接写对应的数据值就完了

```
//rest风格访问路径简化书写方式，配合类注解@RequestMapping使用
@RequestMapping("{id}")
public String restLocation2(@PathVariable Integer id){
    System.out.println("restful is running ....get:"+id);
    return "success.jsp";
}
```

同时我们有时需要返回自己的字符串创建的页面而非字符串对应的jsp文件，此时我们就需要用到ResponseBody注解，我们一般把他加到方法上，而实际上其还可以用于类中，被修饰的类相当于其下的方法都被这个注解修饰了。而Controller注解又是我们必须要写的，springMVC考虑到了这一点，将这两个注解合并到了一个注解中，这个注解就是RestController。

- Restful风格配置

开始本章之前，我们要先知道，我们配置了一个表单页面用于进行测试

```
<%@page pageEncoding="UTF-8" language="java" contentType="text/html;UTF-8" %>
<h1>restful风格请求表单</h1>
<%--切换请求路径为restful风格--%>
<%--GET请求通过地址栏可以发送，也可以通过设置form的请求方式提交--%>
<%--POST请求必须通过form的请求方式提交--%>
<form action="/user/1" method="post">
    <input type="submit"/>
</form>
```

如果我们想要某个方法区只处理某些特定的请求的话，那么我们就要在RequestMapping中，具体配置其method属性，通过后面属性设定其专门处理的请求

```
//接收GET请求配置方式
@RequestMapping(value = "{id}",method = RequestMethod.GET)
//接收GET请求简化配置方式
@GetMapping("{id}")
public String get(@PathVariable Integer id){
    System.out.println("restful is running ....get:"+id);
    return "success.jsp";
}
```

GET请求如此，POST请求也是如此，当然我们的表单项里提交的方法要写成是post

```
//接收POST请求配置方式
@RequestMapping(value = "{id}",method = RequestMethod.POST)
//接收POST请求简化配置方式
@PostMapping("{id}")
public String post(@PathVariable Integer id){
    System.out.println("restful is running ....post:"+id);
    return "success.jsp";
}
```

但是PUT请求就需要更多的改动才能生效，其中之一就是需要对我们的表单页面进行对应的改动，改动如下

```
<%@page pageEncoding="UTF-8" language="java" contentType="text/html;UTF-8" %>
<h1>restful风格请求表单</h1>
<%--切换请求路径为restful风格--%>
<%--GET请求通过地址栏可以发送，也可以通过设置form的请求方式提交--%>
<%--POST请求必须通过form的请求方式提交--%>
<form action="/user/1" method="post">
    <%--当添加了name为_method的隐藏域时，可以通过设置该隐藏域的值，修改请求的提交方式，切换为PUT请求或DELETE请求，但是form表单的提交方式method属性必须填写post--%>
    <%--该配置需要配合HiddenHttpMethodFilter过滤器使用，单独使用无效，请注意检查web.xml中是否配置了对应过滤器--%>
    <input type="text" name="_method" value="PUT"/>
    <input type="submit"/>
</form>
```

我们这里给其加上了新的输入框并在value处设置PUT方式，这里的type属性一般是hidden，其代表不会在浏览器中显示，我们实际开发中一般也不会让用户自身去知道有这玩意的存在，但是我们这里为了演示方便，所以我们允许给其设置为text，而且我们的method的方式仍然是post

然后我们在web.xml中配置我们的代码如下

```
<filter>
  <filter-name>HiddenHttpMethodFilter</filter-name>
  <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
  <filter-name>HiddenHttpMethodFilter</filter-name>
  <servlet-name>DispatcherServlet</servlet-name>
</filter-mapping>
```

这个代码代表的意思是我们又在我们的容器中加入一个过滤器，这个过滤器类的类名是HiddenHttpMethodFilter类，而我们要过滤的方法是所有要经过DispatcherServlet的请求，简而言之就是要经过springmvc的请求，相当于我们配置了两个过滤器，一个专门过滤springmvc的请求，另一个过滤所有的请求，配置了这个过滤器我们就可以将进入到springmvc中的put方法给拦截下来并令其执行对应的方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5500af38006e21d7e3d240e23e7064d3.png)

配置完了这个之后，我们的请求就可以正确运行了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2d70a2411211c321d81415f93b9d46d9.png)

最后我们可以写入我们的分别调用四个方法的代码如下

```
//接收GET请求配置方式
@RequestMapping(value = "{id}",method = RequestMethod.GET)
//接收GET请求简化配置方式
@GetMapping("{id}")
public String get(@PathVariable Integer id){
    System.out.println("restful is running ....get:"+id);
    return "success.jsp";
}

//接收POST请求配置方式
@RequestMapping(value = "{id}",method = RequestMethod.POST)
//接收POST请求简化配置方式
@PostMapping("{id}")
public String post(@PathVariable Integer id){
    System.out.println("restful is running ....post:"+id);
    return "success.jsp";
}

//接收PUT请求简化配置方式
@RequestMapping(value = "{id}",method = RequestMethod.PUT)
//接收PUT请求简化配置方式
@PutMapping("{id}")
public String put(@PathVariable Integer id){
    System.out.println("restful is running ....put:"+id);
    return "success.jsp";
}

//接收DELETE请求简化配置方式
@RequestMapping(value = "{id}",method = RequestMethod.DELETE)
//接收DELETE请求简化配置方式
@DeleteMapping("{id}")
public String delete(@PathVariable Integer id){
    System.out.println("restful is running ....delete:"+id);
    return "success.jsp";
}
```

当然，细心的同学可能会发现，我们这里似乎还多了另外的注解，没错，这种注解就是我们的原来的接受对应请求的简化配置方式，用了这个注解，我们就不用再配置什么特别的属性了，直接用对应的注解然后括号内传入对应的我们想要设置给数据的名字的字符串就行

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4ad99add225efd23629cd8c45d5fdd76.png)

- postman工具安装与使用

最后我们来介绍一款可以发送Restful风格请求的工具，其可以方便开发调试，这款工具就是postman

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc8d165a1d2f56222fa25ed02109b45ee.png)

Restful风格的请求有许多种，但是我们最常用的就是我们最开始介绍的那四种，因此我们这里也只演示这四种

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6c394ac69bc41e680e98d71835b2efd3.png)

开启服务器之后选择对应的请求然后输入网址发送就完了，要填入更多参数也可以自己设置，更多的内容我们就等到以后我们再说