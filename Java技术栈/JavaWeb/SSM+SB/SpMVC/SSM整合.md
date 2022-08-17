接着我们到了我们SpringMVC的最后一个内容的学习，就是SSM整合，本章最长，但也是最后一章节了，加油吧

# SSM整合

## 校验框架入门

本节内容我们来学习校验框架的相关内容，首先我们先来介绍下我们的校验。一般来说，对于用户的输入进行一个简单的前台校验就可以了，这样就可以防止用户输入不合法的内容了，但是可能存在技术人员绕过这一输入直接往通过地址或者是其他方式绕过前台的检测往服务中发送不合法的数据，因此我们的服务器也需要对应的校验技术，也就是前台的校验和服务器的校验共同构成了表单校验

一般来说校验分格式校验和逻辑校验，逻辑校验，这两个校验都分为客户端上的校验与服务器端的校验，逻辑校验里是因为我们是使用json传输我们的数据，那么有可能这个json数据本身 并不合法或者已经有重复了，这时就需要对应的逻辑校验，不过逻辑校验并不是我们本章的重点，我们先按下不表。我们这里重点要讲的还是格式校验，我们前台对格式校验的方法是使用JS技术利用正则表达式进行校验，而在后台，我们校验格式则要使用对应的校验框架

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE49276bf0ff788328f6c76507a2f9d2f9.png)

我们校验的内容一般都长度、非法字符、数据格式、边界值以及重复性

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEaf8f06539c5db583a99ffba2aeb86f29.png)

我们的表单校验框架是JSR，其是一套Java的规范提案，类似于接口，但是接口对我们而言没什么用，我们又不可能自己去实现这一套接口，所以我们需要使用实现类。正好这里有一套免费开源的校验框架的实现类，hibernate-validator，我们就引入这个依赖来完成我们的案例功能

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe2d3f7729daa4d1b162bf1799edb901e.png)

具体实现案例之前，我们需要先来介绍下我们的环境。首先我们这里并没有使用到jQuery类库，我们的web.xml里做的还是老两样的配置，设定对应的字符集以及配置我们的全局过滤器类。springmvc.xml中则是扫描控制类以及开启注解驱动。

然后我们创建一个page.jsp的页面并写入代码如下

```
<%--
  Created by IntelliJ IDEA.
  User: 22592
  Date: 2022/5/11
  Time: 20:41
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>添加员工-用于演示表单验证</title>
</head>
<body>
    <form action="/addemployee" method="post">
        员工姓名:<input type="text" name="name"/> <span style="color: red">${name}</span> <br/>
        员工年龄:<input type="text" name="age"/><br/>
        <input type="submit" value="提交"/>
    </form>
</body>
</html>

```

其实现的就是很简单的提交功能，同时还给予了遇到异常时会提示的信息，没遇到时对应的EL表达式的值为空，则不会显示任何东西，我们要执行的方法是addemployee，我们先创建对应的员工类然后我们在我们的控制类中写入对应的代码如下

```
package com.itheima.controller;

import com.itheima.domain.Employee;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.Errors;
import org.springframework.validation.FieldError;
import org.springframework.web.bind.annotation.RequestMapping;
import javax.validation.Valid;
import java.util.List;

@Controller
public class EmployeeController {

    @RequestMapping(value = "/addemployee")
    public String adEmployee(@Valid Employee employee, Errors errors, Model m){
        if(errors.hasErrors()){
            List<FieldError> fieldErrors = errors.getFieldErrors();
            for (FieldError error:fieldErrors) {
                m.addAttribute(error.getField(),error.getDefaultMessage());
            }
            return "addemployee.jsp";
        }
        return "success.jsp";
    }
}

```

这里我们对上面的代码进行逐一的讲解，首先我们的Valid注解是用于设定对当前实体类的类型参数进行校验的注解，有了这个注解之后，被修饰的形参就会进行对应的校验，而Errors对象则保存了校验得到的结果，其有hasErrors方法，该方法返回一个布尔值，若为真则说明校验出了不合法的异常，若为假则说明没有异常，那么我们就可以构造这么一份代码，分别处理有异常和无异常的情况。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEad9bde189eba06c6cc3bc3dc9ead9d8b.png)

我们上面的代码就是这么做的，有异常我们就获得所有的异常对象，然后进行迭代，接着将其全部写入到我们的模块中，写入的内容分别是异常出现的具体属性名，以及异常的描述信息，接着我们返回我们的原来的注册页面，若出现异常，则该页面的EL表达式则会接受到对应的值接着将其打印出来

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf7306998662f1064bf19612f49d1ba98.png)

不过即使我们指定了要校验的对象，具体的校验规则还是需要我们去指定的，这里我们就需要用到NotBlank注解并加在我们的创建类的对应的属性上，这样被修饰的属性就会进行对应的校验

```
@NotBlank(message = "姓名不能为空")
private String name;
```

比如我们上面的代码就设定了我们的name属性不能为空，若为空我们就返回对应的报错字符串，括号内的message属性内容则是我们的要设置的报错信息的内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd8f56d9fbe15793f2e2c4e8def2587c4.png)

最后我们也能够解释为什么我们要在页面上设置接受的值为我们的属性的名字了，这样才能保证我们能够正确接收到对应的属性出现的异常的信息并打印在客户端上

## 多规则校验，嵌套校验与分组校验

接着我们来讲我们的校验的进阶部分，分别是多规则校验和嵌套校验与分组校验

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf2fbcfff6025b64c41e088cd93c4e721.png)

首先我们来讲多规则校验，如果我们希望我们的某个属性要经过多个规则的校验，我们就只需要往其属性上继续添加对应的注解就可以了，比如说像下面这样

```
@NotNull(message = "该项不能为空",groups = {GroupA.class})
@Max(value = 60,message = "60岁以上的不予通行")
@Min(value = 18,message = "18岁以下的不许进来")
private int age;
```

上面这样就相当于我们规定我们的传入内容不能为空且不能小于18不能大于60，这就是一个多规则校验的简单例子

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE99dead92cfb9ed9b04992acbac8aff27.png)

上图中还有的内容是关于不同的注解的作用域的分别的，就需要的自己看吧

然后我们来讲讲嵌套校验，最简单的例子就是我们在一个对象里引用了另外一个对象，此时我们要进行校验就需要往对应的属性上加入Valid注解，然后在对应的实现类里添加对应的校验规则

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7483a624f2cf2f3661316413d4041611.png)

按照上面的思路，我们可以写入我们的引用属性的代码如下

```
@Valid
private Address address;
```

然后在该属性的实现类上写入其代码如下

```
package com.itheima.domain;



import javax.validation.constraints.NotBlank;
import javax.validation.constraints.Size;
//嵌套校验的实体中，对每个属性正常添加校验规则即可
public class Address {
    @NotBlank(message = "请输入省份名称")
    private String provinceName;//省份名称

    @NotBlank(message = "请输入城市名称")
    private String cityName;//城市名称

    @NotBlank(message = "请输入详细地址")
    private String detail;//详细住址

    @NotBlank(message = "请输入邮政编码")
    @Size(max = 6,min = 6,message = "邮政编码由6位组成")
    private String zipCode;//邮政编码

    public String getProvinceName() {
        return provinceName;
    }

    public void setProvinceName(String provinceName) {
        this.provinceName = provinceName;
    }

    public String getCityName() {
        return cityName;
    }

    public void setCityName(String cityName) {
        this.cityName = cityName;
    }

    public String getDetail() {
        return detail;
    }

    public void setDetail(String detail) {
        this.detail = detail;
    }

    public String getZipCode() {
        return zipCode;
    }

    public void setZipCode(String zipCode) {
        this.zipCode = zipCode;
    }

    @Override
    public String toString() {
        return "Address{" +
                "provinceName='" + provinceName + '\'' +
                ", cityName='" + cityName + '\'' +
                ", detail='" + detail + '\'' +
                ", zipCode='" + zipCode + '\'' +
                '}';
    }
}

```

可以看到上面我们给对应的属性加上了校验的规则的注解，然后我们需要在界面上将其对应的异常信息打印出来（如果发生了异常的话），那么我们可以将我们的提交页面的代码修改如下

```
<%--
  Created by IntelliJ IDEA.
  User: 22592
  Date: 2022/5/11
  Time: 20:41
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>添加员工-用于演示表单验证</title>
</head>
<body>
    <form action="/addemployee" method="post">
        员工姓名:<input type="text" name="name"/> <span style="color: red">${name}</span> <br/>
        员工年龄:<input type="text" name="age"/><span style="color: red">${age}</span> <br/>
        省:<input type="text" name="address.provinceName"/><span style="color: red">${requestScope['address.provinceName']}</span> <br/>
        <input type="submit" value="提交"/>
    </form>
</body>
</html>

```

我们这里提交的数据的名字是address.provinceName，那为什么我们EL表达式里的代码却是requestScope['address.provinceName']呢？因为如果我们直接写address.provinceName的话，我们的代码会认为我们是要接受address对象里的provinceName属性，而实际上，我们在异常处理中传入的只是一个属性名和对应的字符串而已，并没有传入什么对象进来，所以我们这里需要令其将整个代码识别为一个字符串而非对象中的属性，所以我们这里需要使用requestScope来帮助我们完成这件事

最后我们来讲解我们的分组校验，我们的分组校验存在的意义就是，有些时候可能根据我们执行的业务不同，我们需要校验的属性也不一样，有些业务不需要对某些属性进行校验而某些业务又需要，此时我们就需要分组校验对其进行对应的设置

要进行分组校验我们首先要创建一个用于分组的接口，然后我们这里要使用Validated注解，往其中传入对应的接口的class对象，然后只有同样传入对应的字节码对象的属性在执行这个方法时才会发生对应的校验，否则就不会

```
@RequestMapping(value = "/addemployee")
public String adEmployee2 (@Validated(value = {GroupA.class}) Employee employee, Errors errors, Model m){
    if(errors.hasErrors()){
        List<FieldError> fieldErrors = errors.getFieldErrors();
        for (FieldError error:fieldErrors) {
            m.addAttribute(error.getField(),error.getDefaultMessage());
        }
        return "addemployee.jsp";
    }
    return "success.jsp";
}
```

那么我们可以在我们的实现类中写入我们的代码如下，可以看到我们这里NotBlank和NotNull上加了对应的接口，这样我们执行上面的方法进行访问时，就只有这两个注解会发挥校验作用，其他的不会

```
@NotBlank(message = "姓名不能为空",groups = {GroupA.class})
private String name;

@NotNull(message = "该项不能为空",groups = {GroupA.class})
@Max(value = 60,message = "60岁以上的不予通行")
@Min(value = 18,message = "18岁以下的不许进来")
private int age;

@Valid
private Address address;

```

一般来说，我们都是在控制层中采用Validated注解，因为该注解囊括了Valid注解的作用，我们在一个实现类的引用了另一个实现类的引用属性中，则用Valid注解来令其内部的实现类也参与校验。我们按照这样的规范，我们就不容易搞错这两个注解的作用

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1ef44f0e2d3f2fb1ad798a910d70e334.png)

## SSM整合流程简介

 现在我们来学习SSM的整合，所谓SSM整合，就使用Spring、SpringMVC、MyBatis进行整合最后制作而来的项目，具体的步骤请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9790678b5752f5065c6775a551bac5c3.png)

 其中第一部分属于复习，第三四五部分则是我们重点要学习的东西，然后我们这里要使用SSM的整合案例的数据，分别是用户表和学生表，这个我们先了解下就好了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5d019128b3fbd986bc34d969fac6016e.png)

## 项目结构搭建

接着我们就来讲解我们的第0的部分的内容，我们这里真的是完全从零开始搭建，所以我们这里有第0节，也就是先搭建项目的结构，我们这一节认真学好项目搭建的思路，以后我们的项目都可以按照这个思路来搭建，先来看看我们的步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEdf04208f71cb37d319cc4d0e27c89f1b.png)

首先我们创建项目，我们新建一个模块并取名，我们这里创建maven工程，然后我们使用的模板是webapp，我们创建完这个项目之后我们往内部创建对应的java和resources文件夹，然后我们按照三层架构来构建项目，首先我们创建统一的包com.itheima，然后我们创建三层架构的包，首先是数据层dao包，然后是业务层service包，最后是控制层controller包。这里我们还额外创建一个包domian来保存我们的实体类

然后我们进入我们的数据库，创建我们的对应的要使用的数据库和User表，其代码如下

```
CREATE DATABASE ssm_db;

CREATE TABLE `user` (
   `uuid` INT(10) NOT NULL AUTO_INCREMENT,
   `userName` VARCHAR(100) DEFAULT NULL,
   `password` VARCHAR(100) DEFAULT NULL,
   `realName` VARCHAR(100) DEFAULT NULL,
   `gender` INT(1) DEFAULT NULL,
   `birthday` DATE DEFAULT NULL,
   PRIMARY KEY (`uuid`)
)ENGINE=INNODB AUTO_INCREMENT=15 DEFAULT CHARSET=utf8
```

然后我们就可以创建处一个我们所需要的User的用户表，接着我们去创建我们的完成项目所需要的对应的User实体类，其代码如下

```
package com.itheima.domain;

import java.io.Serializable;
import java.util.Date;

public class User implements Serializable {

    private Integer uuid;
    private String userName;
    private String password;
    private String realName;
    private Integer gender;
    private Date birthday;

    @Override
    public String toString() {
        return "User{" +
                "uuid=" + uuid +
                ", userName='" + userName + '\'' +
                ", password='" + password + '\'' +
                ", realName='" + realName + '\'' +
                ", gender=" + gender +
                ", birthday=" + birthday +
                '}';
    }

    public Integer getUuid() {
        return uuid;
    }

    public void setUuid(Integer uuid) {
        this.uuid = uuid;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getRealName() {
        return realName;
    }

    public void setRealName(String realName) {
        this.realName = realName;
    }

    public Integer getGender() {
        return gender;
    }

    public void setGender(Integer gender) {
        this.gender = gender;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public User() {
    }

    public User(Integer uuid, String userName, String password, String realName, Integer gender, Date birthday) {
        this.uuid = uuid;
        this.userName = userName;
        this.password = password;
        this.realName = realName;
        this.gender = gender;
        this.birthday = birthday;
    }
}

```

这里的过程也无非就是设置对应的私有属性然后提供方法罢了，我们这里值得一提的是，为了使我们的创建对应的属性的顺序不出错，我们可以将我们最初的sql代码放到我们的实体类上，一个个属性对照着来创建，这样可以有效防止出错

接着我们在Dao层创建UserDao接口，由于我们直到Dao层中的代码是依靠我们的框架进行动态实现的，因此Dao层中不需要实现类，只需要对应的接口方法

```
package com.itheima.dao;

import com.itheima.domain.User;

import java.util.List;

public interface UserDao {
    /**
     * 添加用户
     * @param user
     * @return
     */
    public boolean save(User user);

    /**
     * 修改用户
     * @param user
     * @return
     */
    public boolean update(User user);

    /**
     * 删除用户
     * @param uuid
     * @return
     */
    public boolean delete(Integer uuid);

    /**
     * 查询单个用户信息
     * @param uuid
     * @return
     */
    public User get(Integer uuid);

    /**
     * 查询全部用户信息
     * @return
     */
    public List<User> getAll();

    /**
     * 根据用户名密码查询用户信息
     * @param userName
     * @param password
     * @return
     */
    public User getByUserNameAndPassword(String userName,String password);
}

```

这里我们创建对应的方法，有两点需要注意，第一点是一般我们在Dao层接口中，我们的方法名不直接写我们的业务方法名，而是写我们的与数据打交道的名字，比如我们的第五个方法是根据用户名和密码进行登录，但我们不直接写登录，而是写根据用户名和密码获得某某东西，同时我们创建完对应的方法之后不要忘了给对应的方法加上对应的方法注释，便于后面的程序员的阅读。我们的这里要执行的方法分别是增删改查，无非是查的方法多了几个而已，不同的查询方法其返回值也不同，一般多个的要返回集合，单个的返回一个对象，而增加删除为了表示成功与否则用布尔类型变量作为返回值

接着我们创建对应的UserService层的接口，写入其接口代码如下

```
package com.itheima.service;

import com.itheima.domain.User;

import java.util.List;

public interface UserService {
    /**
     * 添加用户
     * @param user
     * @return
     */
    public boolean save(User user);

    /**
     * 修改用户
     * @param user
     * @return
     */
    public boolean update(User user);

    /**
     * 删除用户
     * @param uuid
     * @return
     */
    public boolean delete(Integer uuid);

    /**
     * 查询单个用户信息
     * @param uuid
     * @return
     */
    public User get(Integer uuid);

    /**
     * 查询全部用户信息
     * @return
     */
    public List<User> getAll();

    /**
     * 根据用户名密码进行登录
     * @param userName
     * @param password
     * @return
     */
    public User login(String userName,String password);
}

```

可以看到上面上面接口的内容跟之前的一模一样，不过方法名做了改动，我们这里的方法名是真正的我们的业务名字，而不是用通过数据具体做什么互动来确定其名字的，这也是唯一的不同了，然后创建其对应的实现类，并且在控制层上创建对应的控制层类，然后我们的大体框架就实现完毕了，具体结构图如下所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEba16ae95419f8911bbb4d4fd7be11a02.png)

## Spring整合Mybatis

然后本节我们来学习SPring整合Mybtis，我们先来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb0fb90527ea2bc4a41c071a2c935db72.png)

首先我们要引入对应的依赖，具体我们引入依赖的配置代码如下

```
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">

  <modelVersion>4.0.0</modelVersion>
  <packaging>war</packaging>

  <name>springmvc_ssm</name>
  <groupId>com.itheima</groupId>
  <artifactId>springmvc_ssm</artifactId>
  <version>1.0-SNAPSHOT</version>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>

  <dependencies>
    <!--mybatis环境-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.3</version>
    </dependency>
    <!--mysql环境-->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.47</version>
    </dependency>
    <!--spring整合jdbc-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.1.9.RELEASE</version>
    </dependency>
    <!--spring整合mybatis-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>2.0.3</version>
    </dependency>
    <!--druid连接池-->
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.16</version>
    </dependency>
    <!--分页插件坐标-->
    <dependency>
      <groupId>com.github.pagehelper</groupId>
      <artifactId>pagehelper</artifactId>
      <version>5.1.2</version>
    </dependency>

    <!--springmvc环境-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.1.9.RELEASE</version>
    </dependency>
    <!--jackson相关坐标3个-->
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.9.0</version>
    </dependency>

    <!--servlet环境-->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
    </dependency>

    <!--其他组件-->
    <!--junit单元测试-->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
    <!--spring整合junit-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>5.1.9.RELEASE</version>
    </dependency>
  </dependencies>


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

我们上面引入的依赖都是我们的项目所需要的依赖，这里有一些依赖引入时会引入另外一些依赖，那些被附带引入的依赖我们就不需要再额外引入一次了，因为已经引入过了，没必要这样多此一举

然后我们要创建Spring的核心配置文件和Mybatis的映射配置文件，我们先来创建spring的核心配置文件applicationContext.xml，然后在其中开启扫描组件com.itheima，我们就令其扫描这些包，这里我们有一点要提一下，就是我们之前说过，spring在扫描的时候是不需要加载SpringMVC里要加载的类的，所以我们这里应该要加一个排除功能或者是过滤功能，这个等到我们后面再来做

```
<context:component-scan base-package="com.itheima"/>
```

然后我们来说Mybatis映射配置文件，我们首先要在对应的resources资源包下创建对应的com.itheima.dao包，注意我们这里创建时不能用.的方式，要用/来代替.，否则最后生成的其实是一个目录而不是嵌套的目录，然后我们在dao层中创建对应的xml文件并写入其搜索代码如下

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.itheima.dao.UserDao">
<!--上面是开启对应的命名空间，指定我们想要进行动态代理的接口-->

    <!--添加-->
    <insert id="save" parameterType="user">
        insert into user(userName,password,realName,gender,birthday)values(#{userName},#{password},#{realName},#{gender},#{birthday})
    </insert>

    <!--删除-->
    <delete id="delete" parameterType="int">
        delete from user where uuid = #{uuid}
    </delete>

    <!--修改-->
    <update id="update" parameterType="user">
        update user set userName=#{userName},password=#{password},realName=#{realName},gender=#{gender},birthday=#{birthday} where uuid=#{uuid}
    </update>

    <!--查询单个-->
    <select id="get" resultType="user" parameterType="int">
        select * from user where uuid= #{uuid}
    </select>

    <!--分页查询-->
    <select id="getAll" resultType="user">
        select * from user
    </select>

    <!--登录-->
    <select id="getByUserNameAndPassword" resultType="user" >
        select * from user where userName=#{userName} and password=#{password}
    </select>

</mapper>
```

最后我们还需要在对应的第五个动态代理的实现类的接口的对应形参里加上Param注解，不过这里为什么要加我没搞懂，百度说这个注解是为了给SQL语句中的参数赋值而服务的，但我不明白的是为什么又只给这一个加而不给其他的加，既然要赋值那不应该是其他要获得值都应该加一个吗？还有就是里面的具体过程又是如何，暂时先记着有这个疑问吧

```
/**
 * 根据用户名密码查询用户信息
 * @param userName
 * @param password
 * @return
 */
public User getByUserNameAndPassword(@Param("userName") String userName,@Param("password") String password);
```

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEdb49311f77214db017e8e13f989f884f.png)

然后我们要令我们的业务层去调用我们的数据层，那么我们进入我们业务层的实现类，首先令业务层被Spring容器管理，因为其是业务层，所以不使用Component注解而使用Service注解，然后其下赋予属性UserDao，使用Autowired注解令其自动获得对应的动态实现类，接着在所有对应的业务层方法中调用数据层的方法就可以了

```
package com.itheima.service.impl;

import com.github.pagehelper.PageHelper;
import com.github.pagehelper.PageInfo;
import com.itheima.dao.UserDao;
import com.itheima.domain.User;
import com.itheima.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class UserServiceImpl implements UserService {

    @Autowired
    private UserDao userDao;

    public boolean save(User user) {
        return userDao.save(user);
    }

    public boolean update(User user) {
        return userDao.update(user);
    }

    public boolean delete(Integer uuid) {
        return userDao.delete(uuid);
    }

    public User get(Integer uuid) {
        return userDao.get(uuid);
    }

    public PageInfo<User> getAll(int page,int size) {
        PageHelper.startPage(page,size);
        List<User> all = userDao.getAll();
        return new PageInfo<User>(all);
    }

    public User login(String userName, String password) {
        return userDao.getByUserNameAndPassword(userName,password);
    }
}

```

这里我们可以看到我们的获得所有的方法已经发生了变动了，这是因为我们这里查询我们的信息利用的是我们的分页插件进行查询的，因此我们就不能令其返回一个简单的集合对象，而是要返回一个分页插件会返回的集合对象，然后分页插件需要两个参数，这里我们通过形参的方式给出，然后在方法里利用分页对象的方法进行对应的处理，然后调用数据层的方法，接着返回一个对应的分页参数的对象。

既然我们的实现类改了方法，那么我们的接口也需要做相应的变动，那么接口的方法就需要修改如下

```
/**
 * 查询全部用户信息
 * @return
 */
public PageInfo<User> getAll(int page,int size);
```

最后我们就来配置Spring的核心配置文件，首先我们要注入我们的数据源工厂对象，其下需要引入一个数据源，因此我们要先创建对应的数据源，因此我们先创建对应的数据源对象，数据源对象里需要给予必要的jdbc的连接地址和配置，所以我们先创建对应的jdbc文件如下

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://175.178.114.158:3306/ssm_db
jdbc.username=root
jdbc.password=itheima
```

然后我们在配置文件中要开启对这个jdbc文件的加载，最后我们就可以按照我们之前的步骤返回来一步步创建了

```
  <!--映射扫描-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.itheima.dao"/>
    </bean>
```

最后别忘了我们还需要Mybatis的映射扫描配置，因此我们这里还要写入Mybatis的映射扫描代码，指定其要扫描的包

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE50d50c55ef9976a4238740f4fa31bc82.png)

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE209fe9138f4310715041e30fef6c9046.png)

## 配置分页插件与事务

然后本节我们在核心配置文件中来配置我们的分页插件并开启我们的事务，首先我们开启我们的分页插件，我们需要进入到对应的数据源对象中查看其设置插件的方法，我们根据这个方法来配置我们的分页插件

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE56d3b462d16fde15b5f6dd1d36dc267c.png)

首先我们可以看到形参的名字是plugins，因此我们的配置时的名字就必须设置为plugins了，否则不会正确赋予到对应的值上，同时我们可以看到其需要的参数是传入一个过滤器数组，这就意为着我们传入时必然要利用数组传入，然后我们往里面传入时，我们传入的是一个过滤器对象，直接用bean标签传入，过滤器里又需要设定值，我们利用props标签进行对集合的对应赋值，内部有很多值可以设定，我们这里只设定两个作为测试

```
<property name="plugins">
    <array>
        <bean class="com.github.pagehelper.PageInterceptor">
            <property name="properties">
                <props>
                    <prop key="helperDialect">mysql</prop>
                    <prop key="reasonable">true</prop>
                </props>
            </property>
        </bean>
    </array>
</property>
```

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE455c2bfbd73304584a096b55105efc5b.png)

然后我们还需要开启我们的事务配置，我们开启事务配置的方法很简单，首先要引入事务配置的头格式，然后我们注入事务管理的对应类，往其内部引入我们的数据源对象，接着在代码的最上方开启事务就行了

```
<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>
```

```
<tx:annotation-driven transaction-manager="txManager"/>
```

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE02a999e876e156cae20b7cb7f9431b71.png)

那么我们最后就可以将我们的核心配置文件的代码写成如下的样子

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

    <context:component-scan base-package="com.itheima"/>

    <tx:annotation-driven transaction-manager="txManager"/>

    <!--加载properties文件-->
    <context:property-placeholder location="classpath*:jdbc.properties"/>

    <!--数据源-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

    <!--整合mybatis到spring中-->
    <bean class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <property name="typeAliasesPackage" value="com.itheima.domain"/>
        <!--分页插件-->
        <property name="plugins">
            <array>
                <bean class="com.github.pagehelper.PageInterceptor">
                    <property name="properties">
                        <props>
                            <prop key="helperDialect">mysql</prop>
                            <prop key="reasonable">true</prop>
                        </props>
                    </property>
                </bean>
            </array>
        </property>
    </bean>

    <!--映射扫描-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.itheima.dao"/>
    </bean>

    <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

</beans>


```

最后我们还需要在我们对应的业务层中使用注解来令我们的对应的操作能够使用事务并开启事务权限，我们这里一般是写在接口中而不是实现类中，就是为了提高我们的方法的普适性，然后一般我们是查询方法比较多而增删方法比较少，所以我们一般在类中设定对应的注解将所有事务设定为只读事务，然后在具体的方法下设定对应的事务为非只读事务

```
package com.itheima.service;

import com.github.pagehelper.PageInfo;
import com.itheima.domain.User;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

@Transactional(readOnly = true)
public interface UserService {

    /**
     * 添加用户
     * @param user
     * @return
     */
    @Transactional(readOnly = false)
    public boolean save(User user);

    /**
     * 修改用户
     * @param user
     * @return
     */
    @Transactional(readOnly = false)
    public boolean update(User user);

    /**
     * 删除用户
     * @param uuid
     * @return
     */
    @Transactional(readOnly = false)
    public boolean delete(Integer uuid);

    /**
     * 查询单个用户信息
     * @param uuid
     * @return
     */
    public User get(Integer uuid);

    /**
     * 查询全部用户信息
     * @return
     */
    public PageInfo<User> getAll(int page,int size);

    /**
     * 根据用户名密码进行登录
     * @param userName
     * @param password
     * @return
     */
    public User login(String userName,String password);
}

```

## 整合junit测试业务层接口

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5ea2924cdc1c34101d66a4dbce4c3f98.png)

最后我要提一下，我们现在的测试其实仍然是不规范的，因为我们是自己连接我们自己的数据库，企业开发中我们实际上是在我们的test包中也搞一个我们的配置文件，然后我们在test包中去用新的连接去进行测试的，而原本的项目中的配置文件是另外一个属于他自己的连接，这个我们一般是不做什么改动的。这些知识我们先了解下就可以了

## Rest风格开发SpringMVC

本节我们来用Rest风格来开发SpringMVC，首先我们来看看我们的步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf4e7a5a20222282813d10e5c913dba3a.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEad267d59ba883dc4cb44df5c20cbb260.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE939b7e48be26640b4d4bd27450aa5759.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE85f64c5023bd7bd40edf3433a0eab881.png)

最后我们用我们的postman工具经过测试我们会发现我们的方法都的确是可以正确运行的，此时就说明我们本节的案例已经完成了

## Spring整合SpringMVC

接着我们来学习如何用spring整合SpringMVC

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf17438a540d188b5a72ae0328075aaf7.png)

要用Spring整合SpringMVC，我们就需要在web.xml加载时将Spring环境也加载了，因此我们要对web.xml加入一些代码来令其完成这个工作

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5e4f8f7ed6eaf34f733d90b086672dec.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6d70b962e29f6b242f518c5ea919e54b.png)

最后我们发送Rest风格的访问到我们的服务器上，我们使用的PostMan软件，这里我们对应的访问的格式如下

首先是新增，http://localhost/user?userName=Jock&password=1234&realName=JockisSuperMan&gender=1&birthday=1999/09/09

然后是登录，http://localhost/user/login?userName=Jockme&password=root

接着是修改，http://localhost/user?userName=Jock123321&password=1234&realName=JockisSuperMan&gender=1&birthday=1999/09/09&uuid=17

然后是删除，http://localhost/user/17

接着是查询单个，http://localhost/user/1

最后是分页查询，http://localhost/user/1/2

最后其对应的查询风格如下所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8af71a6ce5057f8ae92fc03d0c495153.png)

## 表现层数据封装

当我们做到这里的时，或许有的同学就已经觉得我们的案例已经全部实现完毕了，但其实不是的，我们还有一项最重要的步骤要做，就是表现层的数据封装，这个不做，那么就无法与前端进行一个很好的对接，到时候别人就觉得我们是业余的，这样肯定不行。

为什么我们要进行表现层的数据封装？这是因为我们的服务器返回的结果可能是各种各样的，而且随着不同的情况变化，也会产生各种不同的数据到前台，如果不对这些返回的数据进行封装，使其变成一个统一的格式，那么前台人员看到这个就头大，不知道这个数据代表什么意思。有的同学可能觉得true和false就能代表成功和失败就挺好的，但是有可能会出现两个方法都是返回这个来代表成功或者失败的情况，到时候你要前端人员怎么办呢你说是吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc45b0abb369b048d67a16e02aebf33e8.png)

那么我们要如何进行表现层的数据封装呢？我们可以创建一个用于封装数据的类，姑且称之为Result，其中我们的返回数据的设计一般有三部分，分别是状态、数据、消息。将其返回前台，前台的人员就可以看到你的返回状态的数据内容。同时我们的业务不同我们也要设计不同的状态码来表示我们方法执行的情况，这其实就类似于浏览器中返回给我们的404状态码一样，主要是为了给前端人员看的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf6a98a0bd80730e01dbc4511d121bbb7.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEae83f869bc2e8dfb51547c6f377592ea.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE46b783cc4f0b7438b17dd887410cae6a.png)

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

## 问题消息处理

现在我们来完成问题消息的处理，我们这里需要自定义我们的异常，令对应的程序中的异常转换为我们自定义的异常，这一环节我们之前也学习过了，但是这里不同的是，我们不可以让我们的自定义异常对象直接返回，我们这里需要让我们的自定义的异常对象也封装成我们的统一的数据格式，否则前台人员整不明白，这也是我们本节最重要的思想。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE59c595a37ad141f1ecfeb2d815054522.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc1123c8c0dc4c18c52f29b3cb70f5f82.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb550248c8b17351e41b23f7267af025a.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE48b091b8334f9ddb8479a194f9bbe4ed.png)

那么到此为止，我们SSM的内容就全部讲完了，此时我们已经完成了这个案例的所有内容

## 纯注解开发SSM

最后我们来学习一个补充内容，纯注解的方式来开发我们的SSM，我们将这一节独立出来为一章，其实主要内容还是对以前内容的复习

首先我们来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe5644171ab5129ec2cd74013e843e806.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9e4fcdd3d039ff99978b3ce06931dba9.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9ff5b89567d247a2a99f127ed1291ccb.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8989ee796c15f021678e97465e20e321.png)

