学习完了SpringMVC之后，现在我们正式进入SpringBoot的学习，我们要加快进度了，因为我们很快就要到我们的项目制作中去了

- 课程导学

我们先来看看为什么我们要学习SpringBoot

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE277d611a5c942f5679269092f0396669.png)

我们目前暂时先学基础篇，赶鸭子上架就赶鸭子上架吧，学了再说，同样的按照惯例我们先做一个入门案例来帮助我们的学习

- SpringBoot入门案例之idea联网版

首先我们要知道我们的SpringBoot的作用，SpringBoot的设计目的是用来简化Spring应用的初始搭建及开发过程。这个框架的主要作用就是用于简化另一个框架

那么我们接着来做我们的案例，首先我们创建一个空工程，然后选择导入模块，我们这里要做SpringBoot的工程，然后我们选择模块导入，创建一个新模块，然后选择Spring Initializr，我们这里只要设置我们的JDK为1.8就完了，然后点击next，同样选择Java version为8，最后的Package改为com.itheima（其实我认为不改也行）

进去之后我们可以看到其会进入一个窗口让我们选择对应的选项，上面的下拉框有选择Spring Boot的版本的，当前的稳定版是2.5.4，我们一般也默认选择这个，不做改动

然后我们选择左侧的Web，勾选Spring Web，点击Next和OK，其就会帮我们创建完毕了。其创建完毕的项目里帮我们提前导入了我们所需要的依赖，不过下载还得我们手动下载，我这里光引入依赖就花了15分钟，属实慢。然后我们在com.itheima下创建一个Controller包，然后创建一个类并写入代码如下

```
package com.itheima.controller;

//Rest模式

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/books")
public class BookController {

    @GetMapping
    public String getById(){
        System.out.println("springboot is running");
        return "springboot is running";
    }
}

```

可以看到我们上面就是做了简单的映射而已，如果是在SpringMVC中，我们还要做一大堆的配置才能完成案例，但是我们这里什么都不需要，甚至连配置服务器都不需要，我们只需要直接运行其给我们生成的对应的类，然后直接运行就完了

我们输入网址http://localhost:8080/books，可以发现我们的确进入了对应的字符串返回的页面

最后我们来看看本节的总结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5c82796e5cd28327dada33720773b2dc.png)

这里唯一的痛点就在于，这个工程是一定要联网的，实际上，工程最开始创建的时候，其就需要指定一个给其连接某个网页的地址，那是不是如果没有网络我们就搞不了SB了呢？当然不是，接下来我们做一个不用联网的案例。

- SpringBoot案例之官网创建版

我们可以去SpringBoot的官网创建对应的项目，进入对应的SpringBoot页面就可以了，其会生成一个压缩包，我们可以利用这个压缩包导入到我们的工程中，然后该模块就有和我们之前一样的功能了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa6ccc01ed6fc0182db5d722f68adc261.png)

说实话我个人感觉这未免有点脱裤子放屁，我能联网了我干嘛不直接用idea呢

- SpringBoot入门案例之阿里云版

本节我们来学习SpringBoot的阿里云版本，其实就是在创建对应的SB项目时把要访问的网址改成阿里云的，其地址是http://start.aliyun.com

这里值得一提的是，阿里云默认提供的版本比较低，因此我们可以在pom.xml文件中进行一个版本的改，直接改成我们所需要的版本就可以了

最后来看下总结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE69c3000e0cc1281129adf388f9e6dd5c.png)

- SpringBoot案例之手工制作版

我们之前学过的案例，无一例外都要联网，而本次，我们来学习真正的不用联网的SB案例。当然，有一点我们要注意，我们这里说的不联网指的是我们的项目创建不联网，如果一开始你的项目连最基本的依赖都没有，那肯定还是寄的

首先我们创建一个Maven工程，进入到我们的pom.xml文件中，写入其代码如下

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.6.7</version>
    </parent>

    <groupId>org.example</groupId>
    <artifactId>springboot_01_04_quickstart</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

</project>
```

我们这里所做的事就是令其继承其父类，并且引入了一个依赖，还有其他的依赖或者是插件暂时我们这个案例里用不上，因此就没有引入了。最后我们同样写入对应的测试程序会发现我们的模块的确是可以运用的，此时就说明我们的案例已经成功了

- idea中隐藏文件或文件夹

创建SB带来的附带文件太多，我们可以进入setting中自己去设置因此掉对应的文件夹，具体的设置规则其实和之前的差不多

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE48c3aa5cd9fd71fdbab9706c9d350a6d.png)

- 入门案例解析之parent

接着我们就来正式解析我们的项目了，我们先来解析parent，我们先来看看我们的SB的好处

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8123afe3813f5afa52288d71c164e50c.png)

然后我们来说下我们的Parent标检下的代码的作用，我们开发的时候，总是会遇到两个模块用同一个依赖的时候，这时候我们一般只能无脑引入两次，这有点麻烦，而且最重要的是，容易出现包冲突的情况，此时我们不得不进行排除依赖等操作进行一个调包，那是一个突出痛苦啊，一直整都整不好。但是SB里面的Parent标签就帮我们解决了这个问题，其继承的依赖中事先引入了许多的不同版本的依赖，就相当于是规定了我们引入对应的依赖的时候，就指定使用这个版本，因为他知道使用这个版本最好，就相当于是帮我们做了一个版本的统一，这样我们就不用再考虑那瘠薄的版本冲突了。

其利用版本的时候是用到了maven高级的一些内容的，有出现类似于EL表达式的内容，我们这里赶工，就暂时先将这一部分的内容按下不表。

最后我们值得一提的是，我们从springboot官网中创建的对应的项目文件中，其引入对应的类的方式是继承，也就是使用parent标签，而在阿里云中，则不是使用继承，而是直接将这个继承的类当成一个依赖进行引入，后者可以引入多个，而前者只能继承一次，这是他们的分别

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE17651759546bdc2099b37fba6edeced7.png)

- 入门案例解析之starter

还有一个问题，我们的parent标签下引入的依赖，似乎只要我们不引入对应的其他依赖，其规定好的版本就不会发挥作用，难道他的作用就仅限于此吗？当然不是，我们注意看我们的pom文件中的引入的两个依赖，里面都有starter关键字，其实里引入的这两个依赖，里面引入了其他的依赖，也就是依赖传递，通过这个依赖传递最终我们只需要通过引入两个依赖就可以实现引入多个依赖，而这些依赖的版本，就是要靠我们的parent里面引入的最开始的依赖，这个依赖来确定我们引入的其他的依赖的版本号

```
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

最后我们要注意的是，一般来说，我们引入的依赖是不需要指定版本的，但是有一些对应的依赖，在SpringBoot中是没有配置的，此时我们就不得不进行一个手动的版本的引入了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE48f5d28ea74649dc0cab152e5c8133cc.png)

最后要注意我们自己需要的是引入坐标的是注意版本，别搞了两个模块出现版本冲突了

- 入门案例解析之引导类

那么我们现在已经介绍完了依赖了，那么我们的工程是由谁来运行的呢？答案是靠我们的引导类，那什么是引导类呢？简单来说就是添加了SpringBootApplication注解的类，我们点进去这个注解看看源码

```
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by FernFlower decompiler)
//

package org.springframework.boot.autoconfigure;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Inherited;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import org.springframework.beans.factory.support.BeanNameGenerator;
import org.springframework.boot.SpringBootConfiguration;
import org.springframework.boot.context.TypeExcludeFilter;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.FilterType;
import org.springframework.context.annotation.ComponentScan.Filter;
import org.springframework.core.annotation.AliasFor;

@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
    @AliasFor(
        annotation = EnableAutoConfiguration.class
    )
    Class<?>[] exclude() default {};

    @AliasFor(
        annotation = EnableAutoConfiguration.class
    )
    String[] excludeName() default {};

    @AliasFor(
        annotation = ComponentScan.class,
        attribute = "basePackages"
    )
    String[] scanBasePackages() default {};

    @AliasFor(
        annotation = ComponentScan.class,
        attribute = "basePackageClasses"
    )
    Class<?>[] scanBasePackageClasses() default {};

    @AliasFor(
        annotation = ComponentScan.class,
        attribute = "nameGenerator"
    )
    Class<? extends BeanNameGenerator> nameGenerator() default BeanNameGenerator.class;

    @AliasFor(
        annotation = Configuration.class
    )
    boolean proxyBeanMethods() default true;
}

```

可以看到里面有很多的其他注解，在里面也可以找到我们的构造注解核心配置文件时的注解，包括扫描包一类的注解，简单来说就是以前的注解那么多，现在我们都可以用一个注解来代替了，方便得很。

```
package com.itheima;

import com.itheima.controller.BookController;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;

@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext run = SpringApplication.run(DemoApplication.class, args);
        BookController bean = run.getBean(BookController.class);
        System.out.println(bean);
    }
}
```

而我们的SpringApplication.run就相当于是我们之前在SpringMVC中运行容器的主方法，运行该方法可以获得一个容器对象，这个容器对象同样也可以获得里面所存储的对象

- 入门案例解析之辅助功能

接着我们存在的一个问题就是，我们的Tomcat服务器是怎么启动的？其实我们的Tomcat的服务器是被引入到我们的依赖中了，其是作为一种内嵌服务器来运行的，其引入的依赖的代码是在我们的starter引入依赖中一并引入的

如果我们将对应的Tomcat依赖给排除掉的话，我们的程序仍然能正常运行，不过这次就会自动停止了，因为压根就没有服务器，当然运行完就停了。我们也可以在排除了对应的依赖之后再引入其他的服务器的依赖，这样就可以使用其他的内嵌服务器了

最后我们可以来做一个总结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd5c9bedd51f56e4b94b622dc968f07a2.png)

- 复制模块

接着我们来学习我们的属性配置方式，在学习之前，我们要先来学习一个复制模块的技巧，因为学习的时候避免不了要创建多个项目，每次都重新创建一次就太麻烦了，所以我们来学习下复制模块的技巧

我们选择第二个模块，因为这个模块我们没做什么改动，我们首先将这个模块复制一个副本，然后给其改个名字，就改名为springboot_01_0x_xxxxxxxxxx，然后我们在里面进行一个删除，将没有用的几把玩意全他妈删了，只保留pom文件和src的文件夹，接着我们将pom文件中的artifactId，改为我们的文件夹的名字，接着将下面的这两行代码删除

```
<name>demo</name>
<description>Demo project for Spring Boot</description>
```

这两行代码没有实际意义，但是name标签里面写入的文本会成为我们的右边的maven的名字，如果我们这里不删除，那以后我们拿这个当模板，搞多了以后右边全他妈是一模一样的名字，这尼玛怎么分啊？所以我要将这两个标签删除掉，这样的话其名字就会按照我们创建的名字来，这样我们就不会区分不出来了

最后我们如果需要新的模块的话，就是只需要将这个模块复制改名就完了，然后将pom文件的对应的id也改名，然后加载依赖就可以使用了

- 属性配置方式

接着我们正式来学习我们的SB中的属性配置的方式，我们首先想要配置的是我们的Tomcat的端口号，我们希望将我们的8080的端口号改成80，这时我们应该要怎么办呢？

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE93a2cb002558eabc53e449bbafa3cde0.png)

我们可以点开我们的resources文件夹，然后点开application.properties文件，可以看到里面什么都没写，但是其终究是一个properties文件，那么其更改方式必然是key=value，端口号的英文是port，输入port回车就会有对应的提示，我们敲入这个然后直接进行一个修改，写入代码如下

```
# 服务器端口配置
server.port=80
```

然后我们再启动我们的服务器时我们就可以看到端口号已经完成80了，此时就说明我们已经成功修改了我们的配置了

接着我们要讲解的是，我的这个配置文件实际能配置的东西是基于我们的项目到底引入了多少依赖决定的，比如说我们上面引入了服务器的依赖，所以我们下面可以配置对应的端口号，但如果没有这些依赖了，那就没法配了，寄！

同时我们的方法非常多，如果想要学习对应的配置方法的话，可以去SB的官网上去自己查询

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE9ca95a5af1ae990fa5a55211d85443eb.png)

- 3种配置文件类型

在SB中，提供了三种配置文件的类型，分别是properties、yml和yaml。目前主流的方式是使用yml

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEbce62e4fa16244d3e7673f902866028f.png)

- 配置文件加载优先级

然后我们来讲下配置文件的加载优先级的事情，这个直接看图吧

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc0678bec42c630548a08dc5864495099.png)

- 属性提示消失解决方案

我们会发现当我们进入yaml文件的时候，我们往里面编辑的时候，会发现没有出现对应的属性提示，这是因为我们的idea不认为yaml文件是一个配置文件，所以没有出来对应的属性提示。那要怎么办呢？我们可以点击模块窗口，然后选择facts，选择我们对应的工程，接着在右边的窗口中选择那个绿叶

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEabad606fa8cbe8f06a51af7fdd659d45.png)

然后选择+号，接着我们往里面添加我们要令其成为配置文件的文件就可以了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa351787f3e610415e71e752c083a872c.png)

如果上面报红，不给点OK的话，我们可以事先创建我们的Application.properties文件就可以了，不过什么都不往里面写入

- yaml数据格式

接着我们来学习yaml的数据格式，其优点如下图所示，其其对比于XML和Properties而言，具有很不错的有点，因此我们主流都是用这种格式

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE291e2b64c7fb9ebc36eb40c1da075dc7.png)

接下来我们来学习yaml的语法规则，我一般来说只要记住一个，那对于我们的yaml的语法规则而言，我们放数据的位置是在:后面，而且都会有一个空格，记住这个就可以了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEab4090bd65d5ecaa9b755b1dfffe5c03.png)

后续还有很多的各种不同的数据的存放方式，也就简写方式，这些作为了解就可以了，后面的课程中用不太上其实

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb43d3fe459190cd06271b17689d6cc79.png)

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE26b40dfa6b6733d3d192ae50c4410763.png)

- 读取ymal单一属性数据

接着我们来学习读取ymal的数据的方式，我们一般是直接在控制层中使用Value注解来读取单个数据，配合EL表达式来获取，如果有多个层级，我们就用一级属性名.二级属性名的方式来获取对应的值，如果是数组则要加入对应的下标

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE1a265d0f98d22d49eb646a116e5c4de8.png)

那么我们可以再控制层中写入我们的获取代码如下

```
//读取yaml数据中的单一数据
@Value("${country}")
private String country1;

@Value("${user.name}")
private String name1;

@Value("${likes[1]}")
private String likes1;

@Value("${users[1].age}")
private String age1;

@Value("${server.port}")
private String port;
```

其配置类中的代码如下，其数据是我们上面要获取到的数据

```
country: china
province: beijing
city: beijing
area: haidian

port: 8080

party: true

birthday: 1949-10-01

user:
  name: itcast
  age: 16

user2:
  name: itheima
  age: 16


a:
  b:
    c:
      d:
        e: 123

likes:
  - game
  - music
  - sleep

likes2: [game,music,sleep]

users:
  - name: zhangsan
    age: 18
  - name: lisi
    age: 17

users3: [{name:zhangsan,age:18},{name:lisi,age:17}]

users2:
  -
    name: zhangsan
    age: 18
  -
    name: lisi
    age: 17
```

这一节内容也是告诉我们，我们的配置的东西其实都是数据，是可以拿出来的，就好比如我们设置的端口号

- yaml文件中的变量引用

如果我们希望我们的某一个字符串可以被统一引用，我们可以先设置对应的属性，然后在后面设置对应的数据，然后后面的数据如果想要获得前面的数据，可以通过EL表达式的方式直接引用，其实跟之前的差不多，不过我们这里不需要加Value注解了。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE340cace1c329a3472661a1ab79d9bd58.png)

另外是关于转义字符，如果我们希望转义字符被解析，那么我们要加入双引号，否则就无法解析，会把其当字符串来看待

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEdba76572255d1c694ac5dcdf3c0440f1.png)