- 整合junit测试业务层接口

本节我们来学习整合junit来测试我们的业务层的接口，这里就要利用我们的junit类了，那么我们首先在对应的test包里，创建com包，再创建itheima，最后创建service，然后写入UserServiceTest类

然后我们要使用Junit使用的RunWith注解和ContextConfiguration注解，这一部分的知识忘了的同学可以回去看以前的笔记进行复习

```
package com.itheima.service;

import com.github.pagehelper.PageInfo;
import com.itheima.domain.User;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import java.util.Date;
import java.util.List;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:applicationContext.xml")
public class UserServiceTest {

    @Autowired
    private UserService userService;

    @Test
    public void testSave(){
        User user = new User();
        user.setUserName("Jock");
        user.setPassword("root");
        user.setRealName("Jockme");
        user.setGender(1);
        user.setBirthday(new Date(333333000000L));

        userService.save(user);
    }

    @Test
    public void testDelete(){
        User user = new User();
        userService.delete(15);
    }

    @Test
    public void testUpdate(){
        User user = new User();
        user.setUuid(16);
        user.setUserName("Jockme");
        user.setPassword("root");
        user.setRealName("JockIsMe");
        user.setGender(1);
        user.setBirthday(new Date(333333000000L));

        userService.update(user);
    }

    @Test
    public void testGet(){
        User user = userService.get(16);
        System.out.println(user);
    }

    @Test
    public void testGetAll(){
        PageInfo<User> all = userService.getAll(2, 2);
        System.out.println(all);
        System.out.println(all.getList().get(0));
        System.out.println(all.getList().get(1));
    }

    @Test
    public void testLogin(){
        User user = userService.login("Jockme", "root");
        System.out.println(user);
    }
}

```

然后我经过测试会发现这些都是可以运行的，而且我们的分页操作也玩得来

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf4e7a5a20222282813d10e5c913dba3a.png)

最后我要提一下，我们现在的测试其实仍然是不规范的，因为我们是自己连接我们自己的数据库，企业开发中我们实际上是在我们的test包中也搞一个我们的配置文件，然后我们在test包中去用新的连接去进行测试的，而原本的项目中的配置文件是另外一个属于他自己的连接，这个我们一般是不做什么改动的。这些知识我们先了解下就可以了

- Rest风格开发SpringMVC

本节我们来用Rest风格来开发SpringMVC，首先我们来看看我们的步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5ea2924cdc1c34101d66a4dbce4c3f98.png)

首先我们在webapp下的WEB-INF的包下创建web.xml，然后写入代码如下

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
        http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         version="3.0">

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

其实也是经典的添加过滤器和设定字符集

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEad267d59ba883dc4cb44df5c20cbb260.png)

然后我们就要创建我们的spring-mvc配置文件了，我们在资源路径下创建spring-mvc.xml然后写入其代码如下

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <mvc:annotation-driven/>

    <context:component-scan base-package="com.itheima.controller"/>


</beans>
```

我们这里写入的代码是简单的开启注解驱动以及设定我们要扫描的包，也就是Controller包，但是我们之前的spring的配置文件里里面扫描的包可是包含Controller的，我们不能让同一个类在spring容器中和springmvc容器中都加载一次，因此我们需要在spring中做排除代码，将Controller层给排除掉

那么我们可以将applicationContext.xml的代码修改如下

```
<context:component-scan base-package="com.itheima">
    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

可以看到我们这里是使用了排除过滤器，将所有被Controler修饰的类都排除掉，不加载到Spring容器中来。

然后我们正式来做SpringMVC的控制层的代码，首先我们在其类中填入RestController，其代表我们的不但要开启Rest风格的访问，而且还要将该类加入到我们的容器中去。然后我们配置其映射路径，使用注解RequestMapping，并且在括号中写入/user，代表我们的虚拟路径即为user

```
@RestController
@RequestMapping("/user")
public class UserController {
```

然后我们在下面对应的方法中用对应的注解定义其对应的方法，这里我们的删除方法由于需要传入一个对应的uuid数据，我们需要让这个数据被我们的形参接收到，因此我们在对应的形参前要加入PathVariable注解，这样才能让我们的形参正确获得我们所需要的值

```
@PostMapping
public boolean save(User user){
    System.out.println("save ..."+user);
    return true;
}

@PutMapping
public boolean update(User user){
    System.out.println("save ..."+user);
    return true;
}

@DeleteMapping("/uuid")
public boolean delete(@PathVariable Integer uuid){
    System.out.println("save ..."+uuid);
    return true;
}

```

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE85f64c5023bd7bd40edf3433a0eab881.png)

如果我们想要让我们的方法获得多个参数的值，只需要在对应的注解的括号中按照/{}的格式多写入几个参数就行了，当然我们要注意的是，我们大括号内填入的参数名还和我们的形参名保持一致，且我们要获得值的形参前都应该加入PathVariable注解

```
@GetMapping("/{uuid}")
public User get(@PathVariable Integer uuid){
    System.out.println("get ..."+uuid);
    return null;
}

@GetMapping("/{page}/{size}")
public List getAll(@PathVariable Integer page,@PathVariable Integer size){
    System.out.println("getAll ..."+page+","+size);
    return null;
}

@PostMapping("login")
public User login(String userName,String password){
    System.out.println("login ..."+userName+","+password);
    return null;
}
```

最后我们要注意，我们的登录方法使用的是PostMapping注解，我们之前说过Post一般不这么用——在Rest风格里，但这毕竟只是一个风格，我们不一定要遵守，主要看我们的实际的业务需求，因此我们这里使用Post方法来传输我们的数据。之所以不用Get方式，是因为Get方式不隐藏数据，这样我们的数据就不安全，因此我们要使用Post方式

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf17438a540d188b5a72ae0328075aaf7.png)

最后我们用我们的postman工具经过测试我们会发现我们的方法都的确是可以正确运行的，此时就说明我们本节的案例已经完成了

- Spring整合SpringMVC

接着我们来学习如何用spring整合SpringMVC

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE939b7e48be26640b4d4bd27450aa5759.png)

要用Spring整合SpringMVC，我们就需要在web.xml加载时将Spring环境也加载了，因此我们要对web.xml加入一些代码来令其完成这个工作

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8af71a6ce5057f8ae92fc03d0c495153.png)

我们的一个简单想法就是使用监听器来帮助我们完成这项工作，我们通过监听器来加载我们的spring核心配置文件，我们首先引入我们的监听器代码

```
<!--启动服务器时，通过监听器加载spring运行环境-->
<listener>
  <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

然后还有一点要做的事情是我们需要给我们的监听器加入其要加载的文件，所以我们利用context-param完成这件事情，我们这里令其固定加载我们的springmvc的核心配置文件

```
<context-param>
  <param-name>contextConfigLocation</param-name>
  <param-value>classpath*:applicationContext.xml</param-value>
</context-param>
```

（关于监听器的知识已经忘了差不多了，其复习的时候别忘了顺便去看看监听器的原理）

然后我们在Controller层中写入我们的代码如下，这里我们做的事情就是对业务层的代码的再调用

```
package com.itheima.controller;

import com.github.pagehelper.PageInfo;
import com.itheima.domain.User;
import com.itheima.service.UserService;
import org.omg.CORBA.PUBLIC_MEMBER;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/user")
public class UserController {

    @Autowired
    private UserService userService;

    @PostMapping
    public boolean save(User user){
        return userService.save(user);
    }

    @PutMapping
    public boolean update(User user){
        return userService.update(user);
    }

    @DeleteMapping("/{uuid}")
    public boolean delete(@PathVariable Integer uuid){
        return userService.delete(uuid);
    }

    @GetMapping("/{uuid}")
    public User get(@PathVariable Integer uuid){
        return userService.get(uuid);
    }

    @GetMapping("/{page}/{size}")
    public PageInfo<User> getAll(@PathVariable Integer page, @PathVariable Integer size){
        return userService.getAll(page,size);
    }

    @PostMapping("login")
    public User login(String userName,String password){
        return userService.login(userName,password);
    }
}
```

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf6a98a0bd80730e01dbc4511d121bbb7.png)

最后我们发送Rest风格的访问到我们的服务器上，我们使用的PostMan软件，这里我们对应的访问的格式如下

首先是新增，http://localhost/user?userName=Jock&password=1234&realName=JockisSuperMan&gender=1&birthday=1999/09/09

然后是登录，http://localhost/user/login?userName=Jockme&password=root

接着是修改，http://localhost/user?userName=Jock123321&password=1234&realName=JockisSuperMan&gender=1&birthday=1999/09/09&uuid=17

然后是删除，http://localhost/user/17

接着是查询单个，http://localhost/user/1

最后是分页查询，http://localhost/user/1/2

最后其对应的查询风格如下所示

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5e4f8f7ed6eaf34f733d90b086672dec.png)

- 表现层数据封装

当我们做到这里的时，或许有的同学就已经觉得我们的案例已经全部实现完毕了，但其实不是的，我们还有一项最重要的步骤要做，就是表现层的数据封装，这个不做，那么就无法与前端进行一个很好的对接，到时候别人就觉得我们是业余的，这样肯定不行。

为什么我们要进行表现层的数据封装？这是因为我们的服务器返回的结果可能是各种各样的，而且随着不同的情况变化，也会产生各种不同的数据到前台，如果不对这些返回的数据进行封装，使其变成一个统一的格式，那么前台人员看到这个就头大，不知道这个数据代表什么意思。有的同学可能觉得true和false就能代表成功和失败就挺好的，但是有可能会出现两个方法都是返回这个来代表成功或者失败的情况，到时候你要前端人员怎么办呢你说是吧

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6d70b962e29f6b242f518c5ea919e54b.png)

那么我们要如何进行表现层的数据封装呢？我们可以创建一个用于封装数据的类，姑且称之为Result，其中我们的返回数据的设计一般有三部分，分别是状态、数据、消息。将其返回前台，前台的人员就可以看到你的返回状态的数据内容。同时我们的业务不同我们也要设计不同的状态码来表示我们方法执行的情况，这其实就类似于浏览器中返回给我们的404状态码一样，主要是为了给前端人员看的

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc45b0abb369b048d67a16e02aebf33e8.png)

那么我们首先创建我们的Controller层中创建我们的封装数据类的Result对象，并赋予其以下属性

```
//操作结果编码
private Integer code;
//操作数据结果
private Object data;
//消息
private String message;
```

这里我们给了结果编码、数据、消息这三个属性，然后我们提供其对应的各种构造方法和setandget方法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE46b783cc4f0b7438b17dd887410cae6a.png)

然后我们要创建我们对应的编码类用于设定我们的编码，我们这里只设定的操作结果编码作为演示，后续我们根据我们的业务需求可以设计其他的操作结果编码

```
package com.itheima.controller.results;

public class Code {
    //操作结果编码
    public static final Integer SAVE_OK = 20011;
    public static final Integer UPDATE_OK = 20021;
    public static final Integer DELETE_OK = 20031;
    public static final Integer GET_OK = 20041;

    public static final Integer SAVE_ERROR = 20010;
    public static final Integer UPDATE_ERROR = 20020;
    public static final Integer DELETE_ERROR = 20030;
    public static final Integer GET_ERROR = 20040;

    //系统错误编码

    //操作权限编码

    //校验结果编码
}

```

接着我们去表现层里来具体对我们的返回的结果进行封装，我们先从保存方法下手，首先我们的将我们的结果返回，用三元运算符对其进行判断，我们返回对应的编码并用Result封装，同时我们也要将我们的返回值改为我的Result数据封装对象

```
@PostMapping
public Result save(User user){
    boolean flag = userService.save(user);
    return new Result(flag ? Code.SAVE_OK:Code.SAVE_ERROR);
}
```

依葫芦画瓢可以写入其他类似方法的对应的代码如下

```
@PutMapping
public Result update(User user){
    boolean flag = userService.update(user);
    return new Result(flag ? Code.UPDATE_OK:Code.UPDATE_ERROR);
}

@DeleteMapping("/{uuid}")
public Result delete(@PathVariable Integer uuid){
    boolean flag = userService.delete(uuid);
    return new Result(flag ? Code.DELETE_OK:Code.DELETE_ERROR);
}
```

注意我们这里是要去我们对应的封装类中提供只用Code的构造方法的，不过我们一开始的时候就提供了所有的构造方法，所以这里我们就不用这这件事了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE59c595a37ad141f1ecfeb2d815054522.png)

然后我们接着要封装我们的数据对象，我们这里为了能够令其封装对应的参数和编码，所以我们要提供编码和数据的构造方法，然后我们判断数据是否为空，若为空则返回错误编码和数据对象，为真则返回正确编码和数据对象并进行封装后返回

```
@GetMapping("/{uuid}")
public Result get(@PathVariable Integer uuid){
    User user = userService.get(uuid);
    return new Result(null != user ?Code.GET_OK: Code.GET_ERROR,user);
}

@GetMapping("/{page}/{size}")
public Result getAll(@PathVariable Integer page, @PathVariable Integer size){
    PageInfo<User> all = userService.getAll(page, size);
    return new Result(null != all ?Code.GET_OK: Code.GET_ERROR,all);
}

```

最后我们经过测试会发现这个代码是的确有用，用postman发送访问可以获得的对象的对象里可以看到内部的编码，这就说明我们的案例已经成功了

- 问题消息处理

现在我们来完成问题消息的处理，我们这里需要自定义我们的异常，令对应的程序中的异常转换为我们自定义的异常，这一环节我们之前也学习过了，但是这里不同的是，我们不可以让我们的自定义异常对象直接返回，我们这里需要让我们的自定义的异常对象也封装成我们的统一的数据格式，否则前台人员整不明白，这也是我们本节最重要的思想。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEae83f869bc2e8dfb51547c6f377592ea.png)

那么我们首先要创建对应的异常对象，在其下提供code属性，用于存放错误编码，然后提供对应的构造方法，然后我们还创建了一个系统异常类，这个类也是这样构造了，不过我们这里只是创建了，就不演示其使用了，我们主要演示第一个异常的使用

那么我们可以写入其代码如下

```
package system.exception;

public class BusinessException extends RuntimeException{

    private Integer Code;

    public Integer getCode() {
        return Code;
    }

    public void setCode(Integer code) {
        Code = code;
    }

    public BusinessException() {
    }

    public BusinessException(Integer code) {
        Code = code;
    }

    public BusinessException(String message,Integer code) {
        super(message);
        Code = code;
    }

    public BusinessException(String message, Throwable cause,Integer code) {
        super(message, cause);
        Code = code;
    }

    public BusinessException(Throwable cause,Integer code) {
        super(cause);
        Code = code;
    }

    public BusinessException(String message, Throwable cause, boolean enableSuppression, boolean writableStackTrace,Integer code) {
        super(message, cause, enableSuppression, writableStackTrace);
        Code = code;
    }
}

```

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc1123c8c0dc4c18c52f29b3cb70f5f82.png)

然后我们创建对应的异常消息处理类，我们这里令其加入到spring容器中，然后给其加入增强注解

```
package com.itheima.controller.interceptor;

import com.itheima.controller.results.Result;
import org.springframework.stereotype.Component;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseBody;
import system.exception.BusinessException;

@Component
@ControllerAdvice
public class ProjectExceptionAdvice {

    @ExceptionHandler(BusinessException.class)
    @ResponseBody
    public Result doBusinessException(BusinessException e){
        return new Result(e.getCode(),e.getMessage());
    }
    
}

```

接着在对应的方法上传入我们要处理的异常对象方法，令其返回一个新的我们的自己定义的数据类型的对象。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE48b091b8334f9ddb8479a194f9bbe4ed.png)

最后我们在控制层中写入其测试的异常代码如下，假设传入的uuid为10就抛出我们自定义的异常

```
@GetMapping("/{uuid}")
public Result get(@PathVariable Integer uuid){
    User user = userService.get(uuid);
    if(uuid==10) throw new BusinessException("查询出错，警告",Code.GET_OK);
    return new Result(null != user ?Code.GET_OK: Code.GET_ERROR,user);
}
```

最后经过测试我们会发现我们的这份代码的确能够让我们获得我们想要的正确的统一格式的异常信息，此时就说明我们的案例已经成功了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE9e4fcdd3d039ff99978b3ce06931dba9.png)

那么到此为止，我们SSM的内容就全部讲完了，此时我们已经完成了这个案例的所有内容

- 纯注解开发SSM

最后我们来学习一个补充内容，纯注解的方式来开发我们的SSM，我们将这一节独立出来为一章，其实主要内容还是对以前内容的复习

首先我们来看看步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb550248c8b17351e41b23f7267af025a.png)

那么我们的目标其实就是要将我们的所有的配置文件都干掉，令其全部变为注解形式，首先我们先来干掉我们的UserDao.xml，我们可以用注解的形式将查询语句直接放置到我们的对应的接口中，那么我们可以写入我们的代码如下

```
package com.itheima.dao;

import com.itheima.domain.User;
import org.apache.ibatis.annotations.*;

import java.util.List;

public interface UserDao {
    /**
     * 添加用户
     * @param user
     * @return
     */
    @Insert("insert into user(userName,password,realName,gender,birthday)values(#{userName},#{password},#{realName},#{gender},#{birthday})")
    public boolean save(User user);

    /**
     * 修改用户
     * @param user
     * @return
     */
    @Update("update user set userName=#{userName},password=#{password},realName=#{realName},gender=#{gender},birthday=#{birthday} where uuid=#{uuid}")
    public boolean update(User user);

    /**
     * 删除用户
     * @param uuid
     * @return
     */
    @Delete("delete from user where uuid = #{uuid}")
    public boolean delete(Integer uuid);

    /**
     * 查询单个用户信息
     * @param uuid
     * @return
     */
    @Select("select * from user where uuid = #{uuid}")
    public User get(Integer uuid);

    /**
     * 查询全部用户信息
     * @return
     */
    @Select("select * from user")
    public List<User> getAll();


    /**
     * 根据用户名密码查询个人信息
     * @param userName 用户名
     * @param password 密码信息
     * @return
     */
    @Select("select * from user where userName=#{userName} and password=#{password}")
    //注意：数据层操作不要和业务层操作的名称混淆，通常数据层仅反映与数据库间的信息交换，不体现业务逻辑
    public User getByUserNameAndPassword(@Param("userName") String userName,@Param("password") String password);
}
```

可以看到我们上面就是使用对应的Select注解来帮我们完成我们的转换的

然后我们接着来干掉我们的Spring的核心配置文件applicationContext.xml文件，我们首先创建config包，然后我们将其核心配置文件一共分为三部分，一个是Jdbc的，一个是Mybatis的，还有一个是SpringConfig的，我们令其分开用注解配置对应的环境，最后在全部引入到我们的核心配置文件中

首先我们创建JdbcConfig类，并写入我们的代码如下，这里的代码之前我们就写过了，本质还是加载对应的jdbc文件并创建连接池的代码，这里就不赘述了

```
package com.itheima.config;

import com.alibaba.druid.pool.DruidDataSource;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;

import javax.sql.DataSource;

public class JdbcConfig {
    //使用注入的形式，读取properties文件中的属性值，等同于<property name="*******" value="${jdbc.driver}"/>
    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String userName;
    @Value("${jdbc.password}")
    private String password;

    //定义dataSource的bean，等同于<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
    @Bean("dataSource")
    public DataSource getDataSource(){
        //创建对象
        DruidDataSource ds = new DruidDataSource();
        //手工调用set方法，等同于set属性注入<property name="driverClassName" value="******"/>
        ds.setDriverClassName(driver);
        ds.setUrl(url);
        ds.setUsername(userName);
        ds.setPassword(password);
        return ds;
    }
}
```

再来看看我们的jdbc文件里写入的代码

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://175.178.114.158:3306/ssm_db
jdbc.username=root
jdbc.password=itheima
```

我们这里为什么jdbc的代码是不用去干掉的呢？这是因为jdbc是用于连接数据库的，如果我们把这个干掉了那以后用户需要修改连接的时候都得自己去代码里面修改连接，这个显然是不行的，所以我们的jdbc的代码仍然是保留下来的

然后我们来做Mybatis的配置，我们这里将对应的创建分页插件的代码给整合到一个方法里了

```
package com.itheima.config;

import com.github.pagehelper.PageInterceptor;
import org.apache.ibatis.plugin.Interceptor;
import org.mybatis.spring.SqlSessionFactoryBean;
import org.mybatis.spring.mapper.MapperScannerConfigurer;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;

import javax.sql.DataSource;
import java.util.Properties;

public class MyBatisConfig {
    //定义MyBatis的核心连接工厂bean，等同于<bean class="org.mybatis.spring.SqlSessionFactoryBean">
    @Bean
    //参数使用自动装配的形式加载dataSource，为set注入提供数据，dataSource来源于JdbcConfig中的配置
    public SqlSessionFactoryBean getSqlSessionFactoryBean(@Autowired DataSource dataSource,@Autowired Interceptor interceptor){
        SqlSessionFactoryBean ssfb = new SqlSessionFactoryBean();
        //等同于<property name="typeAliasesPackage" value="com.itheima.domain"/>
        ssfb.setTypeAliasesPackage("com.itheima.domain");
        //等同于<property name="dataSource" ref="dataSource"/>
        ssfb.setDataSource(dataSource);
//        //等同于<bean class="com.github.pagehelper.PageInterceptor">
//        Interceptor interceptor = new PageInterceptor();
//        Properties properties = new Properties();
//        properties.setProperty("helperDialect","mysql");
//        properties.setProperty("reasonable","true");
//        //等同于<property name="properties">
//        interceptor.setProperties(properties);
        ssfb.setPlugins(interceptor);
        return ssfb;
    }

    //定义MyBatis的映射扫描，等同于<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    @Bean
    public MapperScannerConfigurer getMapperScannerConfigurer(){
        MapperScannerConfigurer msc = new MapperScannerConfigurer();
        //等同于<property name="basePackage" value="com.itheima.dao"/>
        msc.setBasePackage("com.itheima.dao");
        return msc;
    }

    @Bean("pageInterceptor")
    public Interceptor getPageInterceptor(){
        Interceptor interceptor = new PageInterceptor();
        Properties properties = new Properties();
        properties.setProperty("helperDialect","mysql");
        properties.setProperty("reasonable","true");
        //等同于<property name="properties">
        interceptor.setProperties(properties);
        return interceptor;
    }

}
```

上面还定义了对应的映射扫描，而至于分页插件里面的方法设置则是按照通过配置文件结合其源码分析搞出来的，当然实际上我们不将其设置为一个方法也可以，我们这里这样搞只不过是为了令其便于我们日后的维护而已

接着我们来做我们的核心配置文件，我们这里很多东西都是一群的老东西了，这些东西就不再赘述了，我们这里重点要讲解的有两个，一个是EnableTransactionManagement注解，另外一个是我们这里事务管理器

```
package com.itheima.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.*;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;
import org.springframework.stereotype.Controller;
import org.springframework.transaction.annotation.EnableTransactionManagement;

import javax.sql.DataSource;

@Configuration
//等同于<context:component-scan base-package="com.itheima">
@ComponentScan(value = "com.itheima",excludeFilters =
        //等同于<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
@ComponentScan.Filter(type= FilterType.ANNOTATION,classes = {Controller.class}))
//等同于<context:property-placeholder location="classpath*:jdbc.properties"/>
@PropertySource("classpath:jdbc.properties")
//等同于<tx:annotation-driven />，bean的名称默认取transactionManager
@EnableTransactionManagement
@Import({MyBatisConfig.class,JdbcConfig.class})
public class SpringConfig {
    //等同于<bean id="txManager"/>
    @Bean("transactionManager")
    //等同于<bean class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    public DataSourceTransactionManager getTxManager(@Autowired DataSource dataSource){
        DataSourceTransactionManager tm = new DataSourceTransactionManager();
        //等同于<property name="dataSource" ref="dataSource"/>
        tm.setDataSource(dataSource);
        return tm;
    }
}
```

首先我们这里的事务管理器的Bean之所以放到我们的Spring的核心配置文件中，是因为我们这里的事务管理是Spring提供的，所以我们将其放置到我们的Spring的核心注解配置文件中，当然实际上你要放到jdbc的配置文件中去也可以，无非是规范与否的区别罢了，实际上都能用

这里我们要重点提一下的是我们的开启注解式事务的代码，我们之前是使用tx:annotation-driven标签来开启的注解并写入了id，这个id就是我们自己创建的事务管理器类，通过id我们实现了让我们的事务注解与我们的具体事务管理类产生了联系，然后他们才能正常工作，但是在我们这里我们的开启注解的注解是EnableTransactionManagement，这里面又没有设置名字的属性，那我们应该怎么办呢？实际上，如果我们没有设置名字，那么其名字就默认为transactionManager，那么我们只要令我们的事务管理器类加入到容器中时给其取名为transactionManager就可以了，这里也是说明了我们使用注解类来搞定我们的事务的话，我们的事务管理类的名字一定要是transactionManager

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE9ff5b89567d247a2a99f127ed1291ccb.png)

接着我们来做spring-mvc的注解形式开发，我们首先创建对应的注解类，然后写入其代码如下

```
package com.itheima.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;

@Configuration
//等同于<context:component-scan base-package="com.itheima.controller"/>
@ComponentScan("com.itheima.controller")
//等同于<mvc:annotation-driven/>，还不完全相同
@EnableWebMvc
public class SpringMvcConfig {
}
```

我们这里开启注解驱动的注解是EnableWebMvc，这个注解驱动的作用等同于我们的在配置文件里的注解驱动的代码，但是又不完全相同，实际上，注解EnableWebMvc的功能要强大得多，由于我们现在学习到的知识还是有限的，因此我们这里无法对内部的强大功能进行一个逐一的讲解，但是有一点我们可以先说，那就是这个注解同时还负责了将我们的数据转换为我们的统一数据格式的功能

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe5644171ab5129ec2cd74013e843e806.png)

最后我们还剩下一个文件要干掉，就是web.xml，我们这里做的就是普通的字符集转换以及设置监听器和过滤器，现在我们要用注解形式来代替这个，我们以前做过这个，只需要将原来的文件复制过来并稍作调试就可以了

我们这里要新添加的东西就是我们的设定监听器的代码，我们当初设置监听器代码的主要作用是令其优先加载我们的spring的配置文件

```
package com.itheima.config;

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

    //创建Servlet容器时，使用注解的方式加载SPRINGMVC配置类中的信息，并加载成WEB专用的ApplicationContext对象
    //该对象放入了ServletContext范围，后期在整个WEB容器中可以随时获取调用
    @Override
    protected WebApplicationContext createServletApplicationContext() {
        AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
        ctx.register(SpringMvcConfig.class);
        return ctx;
    }

    //注解配置映射地址方式，服务于SpringMVC的核心控制器DispatcherServlet
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

    @Override
    //基本等同于<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    protected WebApplicationContext createRootApplicationContext() {
        AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
        ctx.register(SpringConfig.class);
        return ctx;
    }

    //乱码处理作为过滤器，在servlet容器启动时进行配置，相关内容参看Servlet零配置相关课程
    @Override
    public void onStartup(ServletContext servletContext) throws ServletException {
        //触发父类的onStartup
        super.onStartup(servletContext);
        //1.创建字符集过滤器对象
        CharacterEncodingFilter cef = new CharacterEncodingFilter();
        //2.设置使用的字符集
        cef.setEncoding("UTF-8");
        //3.添加到容器（它不是ioc容器，而是ServletContainer）
        FilterRegistration.Dynamic registration = servletContext.addFilter("characterEncodingFilter", cef);
        //4.添加映射
        registration.addMappingForUrlPatterns(EnumSet.of(DispatcherType.REQUEST, DispatcherType.FORWARD, DispatcherType.INCLUDE), false, "/*");
    }
}
```

但是在上面的类中，存在两个方法，分别是createRootApplicationContext和createServletApplicationContext，前者是用于加载Spring的，而后者是用于加载SpringMVC的，因此如果我们希望加载Spring的核心配置文件，我们只要使用和前者一样的写法就可以了，我们之前加载spring核心配置文件的方法是令其返回null的，因为那时我们还不需要在这里加载Spring的核心配置文件

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8989ee796c15f021678e97465e20e321.png)

