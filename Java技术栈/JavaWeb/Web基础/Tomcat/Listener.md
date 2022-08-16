- 监听器的介绍

在学习监听器之前，我们要先学习观察者设计模式，因为所有的监听器都是基于观察者设计模式设计的。

观察者设计模式有三个组组成部分，分别是事件源（触发事件的对象）、事件（触发的动作，其封装了事件源）、监听器（当事件源出发事件后，可以完成一些功能）。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe0eabafeaeacb9be08eb00c10af523c0.png)

在程序当中，我们可以对对象的销毁创建、域对象中属性的变化以及会话相关的内容进行监听。Servlet规范中一共提供了共计8个监听器并且都以接口形式提供，具体功能需要我们自己去实现

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2186ee3cd5fc9834b6823c2117fbc1cc.png)

- 监听对象的监听器介绍

监听对象很多种，有监听对象的，有监听对象中的属性的 ，本节我们先来介绍监听对象的

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE391748e76380053feb79301560fa36b7.png)

首先是ServletContextListener，其是用于监听ServletContext对象的创建和销毁，其中ServletContextEvent代表的是事件对象，也是我们的方法的参数。事件对象中封装了事件源，也就是ServeltContext，真正的事件指的是创建或销毁ServeltContext

依葫芦画瓢还有HttpSessionListener

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf8970acb1ff74277994da4a00ac76406.png)

以及ServletRequest

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6cea3e9705f487f62031c573f3cb1b7f.png)

- 监听域对象属性变化的监听器介绍

这个东西的作用就跟标题说的一模一样其实，这里就不多谈了。直接看图吧，首先是ServletContextAttributeListener

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE4cae3d49860a98beaeb89fa99354248d.png)

然后是HttpSessionAttributeListener

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6788731d496712cfe33ff5c4049ededc.png)

最后是ServletRequestAttributeListener

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc86f3b4f0b876d9e33d2b34fc00f25aa.png)

- 监听会话相关的感知型监听器介绍

一般来说，我们的监听器在重写好了我们的方法之后，是要进行配置才能够进行相关的使用的。而感知性监听器的不同之处在于，其可以不用配置直接用

首先是HttpSessionBindingListener

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEbb5a70e28052b309ebaa89714a761d80.png)

然后是

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE70ddcedb2a78bab3af9cf3db3382bd44.png)

- 监听器的使用

经过我们上面的学习，我们也会发现虽然监听器的种类很多，但他们都大同小异，所以关于监听器的使用，我们这里也是只讲一个重点的，后面的各位依葫芦画瓢自己去试就可以了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE96b8c0b201735ddef227fbafe657ed31.png)

那么我们可以写入其测试代码如下

```
package com.itheima.listener;

import javax.servlet.ServletContext;
import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;

/*
    ServletContext对象的创建和销毁的监听器
 */
@WebListener
public class ServletContextListenerDemo implements ServletContextListener {

    /*
        ServletContext对象创建的时候执行此方法
     */
    @Override
    public void contextInitialized(ServletContextEvent servletContextEvent) {
        System.out.println("监听到了对象的创建");

        //获取对象
        ServletContext servletContext = servletContextEvent.getServletContext();
        System.out.println(servletContext);
    }

    /*
        ServletContext对象销毁的时候执行此方法
     */
    @Override
    public void contextDestroyed(ServletContextEvent servletContextEvent) {
        System.out.println("监听到了对象的销毁...");
    }
}

```

这里我们重写好了我们监听器的方法之后还要对监听器进行配置，这里配置的方式同样有两种，注解和文件配置，我们这里采用注解的方式，直接写上@WebListener就完了，后面什么都不用加

- ServletContextAttributeListener监听器的使用

我们接下来来学习ServletContextAttributeListener监听器的使用，至于会话相关的监听器我们就不演示了，因为我们直到会话相关的监听器是不需要配置的，定义了就可以用了。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE604311727d7a0bd6e344eb4244a5f524.png)

那么我们可以构造其测试代码如下

```
package com.itheima.listener;

import javax.servlet.ServletContext;
import javax.servlet.ServletContextAttributeEvent;
import javax.servlet.ServletContextAttributeListener;
import javax.servlet.annotation.WebListener;

/*
    应用域对象中的属性变化的监听器
 */
@WebListener
public class ServletContextAttributeListenerDemo implements ServletContextAttributeListener {

    /*
        向应用域对象中添加属性时执行此方法
     */
    @Override
    public void attributeAdded(ServletContextAttributeEvent servletContextAttributeEvent) {
        System.out.println("监听到了属性的添加...");

        //获取应用域对象
        ServletContext servletContext = servletContextAttributeEvent.getServletContext();
        //获取属性
        Object value = servletContext.getAttribute("username");
        System.out.println(value);
    }

    /*
        向应用域对象中移除属性时执行此方法
     */
    @Override
    public void attributeRemoved(ServletContextAttributeEvent servletContextAttributeEvent) {
        System.out.println("监听到了属性的移除");

        //获取应用域对象
        ServletContext servletContext = servletContextAttributeEvent.getServletContext();
        //获取属性
        Object value = servletContext.getAttribute("username");
        System.out.println(value);
    }

    /*
        向应用域中替换属性时执行此方法
     */
    @Override
    public void attributeReplaced(ServletContextAttributeEvent servletContextAttributeEvent) {
        System.out.println("监听到了属性的替换");

        //获取应用域对象
        ServletContext servletContext = servletContextAttributeEvent.getServletContext();
        //获取属性
        Object value = servletContext.getAttribute("username");
        System.out.println(value);
    }
}

```

我们为了测试，我们在上一个ServletContextListenerDemo类中，我们获取到了ServletContext应用域对象，然后我们的在这个应用域对象里做了添加、替换和移除等动作

```
//添加属性
servletContext.setAttribute("username","zhangsan");

//替换属性
servletContext.setAttribute("username","lisi");

//移除属性
servletContext.removeAttribute("username");
```

然后我们启动服务器，可以获得我们想要的结果，说明我们已经成功了。