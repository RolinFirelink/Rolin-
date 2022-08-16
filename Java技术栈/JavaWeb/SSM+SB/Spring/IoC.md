本章节我们来学习loC，这是一个比较重要的内容，所以要认真学

- 控制翻转（loC）概念

在学习本章内容之前，我们先来做一个板书约定

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE0f86f35a998afa1cae97916ca034c376.png)

我们在学习Ioc之前，我们先来学习关于耦合和内聚

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE7fd6d9b02221b2db220df76e0c0f2fda.png)

所谓耦合指的是代码书写过程中所使用技术的结合紧密度，主要用于衡量软件中各个模块直接的结合程度。而内聚指的是代码书写过程中单个模块内部各组成部分的联系，用于衡量软件中各个功能模块的内部功能联系。我们程序员追求我们写出的项目以及代码是低耦合和高内聚的，也就是说，我们追求各个模块之间的联系要尽可能少，但是我们希望一个独立模块之间的内容要尽可能结合紧密，让一个模块只靠其自身就可以完成其自身的功能。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd598e7720d3da0719cd66d40b8727977.png)

接着我们来讲讲我们的工厂模式，我们最传统的调用资源的方式是应用程序与资源耦合，通过new对象的方式来达到调用我们的资源，这种方式违背了我们的低耦合的要求，因此我们往中间添加了一个工厂对象，由工厂来负责获得对象，此时就是工厂和资源耦合，还是违背了我们的低耦合要求，不但如此还显得脱裤子放屁多此一举。这时我们又引入了配置文件，将工程与配置文件结合在一起，我们的工厂通过配置文件来获取资源，此时是配置文件和资源高耦合，但是这个高耦合其实无所谓 ，因为在配置文件里进行修改不涉及到我们的源代码，我们修改之后将配置文件发送到服务器上就完了，不需要重新编译或者是发布我们的源代码。这时，我们的工程类和配置类，就组成了Spring的雏形，同时也是我们Spring中最核心的一个思想，IoC

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE9b9d18e5c6b37036eb4a97801c4bf1ae.png)

IoC，也就是控制反转，Spring反向控制应用程序所需要使用的外部资源，在传统模式中，我们的资源的主控权是在类的手中的，而在Spring中，其通过loC模式将主控权给到了Spring

在IoC模式中，我们需要资源时不再需要主动new对象，只需要通过loC容器提供给我们就可以了。IoC容器是Spring框架中自己设置好的容器，其可以将我们的资源提供给我们

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE193d5e300ae8d9dc1b720f3ccb4de553.png)

- 入门案例制作

既然我们现在已经学习了IoC模式，那么现在我们就来做一个入门案例来加深我们的理解。先来看看我们的案例需求

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb129fafa47abcbe9aeac6825b51fc905.png)

我们要做的案例是模拟三次架构中从表现层调用业务层的功能，这里我们的表现层就直接用主方法模拟了，正式一点的话是应该要使用junit来进行测试的，这里我们处于学习阶段，就随意一点吧

接下来让我们来看看步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE11e90d0daaf9d92974cbd0a574dc3b86.png)

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

- bean的基本配置

我们先来说说bean标签，首先有大的beans标签，然后其下有bean的小标签，bean下有三个属性要介绍，第一个是id，id属性是我们自己设定的唯一标识，我们这里推荐使用我们的实现类接口来统一命名，然后是class属性，该属性要填入实现类的路径，最后是name属性，该属性可以用于取别名，在这个属性中我们可以为我们的类取别名并使用，可以取多个，用,分隔开

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE00958b7db149dd212f138c0e82427823.png)

请看我们的配置代码

```
<!-- bean可以定义多个名称，使用name属性完成，中间使用,分割-->
    <bean id="userService" name="userService2,userService1" class="com.itheima.service.impl.UserServiceImpl"/>
```

- scope属性

本节我们来学习bean标签下的scope属性，其意为范围，可以指定bean的作用范围，其有三个可选择值，分别是singlet、prototype以及其他，其他的内容我们具体看图，我们这里主要讲singleton和prototype

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf69d6bd7ef82a49a1372e07d28a17c80.png)

singleton是我们默认的配置，如果我们不设定这个标签的内容，那么其默认就是singleton，其作用是指定我们的类是一个单例的对象，当我们指定了singleton时，我们的指定实现类的对象是在我们加载spring容器类的时候创建出来的，也就是执行ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");这个语句的时候，此后我们再进行任何创建对应类的动作，都是在重复获得最开始创建的对象。但如果我们设定的prototype，那么我们的对象就是非单例的，其能够正确创造出多个对象，且其对象是在new的时候才会产生的

但无论是那种方式，我们的对象都是保存在spring容器中的。他们两个本质区别在于创建的时机不同

- bean的生命周期控制

bean下有两个属性，分别控制器初始化和销毁，这两个属性是init-method和destroy-method，我们使用这两个方法之前要在具体的实现类里定义这两个方法，然后要在对应的属性下填入对应的方法名，这样当我们的对象初始化或者是销毁时对应的方法就会执行了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE32aa401181798f7ee608e701bdcd1cbf.png)

这里的注意事项是，我们的初始化和销毁的方法执行与我们的scope属性的设置息息相关，首先当我们的scope设置为singleton时，spring容器有且仅有一个对象，init方法在创建容器时仅执行一次，同时我们的对象放在我们的spring容器中，其会调用destroy方法（由于我们的程序关闭得太快，所以如果我们不主动先关闭spring容器，我们是无法在控制台上看到destroy方法的执行的）。而当我们的scope设置为prototype时时，spring容器如果创建同一类型的多个对象时，每创建一次init方法就执行一次，但是销毁时，其会由垃圾回收机制gc()控制，destroy方法将不会执行

- 静态工厂与实例工厂创建bean

bean对象的创建方式有两种，一种是静态工厂形式创建bean，一种是动态工厂的形式，这个内容我们做了解就可以了，因此会将的比较简略

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE96d392d2375b246d10578e3e8bf42894.png)

想用静态工厂创建bean，首先要创建出对应的静态工厂类，然后我们要在bean下加入属性class，和factory-method，前者填入工厂类的地址，后者填入工厂类的对应方法，同时工厂类中要有返回实现类的静态方法，然后我们在测试类中同样调用getBean方法就可以获得我们所需要的对象了

```
<!--静态工厂创建bean-->
<!--<bean id="userService4" name="userService2,userService1" class="com.itheima.service.UserServiceFactory" factory-method="getService"/>-->
```

然后是实例工厂创建bean，实例工厂创建对应的bean，首先要创建对应的工厂类，类中要写入获得实现类的对象方法，但是我们这里不填入static关键词，然后我们在配置文件中先写入实例工厂对应的Bean，提供id，class写对应地址，再写入bean，这个bean同样提供id，这个id是我们测试类里用的，而我们这里的factory-bean则填入我们第一个bean标签的id，方法则是我们定义好的getService方法，想想也知道是我们这样配置，其会自动帮我们完成创建对应的对象并返回

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE831e5a19b23a790aead5e5dcfde1a4ac.png)

最后来看看配置代码

```
<!--实例工厂对应的bean-->
<bean id="factoryBean" class="com.itheima.service.UserServiceFactory2"/>
<!--实例工厂创建bean，依赖工厂对象对应的bean-->
<bean id="userService" factory-bean="factoryBean" factory-method="getService"/>
```

- 依赖注入概念（DI）

我们之前学习过IoC，其为控制翻转，在IoC模式中，应用程序所需要的资源由IoC容器提供。而DI其实和IoC是一个意思，只不过是另外一种说法，DI为依赖注入，指的是应用程序运行所依赖的资源由Spring为其提供，资源进入应用程序的方式称为注入

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE1049128b0fe7eebf757ee47972eda460.png)

- set注入

介绍完了注入之后，现在我们正式来学习注入的方式，注入的方式有很多种，我们先来学习最主流的那种，也就是set注入

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE015e0c0891369b1c92bb54f816013329.png)

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

- 构造方法注入

本节内容只是作为了解，我们稍微看看就行了，因为我们只是偶尔使用这个，甚至可能是不使用

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE58eda2438b8e1beeaaf482ac3c0a94e0.png)

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE3d0e403d06b14a46282b751bcf7b3215.png)

- 集合注入

接着我们来学习集合类型数据的注入，我们有五种集合类型，分别是array、list、set、map、props。其都使用property或constructor-arg标签进行集合数据类型的注入

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2278ae82597e898db16cd5fe9887e8e9.png)

首先来看看list集合的注入方式，我们注入list集合的数据的方式是通过property标签，其下有属性可以指定其名字

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE40e225cd1275477f656139ce8320eb45.png)

然后是props

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb9193ce524c24804a0b6c6445cff18b3.png)

接着是array

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE80e5ba8f3e6811bd7a26951675e24f63.png)

然后是set

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE7af985e466236d20414744cffa45e35f.png)

最后是map

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE02cc2a518bfbcb1db10eb08982eb858a.png)

这里list和prop是比较重要的，我们会经常用到的，其他的我们稍微看看就差不多得了

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

- 使用p命名空间简化配置

这个内容我们主要作为了解，使用p命名空间简化配置简单来说就是我们可以将一个长的命名用一个字母来进行代替，这听起来很不错，但实际使用的效果却并没有那么好，因此本节作为了解内容。要使用p命名空间，我们需要写入下图中划上横线的语句

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE03a1f318ddc4a42d8bf0b8a77b1b0f5a.png)

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

- SpEL

这块内容的作用就是我们输入要注入的元素可以统一使用value进行赋值，这是其唯一的好处，当然，爱用不用，无所谓其实

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5f647d7f98e2685d7187b5f207b8846c.png)

我们来看看其对应的各种使用方式，具体请看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE81205cc0d78d9a028f779061debd0d1c.png)

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

- 读取properties文件信息

我们的一些基本信息都是要求写在properties文件中的，本节我们就来学习将数据写在properties中，并且将其读取到我们的代码中

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEaf9568778a5da09b47f76c4982df3bf1.png)

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

- import导入配置文件

如果我们的配置文件全部都写在一个xml文件中，那么我们的配置文件就会非常乱，以后维护起来就很难，因此我们要尽量保持我们的配置文件的有序性，此时我们的imoprt就起作用了，我们可以创建好几个xml文件，然后每一个xml文件负责放置一种类型的配置代码，然后主xml文件里就只用将这些子xml文件导入就可以了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc48756db54d24ffdb5c4d06377c9bf80.png)

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE03538cb7a9216e58768fc412301286af.png)

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE7914953def6280570495b6427c2594f3.png)

举个例子，假设我们现在创建了两个book的xml文件，并且我们在主xml文件中写入代码如下，那么book2.xml文件的内容才会是最终我们对象被赋值的内容

```
<import resource="applicationContext-user.xml"/>
<import resource="applicationContext-book.xml"/>
<import resource="applicationContext-book2.xml"/>
```

这个了解下就行了，因为实际开发中你只要遵守规范，根本不可能出现这种问题

- ApplicationContext对象层次结构

这个内容属于了解内容，自己看看就差不多得了，具体信息看下图吧

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE9a9f9170c4bc084bf155251d6cd12229.png)

- 第三方bean的配置方式

我们之前学习的都是配置我们自己的创建的对象，那如果我们想要配置第三方的对象呢，那我们应该要怎么办呢？比方说我们这里希望我们配置我们阿里云的数据源方案Druid，那我们应该怎么办？当然，首先就要在maven中导入其对应的依赖是吧，没有依赖我们根本用不了

然后我们就要考虑使用什么方式来配置我们的Druid，我们知道一般的配置注入方式有两种，分别是构造器方式和set注入方式，我们可以查看其内部的构造方法和set方法，找到适合我们用于创建对象的方法来创建对象，比如说在Druid中，我们发现去构造方法少，但是set方法是真的多，比如说通过密码，通过url等等，都可以创建，那么我们可以随便选，当然，如果我们实在搞不懂的话，可以点入源码去看看

那么最后我们可以构造我们的配置代码如下

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6d4373b2923a890af4f8cec50082c8ce.png)

可以看到我们这里指定一个对象，然后往里面注入创建该对象所需要的值，这样我们就可以通过配置文件来创建出我们所需要的对象了。