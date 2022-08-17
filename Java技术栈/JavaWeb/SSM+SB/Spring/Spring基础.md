# Spring简介

本章节我们就来正式学习Spring了，首先我们来介绍什么是Spring

## 框架的概念

所谓Spring就是我们平时开发的用的框架的一种，是一种经过验证的具有一定功能的半成品软件，我们的开发就可以基于其进行开发。使用框架进行开发开发具有以下好处

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8169c865ed24077dcf863b0f39406d16.png)

## Spring概念

我们先来介绍Spring是什么，我们之前说过Spring其实就是一种分层的JavaSE/EE应用的full-stack框架，那么其自己也有框架本身的好处，具体到Spring中就是就是有以下五个好处，分别是分层、JavaSE/EE、full-stack、轻量级、开源。接下来我们分别说说这五个好处

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5b3ee2d334f5e9a6ae182844d8ef9c50.png)

首先是分层，分层的好处意味着在Spring中，其下的各种层级的功能都能被独立拿出来使用，而不是说我们要用spring就必须要用他的所有，我们可以只使用他的一部分

第二个是JavaSE/EE，在Spring中不仅能用于JavaSE，还能用于JavaEE的开发。我们都知道在JavaEE中我们是将我们开发的项目分为三层的，分别是Servlet、service以及DAO，而对于Spring而言，其不仅在Dao层中有解决方案，其在Dao层之上的两层也有解决方案，也就是其技术覆盖度宽

第三个是full-stack，也就是一栈式，又称全栈，简而言之就是Spring中提供了一套的解决方案，各种问题的解决方案包括连接数据库等等等等一类的问题其都有，而且其还有一点特点，那就是如果我们用的一套技术或解决方案全部都是Spring的，那么我们的项目的整体效率就会得到显著的提高，原因在于内部的各种问题都先帮我们考虑到了，而如果我们只使用其一部分的话，那些问题就要自己去解决了，这个特点叫做粗粒度封装

第四个是轻量级，这个大家都懂了，占用内存小，资源消耗小

第五个是开源，这个最主要的作用就是无论谁都可以给这个项目发展，加功能

## Spring体系结构

接着我们来讲下Spring的体系结构

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE542628d135ecd23790e596c36f7f703f.png)

首先在Spring中，最重要的是核心容器，也就是Core Container，其下有Beans、Core、Conetext以及SpEL，后续我们会对这个四个进行逐一讲解，现在我们可以简单理解成Spring如果想要工作那么就要靠这四个内容。

在其之上是中间层，中间层里最重要的AOP，其也是Spring核心功能的体现之一，而Aspects简单来说，如果我们把AOP理解为接口，那么Aspects就是其实现类。后面的两个都是小功能，无关紧要

再往上就是其应用层技术，其第一块应用层技术就是Data Access，其意为数据访问，其下有ORM和JDBC，这两个我们都比较熟悉了，其整个结构可以理解为Spring的数据解决方案。其数据解决方案是在内部进行了接口的规范，而具体的实现类是要我们自己导入的，这样我们需要用什么样的解决方案，就导入对应的jar包就可以了。比如之前我们也说过，不同的数据库的JDBC的实现类也不一样，但都有一样的规范，那么我们在Spring中想用不同的数据库的话，就对应导入JDBC的对应实现类就可以了，内部已经帮我们准备好了。而整个这个部分在做的事情就是Integration，也就是集成，集成可以简单理解为其集合了多个技术，用户需要什么对应的技术直接拿来用就完了

除了数据层之外，其还有Web层的解决方案，其也做集成，其下有WebSocket、Servlet、Web、Portlet技术，都已经有了，我们需要直接拿来用就完了。而且Spring自己也有自己的Web实现，当然，我们用不用取决于我们

同时为了保证其自己做的功能的确能够使用，所以其提供了一套Test测试功能，用这个功能可以测试其功能是否可以用，是否完好

## Spring发展史

这个了解下就行，直接看图吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7fd6d9b02221b2db220df76e0c0f2fda.png)

然后是Spring的优势

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0f86f35a998afa1cae97916ca034c376.png)

# IoC

本章节我们来学习loC，这是一个比较重要的内容，所以要认真学

## 控制翻转（loC）概念

在学习本章内容之前，我们先来做一个板书约定

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEaf32e8cd8f1fccb1e88a20fcc21ded07.png)

## 耦合与内聚

我们在学习Ioc之前，我们先来学习关于耦合和内聚

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa157cd34dc91aa6a48864680b08fff7c.png)

所谓耦合指的是代码书写过程中所使用技术的结合紧密度，主要用于衡量软件中各个模块直接的结合程度。而内聚指的是代码书写过程中单个模块内部各组成部分的联系，用于衡量软件中各个功能模块的内部功能联系。我们程序员追求我们写出的项目以及代码是低耦合和高内聚的，也就是说，我们追求各个模块之间的联系要尽可能少，但是我们希望一个独立模块之间的内容要尽可能结合紧密，让一个模块只靠其自身就可以完成其自身的功能。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9b9d18e5c6b37036eb4a97801c4bf1ae.png)

接着我们来讲讲我们的工厂模式，我们最传统的调用资源的方式是应用程序与资源耦合，通过new对象的方式来达到调用我们的资源，这种方式违背了我们的低耦合的要求，因此我们往中间添加了一个工厂对象，由工厂来负责获得对象，此时就是工厂和资源耦合，还是违背了我们的低耦合要求，不但如此还显得脱裤子放屁多此一举。这时我们又引入了配置文件，将工程与配置文件结合在一起，我们的工厂通过配置文件来获取资源，此时是配置文件和资源高耦合，但是这个高耦合其实无所谓 ，因为在配置文件里进行修改不涉及到我们的源代码，我们修改之后将配置文件发送到服务器上就完了，不需要重新编译或者是发布我们的源代码。这时，我们的工程类和配置类，就组成了Spring的雏形，同时也是我们Spring中最核心的一个思想，IoC

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd598e7720d3da0719cd66d40b8727977.png)

IoC，也就是控制反转，Spring反向控制应用程序所需要使用的外部资源，在传统模式中，我们的资源的主控权是在类的手中的，而在Spring中，其通过loC模式将主控权给到了Spring

在IoC模式中，我们需要资源时不再需要主动new对象，只需要通过loC容器提供给我们就可以了。IoC容器是Spring框架中自己设置好的容器，其可以将我们的资源提供给我们

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE193d5e300ae8d9dc1b720f3ccb4de553.png)

## 入门案例制作

既然我们现在已经学习了IoC模式，那么现在我们就来做一个入门案例来加深我们的理解。先来看看我们的案例需求

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb129fafa47abcbe9aeac6825b51fc905.png)

我们要做的案例是模拟三次架构中从表现层调用业务层的功能，这里我们的表现层就直接用主方法模拟了，正式一点的话是应该要使用junit来进行测试的，这里我们处于学习阶段，就随意一点吧

接下来让我们来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE11e90d0daaf9d92974cbd0a574dc3b86.png)

首先我们导入我们的spring坐标，在配置文件中写入代码如下

```
<dependencies>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.1.9.RELEASE</version>
  </dependency>
</dependencies>
```

然后我们编写业务层的接口和实现类，以及表现层的实现类，在其中写入一个打印语句，然后我们建立spring的配置文件，我们所需要的配置文件就直接从Spring的官网中扒下来就行了。接着我们在resource文件夹中建立一个applicationContext.xml的文件，我们规定我们的spring的标准名称为这个，我们以后创建spring的配置文件都叫这个名字，然后我们将对应的配置文件复制进去，然后配置我们对应的资源

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    <!-- 1.创建spring控制的资源-->
    <bean id="userService" class="com.itheima.service.impl.UserServiceImpl"/>
</beans>
```

我们这里创建了对应控制的资源，其实id我们是可以随便写的，但是为了便于人类读者的阅读，我们这里统一用接口来命名，后面的class属性就直接填入我们对应的底层实现类的路径

然后我们创建一个测试类并写入代码如下

```
import com.itheima.service.UserService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class UserApp {
    public static void main(String[] args) {
        //2.加载配置文件
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        //3.获取资源
        UserService userService = (UserService) ctx.getBean("userService");

        userService.save();
    }
}

```

运行此代码我们能够在控制台上得到我们所需要的结果，这里的内容我们先不做解释，我们只要知道我们这里做了加载配置文件并获取了对应的资源的事就可以了，这是的的确确的事情，这里也是我们的IoC的内容之一，就是利用配置文件获得我们所需要的对象并执行其对应的方法。

## bean的基本配置

我们先来说说bean标签，首先有大的beans标签，然后其下有bean的小标签，bean下有三个属性要介绍，第一个是id，id属性是我们自己设定的唯一标识，我们这里推荐使用我们的实现类接口来统一命名，然后是class属性，该属性要填入实现类的路径，最后是name属性，该属性可以用于取别名，在这个属性中我们可以为我们的类取别名并使用，可以取多个，用,分隔开

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE00958b7db149dd212f138c0e82427823.png)

请看我们的配置代码

```
<!-- bean可以定义多个名称，使用name属性完成，中间使用,分割-->
    <bean id="userService" name="userService2,userService1" class="com.itheima.service.impl.UserServiceImpl"/>
```

## scope属性

本节我们来学习bean标签下的scope属性，其意为范围，可以指定bean的作用范围，其有三个可选择值，分别是singlet、prototype以及其他，其他的内容我们具体看图，我们这里主要讲singleton和prototype

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf69d6bd7ef82a49a1372e07d28a17c80.png)

singleton是我们默认的配置，如果我们不设定这个标签的内容，那么其默认就是singleton，其作用是指定我们的类是一个单例的对象，当我们指定了singleton时，我们的指定实现类的对象是在我们加载spring容器类的时候创建出来的，也就是执行ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");这个语句的时候，此后我们再进行任何创建对应类的动作，都是在重复获得最开始创建的对象。但如果我们设定的prototype，那么我们的对象就是非单例的，其能够正确创造出多个对象，且其对象是在new的时候才会产生的

但无论是那种方式，我们的对象都是保存在spring容器中的。他们两个本质区别在于创建的时机不同

## bean的生命周期控制

bean下有两个属性，分别控制器初始化和销毁，这两个属性是init-method和destroy-method，我们使用这两个方法之前要在具体的实现类里定义这两个方法，然后要在对应的属性下填入对应的方法名，这样当我们的对象初始化或者是销毁时对应的方法就会执行了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE32aa401181798f7ee608e701bdcd1cbf.png)

这里的注意事项是，我们的初始化和销毁的方法执行与我们的scope属性的设置息息相关，首先当我们的scope设置为singleton时，spring容器有且仅有一个对象，init方法在创建容器时仅执行一次，同时我们的对象放在我们的spring容器中，其会调用destroy方法（由于我们的程序关闭得太快，所以如果我们不主动先关闭spring容器，我们是无法在控制台上看到destroy方法的执行的）。而当我们的scope设置为prototype时时，spring容器如果创建同一类型的多个对象时，每创建一次init方法就执行一次，但是销毁时，其会由垃圾回收机制gc()控制，destroy方法将不会执行

## 静态工厂与实例工厂创建bean

bean对象的创建方式有两种，一种是静态工厂形式创建bean，一种是动态工厂的形式，这个内容我们做了解就可以了，因此会将的比较简略

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE96d392d2375b246d10578e3e8bf42894.png)

想用静态工厂创建bean，首先要创建出对应的静态工厂类，然后我们要在bean下加入属性class，和factory-method，前者填入工厂类的地址，后者填入工厂类的对应方法，同时工厂类中要有返回实现类的静态方法，然后我们在测试类中同样调用getBean方法就可以获得我们所需要的对象了

```
<!--静态工厂创建bean-->
<!--<bean id="userService4" name="userService2,userService1" class="com.itheima.service.UserServiceFactory" factory-method="getService"/>-->
```

然后是实例工厂创建bean，实例工厂创建对应的bean，首先要创建对应的工厂类，类中要写入获得实现类的对象方法，但是我们这里不填入static关键词，然后我们在配置文件中先写入实例工厂对应的Bean，提供id，class写对应地址，再写入bean，这个bean同样提供id，这个id是我们测试类里用的，而我们这里的factory-bean则填入我们第一个bean标签的id，方法则是我们定义好的getService方法，想想也知道是我们这样配置，其会自动帮我们完成创建对应的对象并返回

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1049128b0fe7eebf757ee47972eda460.png)

最后来看看配置代码

```
<!--实例工厂对应的bean-->
<bean id="factoryBean" class="com.itheima.service.UserServiceFactory2"/>
<!--实例工厂创建bean，依赖工厂对象对应的bean-->
<bean id="userService" factory-bean="factoryBean" factory-method="getService"/>
```

## 依赖注入概念（DI）

我们之前学习过IoC，其为控制翻转，在IoC模式中，应用程序所需要的资源由IoC容器提供。而DI其实和IoC是一个意思，只不过是另外一种说法，DI为依赖注入，指的是应用程序运行所依赖的资源由Spring为其提供，资源进入应用程序的方式称为注入

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE015e0c0891369b1c92bb54f816013329.png)

### set注入

介绍完了注入之后，现在我们正式来学习注入的方式，注入的方式有很多种，我们先来学习最主流的那种，也就是set注入

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE831e5a19b23a790aead5e5dcfde1a4ac.png)

set注入的方式为bean提供资源，现在我们假设我们的service层里需要Dao层的资源，同时还需要一些int和String类型的资源，那么我们使用set注入时，就应该先将我们的service层的代码写入如下

```
package com.itheima.service.impl;

import com.itheima.dao.UserDao;
import com.itheima.service.UserService;

public class UserServiceImpl implements UserService {

    private String version;

    public void setVersion(String version) {
        this.version = version;
    }

    private int num;

    public void setNum(int num) {
        this.num = num;
    }

    private UserDao userDao;
    //1.对需要进行注入的变量添加set方法
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    @Override
    public void save() {
        System.out.println("user service running..."+num+" "+version);
        userDao.save();
    }
}

```

可以看到我们这里先声明了其拥有的成员变量，然后提供了对应的set方法，注意，提供属性的set方法是必须要做的，是set注入必不可少的一步，然后我们可以创建对应的Dao层接口和其实现类，接着我们需要在配置文件中进行相应的配置实现注入，我们需要在其配置文件中写入代码如下

```
<bean id="userService" name="userService2,userService1" class="com.itheima.service.impl.UserServiceImpl">
    <!--3.将要注入的引用类型的变量通过property属性进行注入，对应的name是要注入的变量名，使用ref属性声明要注入的bean的id-->
    <property name="userDao" ref="userDao"/>
    <property name="num" value="666"/>
    <property name="version" value="itheima"/>
</bean>

<!--2.将要注入的资源声明为bean-->
<bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"/>
```

我们这里需要将我们的bean改为开口的形式，否则我们无法使用其下的子标签property，property就是用于将我们的对应类型的变量进行注入的标签，其下有两个属性，分别是name和ref，name的作用是要填入我们注入的变量名，该名规定是我们在service提供的set方法名字的除去set后面的名字，然后ref则填入我们需要引入的引用变量，一般填写一个id，而这个id的呢日用需要我们自己创建，为了创建这个id，我们需要额外声明一个bean，然后在其下赋予唯一指定的id，class属性赋予其实现类的地址，接着我们上面的property标签的ref属性就填入其id就可以了。如果我们声明的是基本数据类型，那么name就直接填写其对应的名字，值就用value属性来赋予，并且直接写入对应的值就可以了。String本身应该是一个引用变量，但是这里其将其进行特殊处理，其也用基本数据类型的注入方式来注入

这个set注入是我们最常用的注入方式，我们要重点记忆。最后，一个bean可以有多个property标签，可以给多个属性注入对应值

### 构造方法注入

本节内容只是作为了解，我们稍微看看就行了，因为我们只是偶尔使用这个，甚至可能是不使用

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE58eda2438b8e1beeaaf482ac3c0a94e0.png)

使用构造器注入需要我们在对应的需要被提供资源的类中，在本题中也就是service类中，需要我们提供对应的构造方法，而我们在配置文件中则使用constructor-arg标签来进行注入，name输入构造器中传入参数的名字，而value和ref则要输入我们希望赋予的值，同样的，使用ref代表其为引用变量，而使用value代表其为基本数据类型的变量。如果我们没有对方法进行任何定位，那么我们输入的对应标签就需要自己对应构造方法中传入参数的位置，否则会报错。

```
<bean id="userService" class="com.itheima.service.impl.UserServiceImpl">
    <!--使用构造方法进行set注入，需要保障注入的属性与bean中定义的属性一致-->
    <!--一致指顺序一致或类型一致或使用index、name、type解决该问题-->
    <constructor-arg name="version" value="itcast"/>
    <constructor-arg name="num" value="666666"/>
    <constructor-arg name="userDao" ref="userDao"/>
</bean>

<!--2.将要注入的资源声明为bean-->
<bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"/>
```

但构造器注入中还有一些方法可以帮助我们的对方法的定位，我们利用其进行定位而不用考虑顺序问题，分别是index、type以及name。这里我们只推荐使用name，前两个就突出一个没用，看看就行了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE40e225cd1275477f656139ce8320eb45.png)

### 集合注入

接着我们来学习集合类型数据的注入，我们有五种集合类型，分别是array、list、set、map、props。其都使用property或constructor-arg标签进行集合数据类型的注入

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3d0e403d06b14a46282b751bcf7b3215.png)

首先来看看list集合的注入方式，我们注入list集合的数据的方式是通过property标签，其下有属性可以指定其名字

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2278ae82597e898db16cd5fe9887e8e9.png)

然后是props

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb9193ce524c24804a0b6c6445cff18b3.png)

接着是array

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE80e5ba8f3e6811bd7a26951675e24f63.png)

然后是set

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7af985e466236d20414744cffa45e35f.png)

最后是map

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5f647d7f98e2685d7187b5f207b8846c.png)

这里list和prop是比较重要的，我们会经常用到的，其他的我们稍微看看就差不多得了

## SpringDebug

在正式学习之前，注入集合之前，我们先来学习下Spring的报错是怎么看的，请看下面的一个报错

```
       D:\JDK\8\bin\java.exe "-javaagent:E:\BaiduNetdiskDownload\idea\IntelliJ IDEA\IntelliJ IDEA 2020.3.2\lib\idea_rt.jar=54739:E:\BaiduNetdiskDownload\idea\IntelliJ IDEA\IntelliJ IDEA 2020.3.2\bin" -Dfile.encoding=GBK -classpath D:\JDK\8\jre\lib\charsets.jar;D:\JDK\8\jre\lib\deploy.jar;D:\JDK\8\jre\lib\ext\access-bridge-64.jar;D:\JDK\8\jre\lib\ext\cldrdata.jar;D:\JDK\8\jre\lib\ext\dnsns.jar;D:\JDK\8\jre\lib\ext\jaccess.jar;D:\JDK\8\jre\lib\ext\jfxrt.jar;D:\JDK\8\jre\lib\ext\localedata.jar;D:\JDK\8\jre\lib\ext\nashorn.jar;D:\JDK\8\jre\lib\ext\sunec.jar;D:\JDK\8\jre\lib\ext\sunjce_provider.jar;D:\JDK\8\jre\lib\ext\sunmscapi.jar;D:\JDK\8\jre\lib\ext\sunpkcs11.jar;D:\JDK\8\jre\lib\ext\zipfs.jar;D:\JDK\8\jre\lib\javaws.jar;D:\JDK\8\jre\lib\jce.jar;D:\JDK\8\jre\lib\jfr.jar;D:\JDK\8\jre\lib\jfxswt.jar;D:\JDK\8\jre\lib\jsse.jar;D:\JDK\8\jre\lib\management-agent.jar;D:\JDK\8\jre\lib\plugin.jar;D:\JDK\8\jre\lib\resources.jar;D:\JDK\8\jre\lib\rt.jar;C:\Users\22592\IdeaProjects\MyProject\Spring\target\classes;E:\maven\repository\org\springframework\spring-context\5.1.9.RELEASE\spring-context-5.1.9.RELEASE.jar;E:\maven\repository\org\springframework\spring-aop\5.1.9.RELEASE\spring-aop-5.1.9.RELEASE.jar;E:\maven\repository\org\springframework\spring-beans\5.1.9.RELEASE\spring-beans-5.1.9.RELEASE.jar;E:\maven\repository\org\springframework\spring-core\5.1.9.RELEASE\spring-core-5.1.9.RELEASE.jar;E:\maven\repository\org\springframework\spring-jcl\5.1.9.RELEASE\spring-jcl-5.1.9.RELEASE.jar;E:\maven\repository\org\springframework\spring-expression\5.1.9.RELEASE\spring-expression-5.1.9.RELEASE.jar UserApp
       四月 18, 2022 3:33:22 下午 org.springframework.context.support.AbstractApplicationContext refresh
       警告: Exception encountered during context initialization - cancelling refresh attempt: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'userService' defined in class path resource [applicationContext.xml]: Cannot resolve reference to bean 'userDao' while setting constructor argument; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'userDao' defined in class path resource [applicationContext.xml]: Cannot resolve reference to bean 'userDao' while setting constructor argument; nested exception is org.springframework.beans.factory.BeanCurrentlyInCreationException: Error creating bean with name 'userDao': Requested bean is currently in creation: Is there an unresolvable circular reference?
       Exception in thread "main" org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'userService' defined in class path resource [applicationContext.xml]: Cannot resolve reference to bean 'userDao' while setting constructor argument; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'userDao' defined in class path resource [applicationContext.xml]: Cannot resolve reference to bean 'userDao' while setting constructor argument; nested exception is org.springframework.beans.factory.BeanCurrentlyInCreationException: Error creating bean with name 'userDao': Requested bean is currently in creation: Is there an unresolvable circular reference?
       at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveReference(BeanDefinitionValueResolver.java:314)
       at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveValueIfNecessary(BeanDefinitionValueResolver.java:110)
       at org.springframework.beans.factory.support.ConstructorResolver.resolveConstructorArguments(ConstructorResolver.java:676)
       at org.springframework.beans.factory.support.ConstructorResolver.autowireConstructor(ConstructorResolver.java:188)
       at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.autowireConstructor(AbstractAutowireCapableBeanFactory.java:1341)
       at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1187)
       at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:555)
       at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:515)
       at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:320)
       at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:222)
       at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:318)
       at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:199)
       at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:845)
       at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:877)
       at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:549)
       at org.springframework.context.support.ClassPathXmlApplicationContext.<init>(ClassPathXmlApplicationContext.java:144)
       at org.springframework.context.support.ClassPathXmlApplicationContext.<init>(ClassPathXmlApplicationContext.java:85)
       at UserApp.main(UserApp.java:8)
       Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'userDao' defined in class path resource [applicationContext.xml]: Cannot resolve reference to bean 'userDao' while setting constructor argument; nested exception is org.springframework.beans.factory.BeanCurrentlyInCreationException: Error creating bean with name 'userDao': Requested bean is currently in creation: Is there an unresolvable circular reference?
       at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveReference(BeanDefinitionValueResolver.java:314)
       at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveValueIfNecessary(BeanDefinitionValueResolver.java:110)
       at org.springframework.beans.factory.support.ConstructorResolver.resolveConstructorArguments(ConstructorResolver.java:662)
       at org.springframework.beans.factory.support.ConstructorResolver.autowireConstructor(ConstructorResolver.java:188)
       at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.autowireConstructor(AbstractAutowireCapableBeanFactory.java:1341)
       at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1187)
       at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:555)
       at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:515)
       at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:320)
       at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:222)
       at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:318)
       at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:199)
       at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveReference(BeanDefinitionValueResolver.java:303)
... 17 more
       Caused by: org.springframework.beans.factory.BeanCurrentlyInCreationException: Error creating bean with name 'userDao': Requested bean is currently in creation: Is there an unresolvable circular reference?
       at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.beforeSingletonCreation(DefaultSingletonBeanRegistry.java:339)
       at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:215)
       at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:318)
       at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:199)
       at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveReference(BeanDefinitionValueResolver.java:303)
... 29 more

       Process finished with exit code 1

```

上面的报错很长，我们怎么看呢？我们直接从下往上看，每次我们就看Caused by后的内容，看看里面的错误是不是我们自己定义的东西出了问题，主要找对应的错误关键词，比如上面我们可以看到第一个Caused by是Bean创建异常，如果看一个解决不了就继续往上看，按照同样的原则继续往上排查，直到到达第一行的最后一串字符，如果每一个错误都不是我们定义的类出现的错误，那说明很有可能是我们的配置文件写错了

## 不同集合注入方式演示

接着我们来看看我们的各种不同集合的注入方式，在这里，我们为了进行演示，我们要创建一些对应的类和接口，首先我们重新创建我们的UserDao接口并重写其实现类，代码如下

```
package com.itheima.dao.impl;

import com.itheima.dao.UserDao;

public class UserDaoImpl implements UserDao{

    private String username;
    private String pwd;
    private String driver;

    public UserDaoImpl(String driver,String username, String pwd) {
        this.driver = driver;
        this.username = username;
        this.pwd = pwd;
    }

    public void save(){
        System.out.println("user dao running..."+ username+" "+pwd + " "+ driver);
    }
}
```

这份代码本身倒是可有可无，只是修改这份代码的话，我们的配置文件里的有些代码也要改，我真的懒得，所以我们这里就一开始就创建好之前的代码吧

然后我们创建BookDao接口并写入其实现类代码如下

```
package com.itheima.dao.impl.impl;

import com.itheima.dao.BookDao;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.HashSet;
import java.util.Properties;

public class BookDaoImpl implements BookDao {

    private ArrayList al;
    private Properties properties;
    private int[] arr;
    private HashSet hs;
    private HashMap hm;



    public void setAl(ArrayList al) {
        this.al = al;
    }

    public void setProperties(Properties properties) {
        this.properties = properties;
    }

    public void setArr(int[] arr) {
        this.arr = arr;
    }

    public void setHs(HashSet hs) {
        this.hs = hs;
    }

    public void setHm(HashMap hm) {
        this.hm = hm;
    }



    @Override
    public void save() {
        System.out.println("book dao running...");
        System.out.println("ArrayList"+al);
        System.out.println("Properties"+properties);
        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);
        }
        System.out.println("HashSet"+hs);
        System.out.println("HashMap"+hm);
    }
}

```

可以看到我们在BookDao里提供了各种不同类型的集合，并提供了set方法，重写save方法并将各种集合的内容进行了输出

然后我们在service层里写入我们的实现类代码如下

```
package com.itheima.service.impl;

import com.itheima.dao.BookDao;
import com.itheima.dao.UserDao;
import com.itheima.service.UserService;

public class UserServiceImpl implements UserService {

    private UserDao userDao;
    private BookDao bookDao;
    private int num;
    private String version;

    public UserServiceImpl() {
    }

    public UserServiceImpl(UserDao userDao, int num, String version){
        this.userDao = userDao;
        this.num = num;
        this.version = version;
    }

    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }

    public void setVersion(String version) {
        this.version = version;
    }
    public void setNum(int num) {
        this.num = num;
    }
    //1.对需要进行诸如的变量添加set方法
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void save() {
        System.out.println("user service running..."+num+" "+version);
        userDao.save();
        bookDao.save();
    }
}

```

可以看到我们这里service层的实现类代码中有UserDao和BookDao的属性并提供了对应的set方法，然后在save方法里调用了这两个对象的save方法

搞定了这些之后，我们就可以编写我们的配置代码了，我们可以写入配置代码如下

```
<bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl">
    <constructor-arg index="2" value="123"/>
    <constructor-arg index="1" value="root"/>
    <constructor-arg index="0" value="com.mysql.jdbc.Driver"/>
</bean>

<bean id="userService" class="com.itheima.service.impl.UserServiceImpl">
    <property name="userDao" ref="userDao"/>
    <property name="bookDao" ref="bookDao"/>
</bean>

<bean id="bookDao" class="com.itheima.dao.impl.impl.BookDaoImpl">
    <property name="al">
        <list>
            <value>itheima</value>
            <value>66666</value>
        </list>
    </property>

    <property name="properties">
        <props>
            <prop key="name">itheima666</prop>
            <prop key="value">666666</prop>
        </props>
    </property>

    <property name="arr">
        <array>
            <value>123456</value>
            <value>66666</value>
        </array>
    </property>

    <property name="hs">
        <set>
            <value>itheima</value>
            <value>66666</value>
        </set>
    </property>

    <property name="hm">
        <map>
            <entry key="name" value="itheima666666"/>
            <entry key="value" value="666666666666"/>
        </map>
    </property>
</bean>
```

我们这里解释一下这个配置代码的执行过程，首先我们要实现的的内容是创建对应的userService对象，然后调用其方法，所以我们在配置文件中首先通过bean标签从引入了userService对象，并指定了其实现类的地址，接着通过property标签给其两个属性赋值，但是这两个变量是引用变量，因此我们要分别将这两个对象创建出来，因此我们多设置了两个bean，分别用于给两个对象的属性赋值并创造出来，然后我们的内容就能成功运行了

我们接着进入我们正式的学习，就是看看其是如何给里面不同的集合赋值的，首先是list，我们这里的name要求和我们在具体的实现类中的属性名保持一致，然后在property标签下，调用其子标签list，再调用其子标签value，该标签可以用于给list集合赋值

然后是prop集合，前面的内容差不多，这里不赘述，不同的是其要指定key，其内容都是字符串，然后在内部写入对应的值

至于array的数组集合，我们这里具体创建的是数字数组，因此我们不可以给其赋予字符串的值

而set集合的赋值方式和list几乎一模一样，没啥值得说的，当然，其也具有set集合的特性，如果给其赋予同样的值，其会将第一个值覆盖

最后是map集合，map集合赋值时需要使用map标签，其下调用entry子标签，子标签下要输入key和value属性，这是什么东西，大家都懂得，这里不多说了，同时其也有map集合的特性

最后运行我们的测试类就可以得到我们想要的结果了，完成一点我们再来看看测试类的代码

```
import com.itheima.service.UserService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class UserApp {
    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserService userService = (UserService) ctx.getBean("userService");
        userService.save();
    }
}
```

## 使用p命名空间简化配置

这个内容我们主要作为了解，使用p命名空间简化配置简单来说就是我们可以将一个长的命名用一个字母来进行代替，这听起来很不错，但实际使用的效果却并没有那么好，因此本节作为了解内容。要使用p命名空间，我们需要写入下图中划上横线的语句

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE02cc2a518bfbcb1db10eb08982eb858a.png)

那么使用p命名空间，我们可以将我们的代码原来注入的代码修改如下，可以看到，也没短到哪里去说实话，所以说这玩意真的是没啥作用

```
<!--        <bean id="userService" class="com.itheima.service.impl.UserServiceImpl">
            <property name="userDao" ref="userDao"/>
            <property name="bookDao" ref="bookDao"/>
        </bean>-->

    <bean id="userService"
           class="com.itheima.service.impl.UserServiceImpl"
          p:userDao-ref="userDao"
          p:bookDao-ref="bookDao"/>
```

## SpEL

这块内容的作用就是我们输入要注入的元素可以统一使用value进行赋值，这是其唯一的好处，当然，爱用不用，无所谓其实

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE03a1f318ddc4a42d8bf0b8a77b1b0f5a.png)

我们来看看其对应的各种使用方式，具体请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc48756db54d24ffdb5c4d06377c9bf80.png)

那么根据EL表达式，我们可以将我们的配置代码修改如下

```
<!--       <bean id="userService" class="com.itheima.service.impl.UserServiceImpl">
            <property name="userDao" ref="userDao"/>
            <property name="bookDao" ref="bookDao"/>
        </bean>-->

    <bean id="userService" class="com.itheima.service.impl.UserServiceImpl">
        <property name="userDao" value="#{userDao}"/>
        <property name="bookDao" value="#{bookDao}"/>
        <property name="num" value="#{6666666}"/>
        <property name="version" value="#{'itcast'}"/>
    </bean>
```

我们上面就演示了最基本的EL表达式的应用，具体更多的应用还有很多，可以看上图，其中有些比较复杂，我们敲代码的时候要小心，别到时候敲错了就寄了

## 读取properties文件信息

我们的一些基本信息都是要求写在properties文件中的，本节我们就来学习将数据写在properties中，并且将其读取到我们的代码中

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE81205cc0d78d9a028f779061debd0d1c.png)

首先我们要创建对应的properties文件，然后我们创建对应的userDao和BookDao的实现类，我们这里用BookDao实现类来承载我们的properties的数据，这个主要进行一个测试就行了

但是要让我们的xml文件能够读取到properties文件的话，我们还需要开启context的命名空间支持，首先在我们的配置文件中写入代码如下

```
<!--1.加载context命名空间的支持-->
<!--xmlns:context="http://www.springframework.org/schema/context"-->
```

然后我们后面还要写入额外的两句

```
http://www.springframework.org/schema/context
https://www.springframework.org/schema/context/spring-context.xsd">
```

最后我们的xml的开头的固定配置代码就是下面这样的

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">
```

固定配置的代码会随着我们以后不断地使用而添加，但我们也不必因此而感到焦虑，觉得以后每次使用都要添加很麻烦，我们添加而又没有用到的东西是允许存在的，以后我们越用越多没关系，直接把一大段复制过去当开头来创建新的就完了，一次搞定解君愁

同时我们还需要加载我们的配置文件，加载配置文件的代码如下

```
<!--2.加载配置文件-->
<context:property-placeholder location="classpath:*.properties"/>
```

这里使用的context标签，我们需要调用其下的location属性，在里面写入我们的properties文件的全称，通常我们还会在前面加上类路径，如果我们填入*，即是说明其会加载能扫描到的所有配置文件

最后我们可以写入我们的注入代码如下

```
<bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl">
    <property name="userName" value="${username}"/>
    <property name="password" value="${pwd}"/>
</bean>
<bean id="bookDao" class="com.itheima.dao.impl.impl.BookDaoImpl"/>

<bean id="userService" class="com.itheima.service.impl.UserServiceImpl">
    <property name="userDao" ref="userDao"/>
    <property name="bookDao" ref="bookDao"/>
</bean>

```

可以看到我们这里赋值采用的是${}的格式来读取配置项，这也是我们在properties文件里常用的获取文件的符号

## import导入配置文件

如果我们的配置文件全部都写在一个xml文件中，那么我们的配置文件就会非常乱，以后维护起来就很难，因此我们要尽量保持我们的配置文件的有序性，此时我们的imoprt就起作用了，我们可以创建好几个xml文件，然后每一个xml文件负责放置一种类型的配置代码，然后主xml文件里就只用将这些子xml文件导入就可以了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE03538cb7a9216e58768fc412301286af.png)

比方说我们现在有book和user的实现类，那么我们可以创建一个book一个user的xml，这里我们创建时，都在resources文件夹中创建，同时我们要注意我们创建的xml文件的名字，我们统一使用原来的名字加上-对应类名的方式来命名，这样才让我们的命名格式变得统一，那么我们可以在user的xml文件中写入代码如下

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl">
        <property name="userName" value="${username}"/>
        <property name="password" value="${pwd}"/>
    </bean>


    <bean id="userService" class="com.itheima.service.impl.UserServiceImpl">
        <property name="userDao" ref="userDao"/>
        <property name="bookDao" ref="bookDao"/>
    </bean>
    
</beans>
```

可以看到我们这里只放user相关的代码，然后在book.xml文件中写入代码如下

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <bean id="bookDao" class="com.itheima.dao.impl.impl.BookDaoImpl"/>

</beans>
```

最后我们在主xml文件中写入代码如下

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">


    <!--1.加载context命名空间的支持-->
    <!--xmlns:context="http://www.springframework.org/schema/context"-->

    <!--2.加载配置文件-->
    <context:property-placeholder location="classpath:*.properties"/>

    <import resource="applicationContext-user.xml"/>
    <import resource="applicationContext-book.xml"/>

</beans>
```

我们的主xml代码就是直接将两个子xml文件引入就行了，其实我们还有一种引入方式就是子xml先引入到另一个子xml中，然后将被引入的xml引入到主xml中，但是这种方式并不好，我们不推荐使用，了解下就行了

接着我们来介绍一个加载模式，就是直接让Spring容器加载多个配置文件，这样就绕过了最开始的主配置文件了，直接引入子配置文件就完了，但是这种方式几乎不用

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEaf9568778a5da09b47f76c4982df3bf1.png)

我们只需要将我们的测试类代码更改如下即可，此时绕过了applicationContext，直接利用其子配置文件运行

```
import com.itheima.service.UserService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class UserApp {
    public static void main(String[] args) {
        //ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext-user.xml","applicationContext-book.xml");
        UserService userService = (UserService) ctx.getBean("userService");
        userService.save();
    }
}
```

接着我们要讲Spring容器中的bean定义冲突问题，假如我们定义了两个名字不同，而会对同一个对象注入值的xml文件的话，那么后面的会覆盖前面的。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7914953def6280570495b6427c2594f3.png)

举个例子，假设我们现在创建了两个book的xml文件，并且我们在主xml文件中写入代码如下，那么book2.xml文件的内容才会是最终我们对象被赋值的内容

```
<import resource="applicationContext-user.xml"/>
<import resource="applicationContext-book.xml"/>
<import resource="applicationContext-book2.xml"/>
```

这个了解下就行了，因为实际开发中你只要遵守规范，根本不可能出现这种问题

## ApplicationContext对象层次结构

这个内容属于了解内容，自己看看就差不多得了，具体信息看下图吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9a9f9170c4bc084bf155251d6cd12229.png)

## 第三方bean的配置方式

我们之前学习的都是配置我们自己的创建的对象，那如果我们想要配置第三方的对象呢，那我们应该要怎么办呢？比方说我们这里希望我们配置我们阿里云的数据源方案Druid，那我们应该怎么办？当然，首先就要在maven中导入其对应的依赖是吧，没有依赖我们根本用不了

然后我们就要考虑使用什么方式来配置我们的Druid，我们知道一般的配置注入方式有两种，分别是构造器方式和set注入方式，我们可以查看其内部的构造方法和set方法，找到适合我们用于创建对象的方法来创建对象，比如说在Druid中，我们发现去构造方法少，但是set方法是真的多，比如说通过密码，通过url等等，都可以创建，那么我们可以随便选，当然，如果我们实在搞不懂的话，可以点入源码去看看

那么最后我们可以构造我们的配置代码如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEadbb6f7b18168b9dc36d51d1332bf95d.png)

可以看到我们这里指定一个对象，然后往里面注入创建该对象所需要的值，这样我们就可以通过配置文件来创建出我们所需要的对象了。

# 综合案例

学习完了spring的基础环境之后，我们现在来正式搞一个spring的案例

## spring整合mybatis环境介绍

先来看看我们的案例分析，先对比下非spring环境与spring环境下的步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa2eb4b016e2734248ef245e81fe59ac0.png)

首先我们要明白，我们要完成的案例的内容是，通过spring框架实现从数据库中查找出对应的数据并封装成对象后在控制台上打印

接着我们来看看我们的基础准备工作

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6d4373b2923a890af4f8cec50082c8ce.png)

这里我们有一点要注意，就是我们用于查找数据的dao层不需要我们去定义，我们利用代理自动生成

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe12fba49c955e98fc49c614cd16d12f5.png)

首先我们先来讲讲关于maven的pom文件里，dependency标签，也就是写入依赖的作用，我们在dependency标签中只要输入对应的地址，我们的idea就会根据这个地址自动将其对应的包导入进来，其实就相当于是自动导入jar包，为了完成我们的案例，我们首先要在我们的pom.xml文件中写入如下代码

```
<dependencies>
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.3</version>
  </dependency>
  <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
  </dependency>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.1.9.RELEASE</version>
  </dependency>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.1.9.RELEASE</version>
  </dependency>
  <dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.16</version>
  </dependency>
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>1.3.0</version>
  </dependency>
```

可以看到，我们这里先写入了mybatis的依赖，然后我们写入了mysql的依赖，接着是spring框架的依赖，然后是druid连接池的依赖，最后是spring整合mybatis的依赖（注意这个jar是属于mybatis的，我猜测任何整合类技术都归属于其jar的提供方，也就是被整合方，而不是整合方，比如这里是属于mybatis而不是属于spring），我们先导入这些依赖的目的非常简单，就是因为我们的项目中需要用到这些依赖，也就是需要用到这些jar包，所以我们需要导入。这里值得一提的是我们注意到我们还多导入了一个spring-jdbc的依赖，之所以导入这个依赖是因为在spring中其有其自己的jdbc策略，我们之前导入了spring-context，其下还包括了许多该jar包会用到的其他jar包，其也将我们导入了，但是其不一定用到jdbc，因此其这里并没有帮我们自动导入spring中的jdbc的jar包，因此我们这里还需要手动导入，否则会报错

然后我们需要介绍下我们的项目里所具有的内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEccbcaef65f389011d5a6fcee4eba5fb9.png)

可以看到我们的项目的整体内容其实是先创建dao层的对应接口，然后对应接口的实现类由资源中的AccountDao.xml来自动实现，接着我们就要通过调用service接口的实现类来调用Dao层的查询方法，最后实现查询

## 具体实现

我们首先需要创建service层的接口和其实现类，其接口代码如下

```
package com.itheima.service;

import com.itheima.domain.Account;

import java.util.List;

public interface AccountService {

    void save(Account account);

    void delete(Integer id);

    void update(Account account);

    List<Account> findAll();

    Account findById(Integer id);

}
```

其实现类代码如下

```
package com.itheima.service.impl;


import com.itheima.dao.AccountDao;
import com.itheima.domain.Account;
import com.itheima.service.AccountService;

import java.util.List;

public class AccountServiceImpl implements AccountService {

    private AccountDao accountDao;

    public void setAccountDao(AccountDao accountDao) {
        this.accountDao = accountDao;
    }

    public void save(Account account) {
        accountDao.save(account);
    }

    public void update(Account account){
        accountDao.update(account);
    }

    public void delete(Integer id) {
        accountDao.delete(id);
    }

    public Account findById(Integer id) {
        return accountDao.findById(id);
    }

    public List<Account> findAll() {
        return accountDao.findAll();
    }
}

```

可以看到实现类的代码其实就是在调用Dao层的代码，那么接着我们来看看Dao层接口的代码

```
package com.itheima.dao;

import com.itheima.domain.Account;

import java.util.List;

public interface AccountDao {

    void save(Account account);

    void delete(Integer id);

    void update(Account account);

    List<Account> findAll();

    Account findById(Integer id);
}

```

Dao层接口是通过配置文件动态实现的，我们继续来看其对应的配置文件

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.itheima.dao.AccountDao">

    <!--配置根据id查询-->
    <select id="findById" resultType="account" parameterType="int">
        select * from account where id = #{id}
    </select>

    <!--配置查询所有-->
    <select id="findAll" resultType="account">
        select * from account
    </select>

    <!--配置保存-->
    <insert id="save" parameterType="account">
        insert into account(name,money)values(#{name},#{money})
    </insert>

    <!--配置删除-->
    <delete id="delete" parameterType="int">
        delete from account where id = #{id}
    </delete>

    <!--配置更新-->
    <update id="update" parameterType="account">
        update account set name=#{name},money=#{money} where id=#{id}
    </update>
</mapper>
```

可以看到我们这里指定了对应的命名空间，然后写入了对应的查询方法，如果忘记了这一部分的内容，可以去mybatis章节复习。然后我们先在我们的applicationContext.xml文件下写入对应的代码如下

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <!--加载perperties配置文件的信息-->
    <context:property-placeholder location="classpath:*.properties"/>

    <!--加载druid资源-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

    <!--配置service作为spring的bean,注入dao-->
    <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
        <property name="accountDao" ref="accountDao"/>
    </bean>
```

我们这里先对我们的xml文件的固定格式进行了引入，使得我们可以加载配置文件的信息，然后在第10-11行加载了我们的配置文件的信息，配置文件的内容其实就是连接我们的druid连接池的一些对应的数据，然后我们在其下创建对应的bean标签，然后通过加载properties的方式给对应属性赋值。最后我们当然还需要创建service实现类，因此我们又创建了一个bean标签生成此对象，同时给其下的属性赋值，当然，这时候我们的accountDao对象还没有生成，所以子在编译器上会报红

然后此时我们查看下我们的mybatis的映射配置文件，看看有哪些可以去除的，去除的部分就是已经在刚刚的配置文件里写入的部分

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0f7c49463fb8c370f8c74a851d480b9a.png)

显然，连接部分，以及读取配置文件的部分都可以去除掉，因此我们可以将我们代码省略为如下部分

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE84e14b95ea67198cf9bd54421a030a6f.png)

此时我们就知道我们接下来需要在spring中实现的部分，分别是上面的两个包的内容，只有实现了，才算是给这个文件进行了全部的搬家，才算是用spring实现了mybatis的案例。我们上面的两个内容，分别是类型别名和映射别名，前者用于指定我们要封装的对象的类型，后者用于指定我们要动态生成的类的对象的类型（也就是用于增删改查的接口的类），但是他们都是用包来实现的，我们只需要填入一个包的地址，其就能自动寻找到合适的类型，不需要我们去操心

那现在我们就来实现这两个内容，首先我们肯定需要spring中整合mybatis之后得到的对象，但是这个对象到底是哪个？这个我们还不知道，因此我们要去自己找，我们首先进入我们的资源图书馆，然后在其中我们能找到我们之前引入的spring整合mybatis的jar包，点开之后在点开里面的文件夹，能找到一个工厂类，一般来说，工厂类里往往会有创建我们所需对象的方法，因此我们进入工厂类中看看

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb9585d1bd02d3c15c8d05af896b9f83b.png)

我们查看其所有的set方法，看看里面有没有我们想要的东西，我们注意到我们之前的类名别名的标签是typeAliases，我们正好在这里能找到对应的方法，有两个，我们同时还注意到我们在配置文件里要填入的内容是包名，那么对于的setTypeAliasesPackage方法，就很有可能是我们所需要的方法。同时我们创建的对象一定要有一个连接，因此我们肯定还需要DataSource方法，在下面也有对应的方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4e1d56e8479fcfb35297e7a387b066b4.png)

根据上面的内容，我们可以再对应的配置文件中写入代码如下

```
<!--spring整合mybatis后控制的创建连接用的对象-->
<bean class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
    <property name="typeAliasesPackage" value="com.itheima.domain"/>
</bean>
```

可以看到我们这里代码自身是用于获得创建连接的工厂类对象的，其下我们给对应的两个变量赋值，分别是dataSource，也就是连接，dataSource对象我们在上面已经获得了，然后我们将其注入

接着我们给typeAliasesPackage指定路径，由于是指定路径，因此我们是填入value，同时我们指定对应的domain包，其指定的是我们的数据最后要封装的对象，那么到此为止，我们的第一个类名别名的标签就已经成功搬运过来了

然后我们接着去整第二个，第二个是映射扫描的配置，我们注意到在我们的spring整合mybatis的jar包中有mapper包，在其下有MapperScannerConfigurer（映射扫描配置），这显然就是我men所需要的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEcb30f420c46fd083138f9a5d8233f100.png)

接着同样我们进入其set方法中看看，我们可以看到一个setBasePackage的方法，设置基础包，正好我们的映射配置里要设置的也是一个包，那么这个方法，就是我们所需要的方法了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEdfe0155ed12941138dfde59258e212a0.png)

因此我们可以写入代码如下

```
<!--加载mybatis映射配置的扫描，将其作为spring的bean进行管理-->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="com.itheima.dao"/>
</bean>
```

我们这里创建了一个MapperScannerConfigurer对象，然后我们给里面的basePackage属性赋予了对应的包地址。这里我们提一下，我们的name的设置规则是将对应方法中的set去掉之后填入的名字，这是我们的Spring中的一个规范，因为实际上我们查看能够看到其成员变量的属性名就是我们根据规则得到的名字，开发中这也是一个规范来的，有了这个规范，就能够便于程序员的使用。同时，这也是为什么我们总是去寻找包中的set方法的原因，一是因为set方法有这个规范，我们可以知道其对应的成员变量，二是因为我们的Spirng的开发规范就是需要Set方法来提供给配置文件，这样配置文件才能给对面的属性注入对应数据。

我们搞定了这两个之后，我们的所有东西就搬家成功了，此时我们就不再需要原来的mybatis的映射文件了，我们创建一个App类并进行测试

```
import com.itheima.domain.Account;
import com.itheima.service.AccountService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App {
    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        AccountService accountService = (AccountService) ctx.getBean("accountService");
        Account ac = accountService.findById(1);
        System.out.println(ac);

//        Account account = new Account();
//        account.setName("Tom");
//        account.setMoney(123456.78);
//
//        accountService.save(account);

    }
}

```

测试之后会发现没有问题，我自己的代码搞出来会出一些奇怪得不行的问题，一直报出绑定错误，说是没有第十行的代码的方法，百度了很久也没有好的解决方法，那就先这样吧

最后我们再来看看我们放置在jdbc中的配置文件的代码

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://175.178.114.158:3306/spring_db
jdbc.username=root
jdbc.password=itheima
```

# 常用注解

我们学习完了spring整合mybatis，利用配置文件实现之后，接着就到了我们的传统艺能，使用注解来实现了，本章我们的主要内容就是来学习相应的注解

## 注解开发的作用与弊端

我们为什么要使用注解开发呢？第一点来说就是注解开发能够让我们的代码变得更加简洁，程序员写起来也更加舒服，因为我们的注解在我们要使用的时候直接往方法上面加对应的注解就完了。而配置文件开发，我们开发的时候肯定是要从java类和配置文件中切来切去的，就突出一个麻烦，所以我们推荐使用注解开发

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2df63c76335d997ea653af2d75c1f673.png)

在上图中就演示了对应的注解对应映射中的属性，@Component注解对应bean标签，要写入id属性就直接往里面填括号

而@Scope对于scope属性，写入属性同理

@PostConstruct和@PreDestroy两个注解分别对应bean中的初始化和摧毁方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEafc311cc70fd5547fbf6eeb4b6e6d140.png)

但是虽然话是这么说，但是用注解开发并不总是更好的，比如使用注解驱动无法在第三方开发的资源中进行编辑，最简单的例子就是我们注册驱动的代码，如果我们用注解的形式来写的话，那么我们就不得不手动进行一个获取对应的连接，虽然我们也很想通过注解直接完成这个动作的，但是注解是应用在java代码中的，而druid一类的连接池是第三方提供给我们的，因此我们无法去给里面的代码加上注解，因为其不允许我们修改

在现实的项目开发中，XML适用于配置第三方的资源，而注解适用于在第三方资源的基础上进行的开发。由于实际做项目我们肯定是开发的内容远远多于配置的，因此使用注解开发导致的这些问题是可以接受的

## bean定义常用注解（定义、作用范围、生命周期）

首先要启动注解功能我们首先要启动注解扫描，注解扫描的代码就如下图所示的一行，包名我就填写要扫描的包，这个包可以任意大，但是为了效率我们要尽量将这个包精确

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4e5872c03f231317bf276ba6e96da87c.png)

无论是注解格式还是XML配置格式，最终都是将资源加载IoC容器中，其差别在于数据读取方式不同，但是从加载效率而言注解优于XML配置文件

首先我们写入我们的applicationContext.xml的配置代码如下

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <!--启动注解驱动，指定对应扫描的路径，也就是资源所在的包-->
    <context:component-scan base-package="com.itheima"/>

    <!--<bean id="userService" class="com.itheima.service.impl.UserServiceImpl" />-->

    <!--<bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl" />-->

    <!--<bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl" />-->

</beans>
```

为了实验我们还准备了三个对应的配置的类，这里我们先注释掉了

在讲解注解类之前，我们先来对我们的项目整体进行一个说明，我们定义了BookDao和UserDao的实现类，实现类里就只是写了非常简单的打印语句而已，在UserService里也是同理，但是在这个类里我们还额外提供了初始化方法和摧毁方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE70522ea87dfb1753bd1cef514b3ccb0a.png)

如果我们想要注解来达到配置文件中bean标签的效果，那么我们要使用注解@Component，其实还有另外三个，其效果也是一样的，不过没什么用，我们还是用最开始的那个。其下有value属性，可以定义bean的访问id，当然我们不定义也可以

那么使用定义，我们可以将我们对应的我们的对应的类修改如下，首先是BookDao的实现类

```
@Component("bookDao")
public class BookDaoImpl implements BookDao {
    @Override
    public void save() {
        System.out.println("book dao running...");
    }
}
```

然后是UserDao的实现类

```
@Component("userDao")
public class UserDaoImpl implements UserDao {
    @Override
    public void save() {
        System.out.println("user dao running...");
    }
}
```

最后是UserService的实现类

```
//定义bean，后面添加bean的id
@Component("userService")
//设定bean的作用域
@Scope("singleton")
public class UserServiceImpl implements UserService {

    public void save() {
        System.out.println("user service running...");
    }

    //设定bean的生命周期
    @PostConstruct
    public void init(){
        System.out.println("user servier init...");
    }

    //设定bean的生命周期
    @PreDestroy
    public void destroy(){
        System.out.println("user servier destroy...");
    }
}
```

然后我们写入我们的测试类代码如下

```
import com.itheima.dao.BookDao;
import com.itheima.dao.UserDao;
import com.itheima.service.UserService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App {
    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");

        //UserService userService = (UserService) ctx.getBean("userService");
        UserService userService = (UserService) ctx.getBean("userService");
        userService.save();

        UserDao userDao = (UserDao) ctx.getBean("userDao");
        userDao.save();

        BookDao bookDao = (BookDao) ctx.getBean("bookDao");
        bookDao.save();

    }
}

```

实际运行之后我们发现我们能够得到我们想要的结果

这里我们值得一提的是@Socpe是设定bean的作用域的注解，简单来说就是设定我们创建的对象是不是单例的，后面的更多注解我们直接看下图中的说明就好了

首先是Scope

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE056539c29db091acce13fe22f7b1d498.png)

然后是生命周期相关的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEab89592afe64929b555bc0ea9d38cc4e.png)

## 注解配置第三方资源（工厂加载bean的形式）

接着我们来学习如何使用注解来配置第三方的资源，这里我们要使用到注解@Bean，使用注解Bean可以让我们的某个方法返回的对象被Spring管理，通过这个方式来替换配置文件中获取连接池对象的代码

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE59b86bc560d196e714f2eba7372e402c.png)

值得一提的是，由于我们的Bean所在的类必须在Spring扫描加载才能生效，因此我们还要在类上加入注解@Component，这个方法以后是要替换掉的，暂时我们先使用这种方式让我们的类能够被spring扫描到，那么我们可以写入代码如下

```
package com.itheima.config;

import com.alibaba.druid.pool.DruidDataSource;
import org.springframework.context.annotation.Bean;
import org.springframework.stereotype.Component;

@Component
public class JDBCConfig {
    @Bean("dataSource")
    public DruidDataSource getDataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName("com.mysql.jdbc.Driver");
        ds.setUrl("jdbc:mysql://175.178.114.158:3306/spring_db");
        ds.setPassword("itheima");
        return ds;
    }
}

```

在测试类中获取连接池对象并打印，能够得到对应的内容，这说明我们的注解开发是没有问题的

## 属性注入常用注解

我们先来讲讲关于非引用类型的使用注解进行属性注入的步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1b14fe0052a915cadc443642b90bdf21.png)

我们首先创建对应的int num属性和String version属性，接着提供对应的set方法，如果我们想要通过注解对其进行赋值的话，我们只需要在其上直接直接写入@Value注解并在括号中写入我们要赋予的值就可以了，同时这个注解也可以放在我们的属性对应的set方法上来完成属性的注入，当然，实际上我们可以将set方法省略，直接在对应属性上写入对应的注解并赋予值就可以了

```
@Value("3")
private int num;
@Value("itheima")
private String version;
```

有的同学会觉得这么做不脱裤子放屁吗？你直接用=的形式给他值不好吗？其实这里我们主要还是训练一个这一类注解的使用规范，后期我们这一类的注解还可以连接properties文件来进行数据的引入，而这种方式则是普通的赋值方式所难以代替的

那么非引用类型的属性注入学习完之后，接下来我们学习引用类型的属性注入

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE710100132d34bddb65b54dd23f8559b6.png)

首先我们在我们的UserServiceImpl中添加对应的引用变量UserDao和BookDao，如果我们希望注入我们所指定的实现类，那么只要直接在对应的属性上加上@Autowired注解就可以了，当然，这个前提是在我们对应的包里，只有一个对应的实现类，如果有两个及以上的话，那么其会根据其实现类中的@Component注解中括号里起的名字来定位，如果有括号内的取名和我们实现类中的取名是一样的，那么其就会自动加载该实现类。如果都不一样就报错

同时如果我们使用@Component但是不往括号内添加内容的话，也就是不取名的话，其会默认使用该实现类的类名来给自己取名，同时注意该注解不可以取两个一样的名字，否则会报异常

那我们如何来避免这个报错呢？我们有两种方式，一种是在我们对应的要使用的实现类中再加入@Primary注解，使用该注解会让我们的程序运行时优先使用这个实现类，但是这个只能定义一次，一旦使用该注解定义了两个及以上的相同实现类，那就报错。另外一种方式是使用@Qualifier注解，使用该注解往括号内添加指定的名字，那么当我们的程序搜索到两个及以上相同类时，就会使用该注解进行搜索，找到对应的名字的类并加载，同时该注解的优先级高于@Primary

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc84f16b8b67a04af9c85c24346516a86.png)

最后来看一些了解的内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe3d61a353117dfc046fac1acc91df22a.png)

## 加载properties文件中的属性

首先我们加载properties文件要使用的注解是@PropertySource，内部要写入我们要加载的properties文件的路径，我们可以写入多个配置文件令其加载，如果想要实现这个的话，就需要用大括号括住内部写入的路径，并用,分隔开，同时我们写入不存在的路径是会报错的，如果想要解决这个报错，可以用ignoreResourceNotFound属性，设置其为true，可以令其无视我们未找到的资源，但仍然会报出警告

当我们想要在内部进行赋值时，就要使用@Value注解进行注入

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf54368573073ae77b2bf16bd7f64db66.png)

那么我们可以写入其代码如下

```
package com.itheima.dao.impl;

import com.itheima.dao.BookDao;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Primary;
import org.springframework.context.annotation.PropertySource;
import org.springframework.stereotype.Component;

@Component
@Primary
@PropertySource(value = {"classpath:jdbc.properties","classpath:abc.properties"},ignoreResourceNotFound = true)
public class BookDaoImpl implements BookDao {
    @Value("${jdbc.userName}")
    private String userName;
    @Value("${jdbc.password}")
    private String password;

    @Override
    public void save() {
        System.out.println("book dao running...1"+userName+" "+password);
    }
}

```

jdbc文件的代码如下

```
jdbc.userName=root6564
jdbc.password=itheima
```

## 纯注解驱动制作

那么到现在为止，我们常用的注解类基本就学习完了，现在我们还缺最后一个，就是用注解来加载我们配置文件中的固定开头格式以及我们定义的要扫描的注解代码的类的位置的代码，将这两个代码都用注解来表示，我们就实现了纯注解开发了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd3af8b875a088afbb35b07e342307340.png)

那么我们要做的是就是用核心配合类来替换spring中的核心配置文件，此类是可以设置为空的，不设置任何变量和属性。因此我们创建一个新的config类，我们在其上先添加@Configuration注解，该注解就是代替核心配置文件中的固定开头，然后我们要设定properties的扫描路径，这里我们就使用ComponentScan注解，往括号内写入我要扫描的路径的包

```
package com.itheima.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan("com.itheima")
public class SpringConfig {
    
}

```

最后我们将我们的测试类修改如下

```
import com.itheima.config.SpringConfig;
import com.itheima.service.UserService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App {
    public static void main(String[] args) {
        //ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext-back.xml");

        ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);

        //UserService userService = (UserService) ctx.getBean("userService");
        UserService userService = (UserService) ctx.getBean("userService");
        userService.save();

/*        UserDao userDao = (UserDao) ctx.getBean("userDao");
        userDao.save();

        BookDao bookDao = (BookDao) ctx.getBean("bookDao");
        bookDao.save();

        DruidDataSource dataSource = (DruidDataSource) ctx.getBean("dataSource");
        System.out.println(dataSource);*/
    }
}

```

可以看到我们注解类里唯一的变动就是获得ApplicationContext对象时，不是采用配置文件获得，而是给其传入了我们对应的注解类的class对象的代码。而且使用的也是AnnotationConfigApplicationContext的对应构造方法。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8c53db17ed6652159f127cd01bf24b9d.png)

## 导入第三方资源对应的配置类

我们之前做过使用注解来创建我们生成连接池的内容，那时我们为了让我们的文件能够被Spring编译到往上加了一个@Conponent注解，但那是不标准的，我们又更好的方法，就是使用@Import注解，将其写在我们的Config注解文件上去，括号内传入我们要被spring控制的资源的class对象，其就可以将其正确加载了，以后如果我们要导入第三方的bean，也是采用这种方式的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE979d66e7c2aa96c3e8ee79737b7b2b5d.png)

那么我们可以写入我们的代码如下

```
package com.itheima.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

@Configuration
@ComponentScan("com.itheima")
@Import(JDBCConfig.class)
public class SpringConfig {

}

```

注意我们如果要导入多个第三方的包，我们直接往注解里加就可以了，用逗号分隔开，我们是不允许使用两个Import注解的

## bean加载控制

关于依赖加载，我们可以通过@DependsOn来控制我们的依赖加载的顺序，比如说，我们已知在我们的程序中，我们是Service调用Dao，而我们的Dao是比Service要先加载的，这是自然的，因为我们的Service要调用Dao，那你肯定是从里边开始生成的是吧。但实际上我们可以通过@DependsOn注解来让我们的Service先于我们的Dao加载，然后再执行我们的程序，如果要实现这个目的，我们就要往我们的Dao中添加这么一行代码

```
@DependsOn("userService")
```

这行代码可以写到我们的方法上或者是类上，其代表的意义是，该类或者是该方法的运行依赖于userService类，当我们的程序检测到这个注解的时候，其就知道该类依赖于另一个类，那么其就会查找到对应的类并先加载这个类。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0e942259ef87776d15177adb726089af.png)

然后是其实我们的配置类也可以通过对应的注解来定义我们的加载顺序，这里的情况是我们定义了多个核心注解配置文件的话，我们只要在对应的配置文件上加入@Order注解就可以了，其括号内填入数字，数字越高代表优先级越高，优先级高的配置文件会先加载

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8fb378b36cd464595d0b9fda64659e05.png)

最后是一个延迟加载的了解内容，具体看图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE501085a24befc426f5fca4a723778ecb.png)

接着我们来了解下具体的依赖加载的应用场景

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4329d5d6ba505ea45b2f5fb8dfbaf6d6.png)

首先似乎关于DependsOn，其应用场景的经典例子就是就是微信订阅，我们总是希望发布消息是慢于订阅消息的bean的加载的，这样才能保证接受的消息不会缺失，此时就需要我们的本注解，本注解的使用场景在于我们明确某一些类一定要先于另一些类加载时，我们就使用此注解

而@Lazy注解，也就是延迟加载的经典例子就是用于灾难处理，我们可以设置一个方案用于应急处理，但是如果我们一直将其创建出来放着就浪费内存，所以我们可以延迟加载该类，只有当我们的某些部分崩溃之后该类才会加载并执行

最后是@Order类，该类可以用于多个种类的配置文件，比如说我们可以将我们的注解配置文件进行分类，分为系统级和业务及的，我们总是令其先加载系统级的后加载业务及的，这样能避免细粒度的加载控制

# 整合第三方技术

接着我们已经学习完了常用注解了，接着我们就使用这些常用注解来将我们的第三方技术进行整合，使得我们的案例全部用注解形式来体现

## 注解整合mybatis分析

那么我们要做的事情就是注解整合MyBatis，我们的目标很简单，就是将Mybatis中的核心配置文件的内容全部用注解来表示，首先是加载配置文件的信息，这个我们通过@PropertySource注解来实现。我们判断我们是用Bean还是用Component，主要取决于我们看我们要生成的对象是自己的创建的，还是别人提供的，如果是别人提供的我们就是Bean，反之则用Component。并且前者内部是通过java代码的形式来实现的，其实都是这样的，别人的东西我们不能坐改动，因此往内部都是写入java代码的来赋值的，而不是用注解注入

相应的配置类我们则通过对应的注解来注入

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE00146c6593c85fc0c71a5ffc9f858039.png)

然后我们来看看步骤分析

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5be72bdad5c4f280d7d6e582675fc579.png)

最后是具体的步骤分析

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc4f318a8814a464458e604afcc767dea.png)

## 注解整合mybatis

接着我们进行一个注解的整合mybatis的动作，我们为此的做法非常简单，我们先打开我们的对应的MyBatis的核心配置文件，然后我们对着核心配置文件来做注解整合，一步步把里面的代码都搬家到我们的注解上去，首先是我们的固定格式的头代码和引入注解扫描区域的代码

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <!--加载perperties配置文件的信息-->
    <context:property-placeholder location="classpath:*.properties"/>
```

首先我们要创建一个对应的注解核心配置类，然后我们在该类上加入如下代码

```
@Configuration
@ComponentScan("com.itheima")
```

这两行代码，第一行代表的是头文件，而第二行代表的是要扫描的注解区域，我们这里写入相对比较精确的位置的包路径

接着我们要加载druid资源，我们加载druid资源的方式就是自己创建一个druid对象然后将其返回，同时，由于我们的路径一类的代码仍然是要放到properties文件中去的，因此我们在使用其代码创建对应的对象的时候，要将传入的参数用注解的形式从properties文件中获得

```
<!--加载druid资源-->
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="${jdbc.driver}"/>
    <property name="url" value="${jdbc.url}"/>
    <property name="username" value="${jdbc.username}"/>
    <property name="password" value="${jdbc.password}"/>
</bean>
```

那么首先我们创建一个jdbc的配置类，然后我们构建我们的代码如下

```
package com.itheima.config;

import com.alibaba.druid.pool.DruidDataSource;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;

import javax.sql.DataSource;

public class JDBCConfig {
    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String username;
    @Value("${jdbc.password}")
    private String password;

    @Bean("dataSource")
    public DataSource getDataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName(driver);
        ds.setUrl(url);
        ds.setUsername(username);
        ds.setPassword(password);
        return ds;
    }
}

```

可以看到我们这里为了和properties文件有所联立，多创建了四个成员变量，通过其进行一个赋值，这里由于我们使用的不是自己的代码，因此我们使用@Bean注解，同时我们的这个对象在后面会被人所使用到，因此我们这里给其取名dataSource，在配置文件中我们也可以看到后面还有内容引用了这个对象的id，因此我们这里也要加入id，最后我们再来看看properties的代码

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://175.178.114.158:3306/spring_db
jdbc.username=root
jdbc.password=itheima
```

最后不要忘了其配置是需要在核心配置文件中进行一个导入的

接着是我们的自己的代码，也就是我们需要创建出一个service层的调用的对象，这个比较简单

```
<!--配置service作为spring的bean,注入dao-->
<bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
    <property name="accountDao" ref="accountDao"/>
</bean>
```

在对面的实现类里加入注解然后取名就可以了，这里由于我们的AccountDao是引用变量，因此要加入Autowired注解，令其自动寻找对应的类

```
package com.itheima.service.impl;


import com.itheima.dao.AccountDao;
import com.itheima.domain.Account;
import com.itheima.service.AccountService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service("accountService")
public class AccountServiceImpl implements AccountService {

    @Autowired
    private AccountDao accountDao;

    public void save(Account account) {
        accountDao.save(account);
    }

    public void update(Account account){
        accountDao.update(account);
    }

    public void delete(Integer id) {
        accountDao.delete(id);
    }

    public Account findById(Integer id) {
        return accountDao.findById(id);
    }

    public List<Account> findAll() {
        return accountDao.findAll();
    }
}

```

接着是创建整合mybatis后控制创建连接的对象，也就是创建一个工厂对象

```
<!--spring整合mybatis后控制的创建连接用的对象-->
<bean class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
    <property name="typeAliasesPackage" value="com.itheima.domain"/>
</bean>
```

我们新创建一个MybatisConfig用于存放这些对象，然后我们首先创建一个返回SqlSessionFactoryBean对象的方法，加入@Bean注解，这里不用取名是因为我们只要有这个对象就可以了，不需要被人再次调用，因此括号内不用填入什么。然后既然在配置文件里其能创建对应的对象，那么其必然有无参构造方法，因此我们首先利用无参构造方法获得我们所需要的对象，然后对其进行对应的赋值，我们首先调用setTypeAliasesPackage方法，然后填入我们设置的包路径，该方法设置的路径里是有我们得到的数据要被封装的对象的。然后我们再调用setDataSource()方法，该方法需要传入一个数据源，这个数据源从哪里来捏？我们一个简单的想法就是从我们的方法中传入，同时我们给其加入@Autowired注解，令这个对象能够自动获得我们之前创建好的数据源对象。

```
@Bean
public SqlSessionFactoryBean getSqlSessionFactoryBean(@Autowired DataSource dataSource){
    SqlSessionFactoryBean ssfb = new SqlSessionFactoryBean();
    ssfb.setTypeAliasesPackage("com.itheima.domain");
    ssfb.setDataSource(dataSource);
    return ssfb;
}
```

最后我们来整最后一个内容，创建我们的映射扫描对象，这个其实是依葫芦画瓢了

```
<!--加载mybatis映射配置的扫描，将其作为spring的bean进行管理-->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="com.itheima.dao"/>
</bean>
```

那么我们可以写入其代码如下

```
@Bean
public MapperScannerConfigurer getMapperScannerConfigurer(){
    MapperScannerConfigurer msc = new MapperScannerConfigurer();
    msc.setBasePackage("com.itheima.dao");
    return msc;
}
```

最后我们将这个配置文件的代码用Import注解传入到注解核心配置文件中，最后我们的注解核心配置文件的代码如下

```
package com.itheima.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;
import org.springframework.context.annotation.PropertySource;

@Configuration
@ComponentScan("com.itheima")
@PropertySource("classpath:jdbc.properties")
@Import({JDBCConfig.class,MyBatisConfig.class})
public class SpringConfig {
}

```

最后我写入测试类的代码如下

```
import com.itheima.config.SpringConfig;
import com.itheima.domain.Account;
import com.itheima.service.AccountService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App {
    public static void main(String[] args) {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        AccountService accountService = (AccountService) ctx.getBean("accountService");
        Account ac = accountService.findById(2);
        System.out.println(ac);
    }
}

```

经过测试发现我们的代码没有问题，此时就说明我们已经成功整合了

## 注解整合Junit

我们上面已经将我们的Mybatis成功整合好了，但是我们的测试还是用的比较随意的一个自己创建的类，这样虽然能够成功，但是是不规范的，我们要使用Junit进行测试，因此我们本节来将我们的Junit也整合到我们的项目中且也用注解形式来进行整合

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb94231dc1607a8597d642b0585f15b81.png)

首先我们要知道，我们一般的Junit是自己整个类加载器然后把我们的类加载到里面进行测试的，而我们这里是要用spring整合Junit，因此我们要让Spring接管Junit的运行权，为此我们要使用Spring专用的Junit类加载器

这里要注意两件事，一是从spring5.0之后，就要求Junit的版本必须是4.12及以上，因此我们在写入依赖时要写入合适的版本，第二是Junit仅用于单元测试，我们不可以将Junit的测试类配置成spring中的bean，否则这个测试类也会被打包到工程中

下图是导入的Junit的坐标以及spring整合Junit测试用例的注解格式，我们看到我们这里配置了Junit的依赖，同时配置了spring-test的依赖，也就是spring技术的测试依赖

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf6fe344be9031f63cf7748aa24723535.png)

此时我们要创建对应的代码就不是在原来的地方上创建了，我们要在test包下创建对应的包并在其下写入对应的测试类代码，这是当然的，因为本来我们这个文件夹就是用于测试的，我们创建对应的测试类，然后写入代码如下

```
package com.itheima.service;

import com.itheima.config.SpringConfig;
import com.itheima.domain.Account;
import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import java.util.ArrayList;
import java.util.List;

//设定spring专用的类加载器
@RunWith(SpringJUnit4ClassRunner.class)
//设定加载的spring上下文对应的配置
@ContextConfiguration(classes = SpringConfig.class)
public class UserServiceTest {
    @Autowired
    private AccountService accountService;

    @Test
    public void testFindById(){
        Account ac = accountService.findById(2);
        System.out.println(ac);
        Assert.assertEquals("Jock1",ac.getName());
    }

    @Test
    public void testFindAll(){
        List<Account> list = accountService.findAll();
        Assert.assertEquals(2,list.size());
    }
}

```

我们首先在对应的类上写入@RunWith注解，该注解用于设定我们spring专用的类加载器，因此我们这里传入对应的类加载器的class对象，然后我们要设定加载的spring上下文的对应的配置，这里使用@ContextConfiguration注解，这里需要设定classes=，后面填入我们的核心配置文件的class对象。接着我们在我们的测试类里传入我们的需要进行测试的对象，上面写入注解@Autowired令其自动获得对应的对象

然后我们创建对应的方法并加上@Test注解，注意我们这里要进行判断例子能否通过不是用打印方法，我们是要使用断言语句，断言中填入我们与其的参数及其获得的实际数据，若断言正确，则在控制台中会显示通过，若不正确，则不通过，同时会在控制台中打印对应的不通过的信息

可以填入多个测试方法，然后一起运行，想要一起运行就直接运行对应的类就完了

