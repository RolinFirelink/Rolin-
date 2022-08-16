本章节我们来讲解一个重量级内容，就是AOP，我们之前学习Spring的时候也介绍过AOP了，其是我们的Spring的核心功能，因此我们要重点学习，同时本节偏理论

- AOP概念及其作用

在我们正式讲解AOP之前，我们先来讲讲OOP，我们以前OOP开发我们的项目时，我们总是先看准我们的类然后实现其三层的接口，最后实现我们的整个功能，换言之，我们OOP开发关注的是层级

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6cb50d29791a4eee18a77870a90fd38e.png)

我们OOP的开发总是会充斥着许多重复代码，尽管我们可以创建一个工具类将重复的代码抽取出来，但是这种抽取只能抽取一部分，而且还有许多代码压根抽不出来。此时我们的AOP功能就出场了，AOP简而言之是将相同的代码做成一个共性功能，当我们编写代码时，可以忽略这些共性功能的代码，而当这些代码真正执行时，其会自动将这些共性功能加入进去，这样就可以让我们的代码变得简洁，同时提高我们代码的复用性

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE65d3310bcaac2f21cce7c518634b111e.png)

接下来我们正式来介绍下AOP的概念，AOP全称是Aspect Oriented Programing，意为面向切面编程，是一种编程范式，也就是一种编程规范。其弥补了OOP的不足

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa520baa0979cb52228eeb0181d6de619.png)

AOP可以将代码转化为共性功能，从而完成业务的开发，其唯一的不足在于，由于各行各业的开发规范并不是完全统一的，因此AOP的使用总是不那么顺利，如果两个项目的开发并不完全相同，那么结合时就会出现种种问题，当哪天我们的开发规范全部都一样的时候，我们就可以完全使用AOP实现半自动化甚至全自动化的开发了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6d020c7d4ce6d63a605dd9cf843b8288.png)

最后我们来看看AOP的好处

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd7aa2f1af018e4cd31b990e1c542bc88.png)

- AOP核心概念

接着我们来学习AOP的相关的核心概念，然后我们来做一个案例去加深我们对AOP的理解

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE257924db4b0f82e2a9167375a44f1559.png)

首先是连接点，我们平常开发时的所有方法都是连接点

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc9f94ad3980219811474e71f15c77eb3.png)

然后我们开发的方法里，如果内部有共性功能，那么该方法就叫做切入点，共性功能本身被称为通知，由于通知本身也是存在差异的，因此不同的通知之间又分有不同的通知类型。切入点和通知之间，也就是被提取出代码之后留白部分，我们称之为切面。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc135887cf871d8f1fd1366521e817aa1.png)

在具体运行时的原始的对象我们称之为是目标对象，原始的目标对象不能够运行出结果，将通知放入切面的过程中我们称之为植入，植入的位置其实是通过原始的目标对象新创建处一个类，然后运行这个类得到的结果，这个新创建对象的过程我们称之为代理，同时这个代理还支持添加一些不存在目标对象中的新功能，我们称之为引入

最后我们来看一个总结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEafcdb1fa55819a6d541545756aeab7f0.png)

接着我们就来正式去实现这个案例，实现之前，我们先对这个案例进行分析，因为并不是所有的功能都需要我们去手动完成的，比如说像连接点，我们肯定要做，但这并不属于AOP的内容，实际上我们要做的又属于是AOP的内容就只有三个，分别是切入点、通知和切面

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE0f184bff1933281f00460523f18c38c2.png)

- 案例制作

那么接着我们就来正式做一个AOP使用的案例，首先我们配置了springframework的依赖，这里有一点值得一提的是我们一配置该依赖，其实对于的AOP依赖就在里面了，已经给我们加上了

```
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context</artifactId>
  <version>5.1.9.RELEASE</version>
</dependency>
```

我们AOP开发有三种方式，分别是xml方式、xml结合注解方式以及注解方式。我们因为是初学，为了简单我们先采取纯xml的方式来进行AOP开发

先来看看我们的步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc05cbe53ce6a635b0962adaed1f71b8a.png)

首先我们进入一个坐标的导，这没什么值得特别说的

```
<dependency>
  <groupId>org.aspectj</groupId>
  <artifactId>aspectjweaver</artifactId>
  <version>1.9.4</version>
</dependency>
```

首先我们要确定我们要抽取的功能，然后我们创建一个专门的类将其制作成方法将代码保存进去，然后删除原始业务中的功能，那么我们可以制作这么一个AOP类并写入代码如下

```
package com.itheima.aop;
//1.制作通知类，在类中定义一个方法用于完成共性功能
public class AOPAdvice {

    public void function(){
        System.out.println("共性功能");
    }

}

```

然后我们就要在配置文件中具体进行我们的操作，首先我们配置我们的共性功能使其成为spring控制的资源，看第17行代码，我们这里采用最简单bean标签进行配置，然后我们开启AOP的命名空间，这里开启命名空间是1-12行的内容，不是第15行的内容，同样开启对应的命名空间需要三行。

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--3.开启AOP命名空间-->
    <bean id="userService" class="com.itheima.service.impl.UserServiceImpl"/>
    <!--2.配置共性功能成为spring控制的资源-->
    <bean id="myAdvice" class="com.itheima.aop.AOPAdvice"/>

    <!--4.配置AOP-->
    <aop:config>
        <!--5.配置切入点-->
        < id="pt" expression="execution(* *..*(..))"/>
        <!--6.配置切面(切入点与通知的关系)-->
        <aop:aspect ref="myAdvice">
            <!--7.配置具体的切入点对应通知中的那个操作方法-->
            <aop:before method="function" pointcut-ref="pt"/>
        </aop:aspect>
    </aop:config>
</beans>
```

然后我们就正式类配置我们的AOP，调用<aop:config>标签，首先要配置切入点，切入点的配置依赖aop:pointcut标签，内部给切入点取名，后面的属性我们暂时按下不表。接着我们要配置切面，也就是要配置切入点和通知的关系，配置切面用aop:aspect标签，我们首先在内部确定我们要植入的通知，所以所以在ref属性中填入我们之前配置的共性资源的id，然后我们在内部要配置具体关系，我们采用aop标签，选择before，表示我们的共性功能是在我们的实际业务之前添加的，接着我们要指定我们要添加具体的方法，也就是要填入的共性代码，，最后我们要填入我们具体要填入的代码是在哪个切入点，因此我们这里填入pt，也就是我们之前配置的切入点

最后我们经过实际测试我们会发现我们的功能可以正常实现，那么此时我们的案例就完成了

- AOP基本配置

接着我们来具体说说我们之前配置里的内容，首先是Aspect，其意为切面，用于描述切入点与通知间的关系，是AOP编程中的一个概念。而Aspectj则是Aspect的基于java语言的实现

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe0830d229b1074131b9d77b0ae97480a.png)

学习完了这个我们就可以解释我们之前引入的坐标是什么了，可以看到我们这里引入的坐标依赖，其实就是aspectj，也就是java语言对Aspect的实验

```
<dependency>
  <groupId>org.aspectj</groupId>
  <artifactId>aspectjweaver</artifactId>
  <version>1.9.4</version>
</dependency>
```

首先我们来说说aop:config标签，该标签可以用于设置AOP，也就是设置具体的共性功能的加入位置，这里值得一提的是，一个beans标签中可以配置多个aop:config标签，并且他们都能够起作用

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE114d416c4f0cfbc74b7c613b7ab27433.png)

然后我们来讲上一个标签下的子标签，aop:aspect标签，该标签用于设置具体的AOP通知对应的切入点，可以让AOP通知正确地加到我们的切入点上。同样可以设置多个相同的标签，并且他们都会生效

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE512664012555118598eeef4afd513cae.png)

自最后我们来讲aop:pointcut标签，该标签用于设置具体的切入点，其下有两个属性，一个是id，另一个是expression，前者是切入点的唯一标识，后者是对应的切入点表达式。我们可以在一个aop:config标签中配置多个pointcut标签，同时每一个point标签都可以被正确使用，注意这里是正确使用，而不是同时生效

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa717bcb95d87b5bfd5da7eb0d5398f75.png)

- 切入点表达式

本节我们来讲解我们的切入点表达式，首先切入点表达式是一种快如匹配方法描述的通配格式，其类似于正则表达式

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEec06fc3956a60c0b6782ce595a4fe916.png)

 其表达式组成为关键字 (访问修饰符 返回值 包名.类名.方法名 (参数) 异常名)。其中关键字描述表达式的匹配模式，其有多种，但是一般都是使用execution，访问修饰符描述的是方法的访问控制权限修饰符，其实就是public那些，而且public可是省略，其中异常名是可以不写的，其他的都一定要写

接着我们来讲切入点表达式的通配符，通配符有三种，分别是*，..和+，其中*是可以单个独立任何符号，也可以独立出现，也可以作为前缀或者后缀的匹配符出现，其意义为一个任意符号，用法非常多，后面我们会慢慢介绍

第二个..，其可以独立出现，常用于简化包名和参数的书写，其意义为多个连续任意符号

最后是+符号，该符号只能用于匹配子类类型，也就是说，使用了这个符号那么就只有继承了某个父类或者是实现了某个接口的类才能够被识别

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE4aaffc2054309a94e7f51f93a8c0ffed.png)

我们就以上图中的三个例子为例，我们的第一个例子中，我们能够识别的类是public的范围的，返回值不限的，且在包com.itheima下的任意一个包下的UserService类中的以find开头结尾任意的方法，传入参数有一个，类型不限定

第二个例子中识别的类是public范围的，返回类型为User的在com包下的任意个包下的UserService类中的findByid下的，传入参数的多少和类型都不限定的方法

第三个例子将public省略，识别返回类型不限，在任意包下都可以的以Service结尾的类同时其必须是某一个类的子类，最后方法名不限定，传入的参数的数量和类型都不限定的方法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE234237c566a5eb49eb0d3d932126c163.png)

切入点表达式还有对应的逻辑运算符，也就是&&、||、和!，&&表达的是连接两个切入点表达式，如果这两个表达式都成立那么就匹配，第二个是任意一个匹配就匹配成功，最后一个是一旦不匹配某一个我们指定的格式，那么其对于我们的而言就是要植入的类。这个其实了解下就好了，不是很重要

值得一提的是，我们最后填写的类不但可以是实现类，还可以是接口，当然能这么做的前提是我们的测试类里写的类型也是接口，不然就寄。

- 三种切入点的配置方式

接着我们来说说切入点的三种配置方式，第一种配置方式是配置公共切入点，其配置在aop:config标签下，其配置要给予id和expression属性，公共切入点的配置的好处在于，一旦配置了一个公共切入点，后面的切点引入都可以使用同一个公共切入点。

第二种配置方式是配置局部切入点，其配置方式和配置公共切入点是一样的，其要配置在aop:aspect下，配置局部切入点是其配置只能够在这一个局部使用

第三种配置方式是直接配置切入点，其直接配置在aop:before等标签内，利用pointcut属性，内部填入我们的切点表达式，这种配置方式只能自己用

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa0f4df09faef377d1ab5be9251dbbdd9.png)

最后我们来讲下切入点的配置经验，我们企业开发要使用AOP，一定要严格遵循规范文档进行开发，否则就寄了，压根玩不转这个。我们开发时一般都是先配置局部切入点，配置完之后再抽取其中的公共切入点，最后再抽取全局切入点，以此来实现我们代码的复用。最后我们在代码走查，也就是自查过程中，我们要看我们的切入点是否存在越界性包含，也就是不该包含的，我们给包含了，第二个是要检查我们应该包含的是不是没包含到，也就是是否存在非包含性进驻，最后我们要设定AOP执行检测程序，在单元测试监控通知被执行次数与预计次数是否匹配（不过这个测试只是一个测试程序，不能保证完全准确），最后设定完毕的切入点如果发生调整需要进行回归测试，以上规则都是使用于XML配置格式的，当然有一些内容注解中也是适用的

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf2f40ffefc15b402e5020b6911307bf6.png)

- 五种通知类型配置

接着我们就来讲讲我们的AOP的通知类型，我们之前一直使用的类型都是前置通知类型，也就是在方法执行前，先将通知执行，实际上，AOP的通知类型共有五种，分别是前置通知、后置通知、返回后通知、抛出异常后通知以及环绕通知。具体的不同请看图，接下来我们将对这五种通知进行逐一的讲解

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEee7efefc2c9a873effccd05ac8b66704.png)

首先是前置通知，前置通知是可以配置通过的，其会在原始方法执行前执行，如果在通知过程中抛出异常，那么会阻止原始方法的执行

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE7319e98d00d03feea69dbfb3ee6bbc6d.png)

然后是后置通知，其会在原始方法执行后执行，无论原始方法执行时是否抛出异常，后置通知都会执行

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEbbffe58aed007f1202453535147913bf.png)

接着是返回后通知，其也是在原始方法执行后再执行，但是不同的是，如果原始方法中出现了异常，那么该方法就不会执行

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6a9742356f7d84393d056d128ff3c6cc.png)

然后是抛出异常后通知，这个通知的不同之处在于，如果原始代码中没有抛出异常，那么其就不执行

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf1103a765a3b562eacdad902650d7e63.png)

最后是环绕通知

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE21693f92fdc72c693e79cb1948f28e69.png)

这里我们需要重点讲一下的通知是环绕通知，其是这五种通知中兼容性最强的通知，通过特殊的处理，可以让最后一个通知实现前四个通知的作用。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE18eb76127966d435a83f44648297e544.png)

其调用方式也比较特殊，其需要在其方法中传入ProceedingJoinPoint，来执行原始对象的方法，同时该方法可能有异常，因此要对其进行相应的处理。同时返回的类型只能是Object或者是void，该方法的上下面就代表了我们要执行的方法的上面下面，因为我们的around方法本身就是代表我们的通知是方法执行前要执行的，执行后也要执行的，这里这样设置是能够让我们自己去设置了。如果没有调用对应的ProceedingJoinPoint的方法，则代表原始代码的执行被阻拦

那么我们可以设置我们对应的共性功能的代码如下

```
package com.itheima.aop;

import org.aspectj.lang.ProceedingJoinPoint;

//1.制作通知类，在类中定义一个方法用于完成共性功能
public class AOPAdvice {

    public void before(){
        System.out.println("before");
    }

    public void after(){
        System.out.println("after");
    }

    public void afterReturning(){
        System.out.println("afterReturning");
    }

    public void afterThrowing(){
        System.out.println("afterThrowing");
    }

    public void around(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("around before");
        pjp.proceed();
        System.out.println("around after");
    }

}

```

然后我们可以构造这五个通知的调用代码如下

```
<aop:config>
    <aop:pointcut id="pt" expression="execution(* *..*(..))"/>
    <aop:aspect ref="myAdvice">
        <!-- 前置通知 <aop:before method="before" pointcut-ref="pt"/>-->
        <!-- 后置通知 <aop:after method="after" pointcut-ref="pt"/>-->
        <!-- 返回后通知 <aop:after-returning method="afterReturning" pointcut-ref="pt"/>-->
        <!-- 抛出异常后通知 <aop:after-throwing method="afterThrowing" pointcut-ref="pt"/>-->
         <!--环绕通知--> <aop:around method="around" pointcut-ref="pt"/>
    </aop:aspect>
</aop:config>
```

