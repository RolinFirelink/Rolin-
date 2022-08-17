学习完了SpringMVC之后，现在我们正式进入SpringBoot的学习，我们要加快进度了，因为我们很快就要到我们的项目制作中去了

# 课程导学

我们先来看看为什么我们要学习SpringBoot

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5c82796e5cd28327dada33720773b2dc.png)

我们目前暂时先学基础篇，赶鸭子上架就赶鸭子上架吧，学了再说，同样的按照惯例我们先做一个入门案例来帮助我们的学习

# SpringBoot入门案例

## idea联网版

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE277d611a5c942f5679269092f0396669.png)

这里唯一的痛点就在于，这个工程是一定要联网的，实际上，工程最开始创建的时候，其就需要指定一个给其连接某个网页的地址，那是不是如果没有网络我们就搞不了SB了呢？当然不是，接下来我们做一个不用联网的案例。

## 官网创建版

我们可以去SpringBoot的官网创建对应的项目，进入对应的SpringBoot页面就可以了，其会生成一个压缩包，我们可以利用这个压缩包导入到我们的工程中，然后该模块就有和我们之前一样的功能了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa6ccc01ed6fc0182db5d722f68adc261.png)

说实话我个人感觉这未免有点脱裤子放屁，我能联网了我干嘛不直接用idea呢

## 阿里云版

本节我们来学习SpringBoot的阿里云版本，其实就是在创建对应的SB项目时把要访问的网址改成阿里云的，其地址是http://start.aliyun.com

这里值得一提的是，阿里云默认提供的版本比较低，因此我们可以在pom.xml文件中进行一个版本的改，直接改成我们所需要的版本就可以了

最后来看下总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE69c3000e0cc1281129adf388f9e6dd5c.png)

## 手工制作版

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

# idea中隐藏文件或文件夹

创建SB带来的附带文件太多，我们可以进入setting中自己去设置因此掉对应的文件夹，具体的设置规则其实和之前的差不多

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8123afe3813f5afa52288d71c164e50c.png)

# 入门案例解析

## parent

接着我们就来正式解析我们的项目了，我们先来解析parent，我们先来看看我们的SB的好处

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE17651759546bdc2099b37fba6edeced7.png)

然后我们来说下我们的Parent标检下的代码的作用，我们开发的时候，总是会遇到两个模块用同一个依赖的时候，这时候我们一般只能无脑引入两次，这有点麻烦，而且最重要的是，容易出现包冲突的情况，此时我们不得不进行排除依赖等操作进行一个调包，那是一个突出痛苦啊，一直整都整不好。但是SB里面的Parent标签就帮我们解决了这个问题，其继承的依赖中事先引入了许多的不同版本的依赖，就相当于是规定了我们引入对应的依赖的时候，就指定使用这个版本，因为他知道使用这个版本最好，就相当于是帮我们做了一个版本的统一，这样我们就不用再考虑那瘠薄的版本冲突了。

其利用版本的时候是用到了maven高级的一些内容的，有出现类似于EL表达式的内容，我们这里赶工，就暂时先将这一部分的内容按下不表。

最后我们值得一提的是，我们从springboot官网中创建的对应的项目文件中，其引入对应的类的方式是继承，也就是使用parent标签，而在阿里云中，则不是使用继承，而是直接将这个继承的类当成一个依赖进行引入，后者可以引入多个，而前者只能继承一次，这是他们的分别

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE48c3aa5cd9fd71fdbab9706c9d350a6d.png)

## starter

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE48f5d28ea74649dc0cab152e5c8133cc.png)

最后要注意我们自己需要的是引入坐标的是注意版本，别搞了两个模块出现版本冲突了

## 引导类

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

## 辅助功能

接着我们存在的一个问题就是，我们的Tomcat服务器是怎么启动的？其实我们的Tomcat的服务器是被引入到我们的依赖中了，其是作为一种内嵌服务器来运行的，其引入的依赖的代码是在我们的starter引入依赖中一并引入的

如果我们将对应的Tomcat依赖给排除掉的话，我们的程序仍然能正常运行，不过这次就会自动停止了，因为压根就没有服务器，当然运行完就停了。我们也可以在排除了对应的依赖之后再引入其他的服务器的依赖，这样就可以使用其他的内嵌服务器了

最后我们可以来做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE93a2cb002558eabc53e449bbafa3cde0.png)

# 复制模块

接着我们来学习我们的属性配置方式，在学习之前，我们要先来学习一个复制模块的技巧，因为学习的时候避免不了要创建多个项目，每次都重新创建一次就太麻烦了，所以我们来学习下复制模块的技巧

我们选择第二个模块，因为这个模块我们没做什么改动，我们首先将这个模块复制一个副本，然后给其改个名字，就改名为springboot_01_0x_xxxxxxxxxx，然后我们在里面进行一个删除，将没有用的几把玩意全他妈删了，只保留pom文件和src的文件夹，接着我们将pom文件中的artifactId，改为我们的文件夹的名字，接着将下面的这两行代码删除

```
<name>demo</name>
<description>Demo project for Spring Boot</description>
```

这两行代码没有实际意义，但是name标签里面写入的文本会成为我们的右边的maven的名字，如果我们这里不删除，那以后我们拿这个当模板，搞多了以后右边全他妈是一模一样的名字，这尼玛怎么分啊？所以我要将这两个标签删除掉，这样的话其名字就会按照我们创建的名字来，这样我们就不会区分不出来了

最后我们如果需要新的模块的话，就是只需要将这个模块复制改名就完了，然后将pom文件的对应的id也改名，然后加载依赖就可以使用了

# 属性配置方式

接着我们正式来学习我们的SB中的属性配置的方式，我们首先想要配置的是我们的Tomcat的端口号，我们希望将我们的8080的端口号改成80，这时我们应该要怎么办呢？

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbce62e4fa16244d3e7673f902866028f.png)

我们可以点开我们的resources文件夹，然后点开application.properties文件，可以看到里面什么都没写，但是其终究是一个properties文件，那么其更改方式必然是key=value，端口号的英文是port，输入port回车就会有对应的提示，我们敲入这个然后直接进行一个修改，写入代码如下

```
# 服务器端口配置
server.port=80
```

然后我们再启动我们的服务器时我们就可以看到端口号已经完成80了，此时就说明我们已经成功修改了我们的配置了

接着我们要讲解的是，我的这个配置文件实际能配置的东西是基于我们的项目到底引入了多少依赖决定的，比如说我们上面引入了服务器的依赖，所以我们下面可以配置对应的端口号，但如果没有这些依赖了，那就没法配了，寄！

同时我们的方法非常多，如果想要学习对应的配置方法的话，可以去SB的官网上去自己查询

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9ca95a5af1ae990fa5a55211d85443eb.png)

# 3种配置文件类型

在SB中，提供了三种配置文件的类型，分别是properties、yml和yaml。目前主流的方式是使用yml

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd5c9bedd51f56e4b94b622dc968f07a2.png)

## 配置文件加载优先级

然后我们来讲下配置文件的加载优先级的事情，这个直接看图吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEabad606fa8cbe8f06a51af7fdd659d45.png)

## 属性提示消失解决方案

我们会发现当我们进入yaml文件的时候，我们往里面编辑的时候，会发现没有出现对应的属性提示，这是因为我们的idea不认为yaml文件是一个配置文件，所以没有出来对应的属性提示。那要怎么办呢？我们可以点击模块窗口，然后选择facts，选择我们对应的工程，接着在右边的窗口中选择那个绿叶

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE291e2b64c7fb9ebc36eb40c1da075dc7.png)

然后选择+号，接着我们往里面添加我们要令其成为配置文件的文件就可以了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc0678bec42c630548a08dc5864495099.png)

如果上面报红，不给点OK的话，我们可以事先创建我们的Application.properties文件就可以了，不过什么都不往里面写入

## yaml数据格式

接着我们来学习yaml的数据格式，其优点如下图所示，其其对比于XML和Properties而言，具有很不错的有点，因此我们主流都是用这种格式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa351787f3e610415e71e752c083a872c.png)

接下来我们来学习yaml的语法规则，我一般来说只要记住一个，那对于我们的yaml的语法规则而言，我们放数据的位置是在:后面，而且都会有一个空格，记住这个就可以了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEab4090bd65d5ecaa9b755b1dfffe5c03.png)

后续还有很多的各种不同的数据的存放方式，也就简写方式，这些作为了解就可以了，后面的课程中用不太上其实

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE26b40dfa6b6733d3d192ae50c4410763.png)

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb43d3fe459190cd06271b17689d6cc79.png)

## 读取ymal单一属性数据

接着我们来学习读取ymal的数据的方式，我们一般是直接在控制层中使用Value注解来读取单个数据，配合EL表达式来获取，如果有多个层级，我们就用一级属性名.二级属性名的方式来获取对应的值，如果是数组则要加入对应的下标

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEdba76572255d1c694ac5dcdf3c0440f1.png)

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

## yaml文件中的变量引用

如果我们希望我们的某一个字符串可以被统一引用，我们可以先设置对应的属性，然后在后面设置对应的数据，然后后面的数据如果想要获得前面的数据，可以通过EL表达式的方式直接引用，其实跟之前的差不多，不过我们这里不需要加Value注解了。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE340cace1c329a3472661a1ab79d9bd58.png)

另外是关于转义字符，如果我们希望转义字符被解析，那么我们要加入双引号，否则就无法解析，会把其当字符串来看待

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1a265d0f98d22d49eb646a116e5c4de8.png)

## 读取ymal的全部数据

如果我们在配置中写了很多的数据，那么我们在单独获取的时候代码量就会增加很多，此时我们有一种方式可以获得所有的数据，并将其封装到一个对象中，SpringBoot给我们提供了这么一个对象，这个对象就是Environment对象，要使用该对象，就要使用Autowired注解，令其自动获得我们的对象

```
//使用自动装配将所有的数据封装到一个对象Environment中
@Autowired
private Environment env;
```

如果要获取里面的数据，就要调用其中的getProperty方法，传入对应的数据的名称的字符串就可以获得了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc3d3e74fa65764b1f0ce3c69a322b453.png)

我知道你们肯定想说这他妈不是脱裤子放屁吗？别急，其实我也是这么想的，这招基本没用。一般来说我们都是将我们的固定的内容放到一起，然后将其封装为一个对象然后在获得的，这也是我们目前最主流的方式

假设我们要封装如下的数据类

```
datasource:
  driver: com.mysql.jdbc.Driver
  url: jdbc:mysql://localhost/springboot_db
  username: root
  password: root666123
```

此时我们可以容易知道，这个对象的名字叫datasource，而且其下有四个属性，那么我们就要去构造其对应的封装类，我们可以构造其对应的代码如下

```
package com.itheima.springboot_01_02_quickstart;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

//1.定义数据模型封装yaml文件中对应的数据
//2.定义为spring管控的bean
@Component
//3.指定加载的数据
@ConfigurationProperties(prefix = "datasource")
public class MyDataSource {
    private String driver;
    private String url;
    private String username;
    private String password;

    @Override
    public String toString() {
        return "MyDataSource{" +
                "driver='" + driver + '\'' +
                ", url='" + url + '\'' +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                '}';
    }

    public String getDriver() {
        return driver;
    }

    public void setDriver(String driver) {
        this.driver = driver;
    }

    public String getUrl() {
        return url;
    }

    public void setUrl(String url) {
        this.url = url;
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
}

```

我们这里之所以构造这样的代码，首先第一点为了要封装对象，我们要在类中写入对应的属性并提供对应的方法，然后给类必须要给Spring管理，否则后续我们无法获取到这个对象，因此我们加入了Component注解，接着这个对象需要知道其到底应该接受哪个配置文件中的数据，因此我们要使用ConfigurationProperties注解并指定datasource为括号内的内容，这样其就会寻找到dataSource的对象并将其数据获取到

然后我们在控制类使用自动获取对象的注解获取到该对象并打印，可以看到其内容就是我们封装的数据

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf58005298c74fa31dea2fa1e4edbf23e.png)

经过了这节的学习我们也能够理解为什么我们的这里的配置的数据总是上下且带着空格隔开的形式了，上面就代表一个对象，而下面的内容则是上面对象的属性，这个性质对其下的属性也是适用的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb754f3e20e3d47e0dadab3aea03aafca.png)

注意，第一个获取全部对象的方式可以忘记，但是这个不行，这个方法非常重要，因为我现在大多都是使用这个方法来获取到我们的对应的数据的，也是这样来存储数据的

# SpringBoot整合Junit

接着我们来学习SpringBoot整合第三方技术，首先我们来学习整合Junit，先来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb7ebc0dc45c24737552a41f38b49a404.png)

我们可以创建对应的接口然后实现其对应的实现类，做一个测试方法，将其加入到Spring容器中，然后在对应的test类中写入代码如下

```
package com.itheima.springboot_04_junit;

import itheima.dao.BookDao;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class Springboot04JunitApplicationTests {

    //1.注入你要测试的对象

    @Autowired
    private BookDao bookDao;

    @Test
    void contextLoads() {
        bookDao.save();
    }

}

```

我们这里的第一步是要导入测试所需要的对应的starter，不过这事情一开始其就已经帮我们做了，其还会顺便引入一个用于测试的依赖，因为本质上SpringBoot的工程还是一个maven工程，而对于任何一个maven工程而言，其不可能避开的一点就是需要测试，因此任何SpringBoot工程其就是会默认导入一个基本的依赖以及测试依赖的

然后我们可以看到的是，如果我们的测试类中的类不和我们的引导类，也就是Springboot04JunitApplication保持平级或者是在其子级之下的话，那么我们的编译过程就会报错，会显示压根没有对应的Bean，这是怎么回事呢？

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEacf3b5d97b75e0f813b4fc7bbe0027a8.png)

这是因为我们的测试类默认的注解SpringBootTest，其会在其当前包或者其父包上寻找SpringBootConfiguration注解，如果我们一开始就将这个类的地址放到比我们的引导类还要高的层级上，那么其肯定找不到，那当然会报错。有的同学可能会说，你这引导类里的注解是SpringBootApplication啊，你这样也不是SpringBootConfiguration啊，其实这是因为前者的注解下包含了后者的注解而已，虽然看不见但是的确是有的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE60c706712e68a7a9c45c5253563e13d5.png)

想要解决这个问题有两种方式，一种方式是往我们的SpringBootTest中直接传入我们的引导类的字节码文件，另一种方式将其设置为其子级，这个很好理解，就不多提了。其实还是第三种方法，就是使用ContextConfiguration注解然后传入我们的引导类的class文件，不过这个纯属是脱裤子放屁，知道就行了

接着我们来继续学习SB的基础篇，本次我们来学习使用SB整合各种第三方技术

# SpringBoot整合MyBatis

我们先来看看我们整合MyBatis要做的事情

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8c030971aadd063763cf04996a140e22.png)

那么首先我们创建这个一个工程，在勾选的技术上勾选中SQL下的MyBatis以及数据库连接的相关驱动技术，这里我们勾选中对应的技术，其实就相当于是令其帮助我们先加入我们所需要的坐标，没了。当然，他要能帮我导入我们需要的坐标我们当然是省事了就是

我们在其对应的pom文件中可以看到如下的依赖

```
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.2.2</version>
</dependency>
```

可以看到我们这里导入了mybatis-spring-boot-starter的包，这里我们要提一点的是，任何由我们的Spring提供的依赖，其别名都是spring-boot-xxx，而由第三方技术提供的依赖，则是yyyyy-spring-boot-xxxx，其中yyyyy代表第三方的技术名称

接着我们就去写入我们连接数据库的配置，我们可以写入其配置代码如下（注意我们这里的配置只是一个对应的格式，实际连接的话这份代码是需要改动的）

```
#2.配置相关信息
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db
    username: root
    password: itheima

```

然后我们创建对应的封装类并提供对应的方法，然后我们创建对应的接口并用注解动态实现其查询方法

```
package com.itheima.dao;

import com.itheima.domain.Book;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

@Mapper
public interface BookDao {
    @Select("select * from tbl_book where id = #{id}")
    public Book getById(Integer id);

}

```

这里我们要使用Mapper注解，否则我们添加的SQL映射是无法被容器识别到的，所以我们这里一定要使用Mapper注解

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe2269f82a68c679f03993cfe58797e4d.png)

最后我们进入测试类进行一个测试就完了，发现的确可以正确获得其数据，那就行了。

然而如果我们的SpringBoot版本比较低的话，那么我们的这个程序就会报出连接错误的异常。这其实是因为我们的SB版本降低导致我们的MySQL版本改为8.X，其会强制要求设置时区，因为我们没有设置时区，所以会报错

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE133067a01a24ea364ad1fbcdb8c3a651.png)

有两种解决方式，一种是修改url，另一种是修改MySQL的数据库配置，我们这里只讲前一种，具体方式就是将代码修改如下就可以了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3f9d03440054670c9696af44498bc03c.png)

# SpringBoot整合MyBatisPlus

为了这一节的内容我们可是回去学习了一天的MP的，突出一个痛苦不堪，现在学成归来，我们速速将这一节过了。我们先来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf812cf6254370c89a84debf7eab2d1ec.png)

首先我们要创建一个新的Spring工程，勾选对应的坐标，但是不幸的是Spring中并没有收录MP的坐标，所以我们必须手动导入对应的坐标，其坐标如下

```
<!--        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>-->

        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.4.3</version>
        </dependency>
```

并且导入完对应的坐标后都可以将被注释的原来SB帮我们导入的坐标给删除掉了，因为这些包在我们导入的包里就已经导入了，因此我们这里删除，不必重复导入。

其实还有一种方法可以让我们解决这个问题，就是用阿里云的网址来去生成SB项目，其下就有MP的坐标，但是问题是这个版本太低了，所以屁用没有。

到此为止我们就整合完了，没错，整合完了。之后就按照MP的方式来去改造我们的实现查询的接口类就可以了

最后别忘了我们还有设置数据表的全局配置

```
mybatis-plus:
  global-config:
    db-config:
      table-prefix: tbl_
```

# SpringBoot整合Druid

这个内容其实非常简单，我们首先创建对应的工程，然后导入对应的坐标，接着我们手动引入Druid的依赖如下

```
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.2.6</version>
</dependency>
```

然后其实我们就已经整合完毕了，接着要做的事情就是配置我们的连接池，我们有两种方式，如下所示

```
#2.配置相关信息
#spring:
#  datasource:
#    driver-class-name: com.mysql.jdbc.Driver
#    url: jdbc:mysql://localhost:3306/ssm_db
#    username: root
#    password: itheima
#    type: com.alibaba.druid.pool.DruidDataSource

spring:
  datasource:
    druid:
      driver-class-name: com.mysql.jdbc.Driver
      url: jdbc:mysql://localhost:3306/ssm_db
      username: root
      password: itheima

```

我们第一种方式是直接设置数据源的type属性，然后设置其数据源为DruidDataSource，这样可以，但我们不推荐这样的。我们正规的方式应该是下面那种，我们在数据源对象中引入druid的对象，然后设置其里面的连接数据令其能够正确获得连接（注意，能使用这种方式的前提是我们导入了druid的starter对应依赖）

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc34578b04c39829cb15709d958640404.png)

最后整合了这么多技术，相信各位也从里面看出门路来了，其经典流程其实就下面两步

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE175a1549ef76baf10074f75ef34e7875.png)

# SSMP整合案例制作分析

接着我们来讲我们的SSMP的整合案例，其实也就是我们的SpringBoot整合我们的MyBatisPlus以及SpringMVC等技术，先来看看其步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa2b5104222761dbcae73030bbd44bbd4.png)

接着我们先来创建我们的对应的模块，导入对应的坐标，然后引入MP和Druid的坐标，修改配置文件的格式，然后设置端口号

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc2a478d53937647e7f3f8f36f7a43da1.png)

然后我们创建我们的封装数据的实体类，这里我们使用lombok注解令其自动生成对应的方法，那么我们可以构建我们实体类的代码如下

```
package com.itheima.domain;

import lombok.Data;

@Data
public class Book {
    private Integer id;
    private String type;
    private String name;
    private String description;
}

```

然后我们要提一点的是，在我们的SB中，如果我们想要成功连接我们的远程数据库，那么我们连接代码应该是如下所示

```
spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://175.178.114.158:3306/ssm_db?useUnicode=true&useSSL=false&serverTimezone=GMT&characterEncoding=UTF-8
      username: root
      password: itheima
```

可以看到我这里再url处还加了一堆不知道什么几把玩意，但是没有这串玩意就连接不上，所以我们还是加上去吧

然后我们正式来实现我们的简单的增删改查的功能，首先我们创建对应的接口并用MP的继承方式来令其生成对应的增删改查的动态方法

再写入我们的全局配置如下，这里设置了自增策略和统一的前缀名

```
mybatis-plus:
  global-config:
    db-config:
      table-prefix: tbl_
      id-type: auto
```

然后我们就去测试类中写入代码如下

```
package com.itheima.dao;

import com.itheima.domain.Book;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
public class BookDaoTestCase {

    @Autowired
    private BookDao bookDao;

    @Test
    void testGetById(){
        System.out.println(bookDao.selectById(1));
    }

    @Test
    void testSave(){
        Book book = new Book();
        book.setType("我早已心如槁木");
        book.setName("大潮奔涌");
        book.setDescription("为他，我必当如此");
        bookDao.insert(book);
    }

    @Test
    void testUpdate(){
        Book book = new Book();
        book.setId(12);
        book.setType("我早已心如槁木");
        book.setName("大潮奔涌");
        book.setDescription("为他，我必当如此");
        bookDao.updateById(book);
    }

    @Test
    void testDelete(){
        bookDao.deleteById(14);
    }

    @Test
    void testGetAll(){
        System.out.println(bookDao.selectList(null));
    }

}

```

上面的测试中，查询方法没有问题，但是一旦涉及到修改就会报错，为什么会这样我也搞不懂，而且百度了很久也找不到解决方法，那就先这样吧。最后来看看总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE79e5828b0e18bb8c31b34fcfda6117db.png)

接着我们来开启MyBatisPlus的调试日志，开启这个可以让我们看到操作执行过程的所有信息

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb75c6ec0aa134c229b9b7a4f1fc59968.png)

开启了这个之后我们就不用在测试类里自己打印数据了，同时要注意的是，项目上线的时候这个不能打开，要不然后台直接炸了。

然后我们接着将来实现分页查询的功能，这里实现分页功能的过程和实现MP分页查询的过程一模一样，首先写入拦截器代码如下

```
package com.itheima.config;

import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MPConfig {
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return interceptor;
    }
}
```

然后我们写入测试类的代码如下

```
@Test
void testGetPage(){
    IPage page = new Page(1,5);
    bookDao.selectPage(page,null);
    System.out.println(page.getCurrent());
    System.out.println(page.getSize());
    System.out.println(page.getTotal());
    System.out.println(page.getPages());
    System.out.println(page.getRecords());
}
```

说实话，这也算是SB整合？这特么不就是在用MP那一套吗？

至于按条件查询，这个内容居然和MP的是完全一模一样的，我真的是懒得再写一遍了，直接看图吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE212688a9670e693597024dddb020570e.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa7dfdb808f8d9d874fd4bb5ca1b81732.png)

接着我们来学习业务层的功能，其实也就是多定义一个和实现类的事，这里值得一提的是，我们数据层的方法是非常直接的和以和数据打交道的名字来命名的，而在业务层中则是以业务功能来命名的，这是规范。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0d0765ad39adbcc819d8b0257442a1af.png)

这里的代码就不传上来了，我是真的嫌麻烦，其也就是做了一个业务层调用数据层的动作而已，其他的没什么

然后我们来学习快速开发Service层的方式，其实这个方式就是MP章节中最后介绍的快速生成的Service的继承类，我们当时提了提，这里我们就认真学学

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8fbbc27e1944337c97b90812fc2882ba.png)

其接口继承的类是Iserivce，该类类似于BaseMapper，继承该类可以让我们的类获得各种对应的业务层的方法，然后我们要写入其实现类，如果我们写入其实现类的话，就得实现其下的所有抽象方法，那也很麻烦，所以我们可以在其中再继承一个ServiceImpl的类，传入我们的数据层的接口对象和Book实体类对象，就可以自动帮我们生成这些方法

```
@Service
public class BookServiceImpl1 extends ServiceImpl<BookDao, Book> implements IbookService {
}
```

然后经过实际测试我们会发现这个方法是可以使用的，只不过其方法名和我们的有些不同就是了。这里要提一点的是，这里自动生成的分页方法是要我们自己先创建一个分页对象然后传进去的，跟我们之前自己定义的直接传入对应的分页参数就可以运行的方法是不同的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEeeb486ee4110783f329e143880610c04.png)

最后我们值得一提的是，如果说其中的提供的方法无法满足我们的业务需求，我们可以自己在接口中定义新的方法来满足我们的需求，注意不要覆盖原始的方法，否则会造成原始提供的功能丢失，我们可以通过Overwrite注解来判断是否对原始方法进行了重写

# 表现层数据开发

现在数据层和业务层都搞定了，接着我们来干掉最后一层，表现层。我们要制作表现层，当然首先得创建对应的表现层，我们这里要使用Rest风格来开发，所以我们在类上加入对应的注解，在后面的方法中我们也要加入使用Rest风格的对应注解

```
package com.itheima.controller;

import com.baomidou.mybatisplus.core.metadata.IPage;
import com.itheima.domain.Book;
import com.itheima.service.IbookService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/books")
public class BookController {

    @Autowired
    private IbookService bookService;

    @GetMapping
    public List<Book> getAll(){
        return bookService.list();
    }

    @PostMapping
    public Boolean save(@RequestBody Book book){
        return bookService.save(book);
    }

    @PutMapping
    public Boolean update(@RequestBody Book book){
        return bookService.modify(book);
    }

    @DeleteMapping("{id}")
    public Boolean delete(@PathVariable Integer id){
        return bookService.delete(id);
    }

    @GetMapping("{id}")
    public Book getById(@PathVariable Integer id){
        return bookService.getById(id);
    }

    @GetMapping("{currentPage}/{pageSize}")
    public IPage<Book> getPage(@PathVariable int currentPage,@PathVariable int pageSize){
        return bookService.getPage(currentPage,pageSize);
    }
}

```

我们上面的代码都是用我们以前所学习过的知识所构造而来的，我们这里唯一新增的是我们的分页操作，这里我是特别新构造了一个分页方法整来的，这点要注意

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb875f7d8778e469fedf24bb101e8dc5a.png)

最后我们来复习下RequestBody注解和PathVariable注解的不同，前者表示接受一个对象，而后者则是接受具体的值，如果我们接受的内容只是一两个值，那么我们建议用后者，其代表从路径中直接获取对应的数据，如果是一个对象，那么就要用前者，其代表的意思是接受传送过来的json数据，并将其转换为所需的对象（当然，我们传过来的内容是通过post封装成json数据传过来的）

# 表现层数据一致性处理

接着我们来做表现层数据和后端返回数据的一致性处理，也就是进行后盾与前端的数据格式的统一，也被称为前后端数据协议。这个内容我们在SpringMVC中已经做过了，这里我们快速过一遍

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE97de0d18f4cafb4dacd27095a9db33fc.png)

首先我们创建对应的用于统一格式的类并提供其所有的构造方法

```
package com.itheima.controller.utils;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.omg.CORBA.PUBLIC_MEMBER;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class R {
    private Boolean flag;
    private Object data;

    public R(Boolean flag){
        this.flag=flag;
    }

    public R(Object data) {
        this.data = data;
    }
}

```

然后将我们的表现层返回的结果都封装到我们的统一格式类中并返回给前端人员就完了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5900ee471da9abb7b8587673ee3fd6e8.png)

# 前后端调用（axios发送异步请求）

因为这一节的内容又让我不得不回去把Vue和Element的内容给快速看完了，我真的佛了，横竖都跑不开这些内容。总之我们花了半天时间总算是迅速过了一遍了，那么现在我们就正式开始我们的学习吧

首先我们来看看我们的步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc0d7d150ede39d0b02aefe7d5c634ebc.png)

这里我们要特别提一下的是，一般来说我们的单体工程是放在resourece下的static目录下的，同时一般我们的前端和后端是有两个不同的服务器的，不过我们显然没这么好的资源，所以我们这里就直接一个服务器就开整了，就不搞这么多有的没的了。最后我们这里还要提一下的是，如果我们的工程中出现了什么奇怪的问题，那么建议先clean一下重新编译试试，idea是有bug的，很多东西就是指不定重新编译下就好了

然后我们将对应的工程引入到对应的文件夹中，然后我们这里有已经先整好了的books.html文件，里面有提前构造好的函数和页面。我们利用其下的钩子函数create，只要vue初始化完成就必定会执行这个方法，这里调用我们的查询全部的方法，然后我们查询全部的方法里利用axios发送了一个get的异步请求，其请求的服务是查询去所有数据，然后利用then获得返回的结果接着在控制台上输出

```
//钩子函数，VUE对象初始化完成后自动执行
created() {
    //调用查询全部数据的操作
    this.getAll();
},

methods: {
    //列表
    getAll() {
        //发送异步请求
        axios.get("/books").then((res)=>{
            console.log(res.data);
            });
    },
```

实际上我们在控制台上和我们的idea的日志上都可以找到对应的数据，此时就说明我们的文件已经成功了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE17e7596ebaa259eb36e436c4d4573d38.png)

## 列表功能

想必大家也是很容易猜到，我们之后的要做的内容就是往我们的books.html中不断添加我们的功能。本节我们来实现列表功能，接着我们就要将我们的数据库的数据内容读取到我们的页面中去，我们通过分析很容易能知道，我们的页面展示的数据是通过dataList里的数据来展示的，因此我们的思路就是每次我们的Vue对象产生时，就自动查询全部数据并将对应的数据加到dataList中去，这样我们的页面就能正确展示我们的数据了

那么我们可以将我们的getAll的函数的代码修改如下

```
getAll() {
    //发送异步请求
    axios.get("/books").then((res)=>{
        // console.log(res.data);
        this.dataList = res.data.data;
        });
},
```

这里我们要注意，我们取出的数据是先从我们的res响应对象中取出其data数据，然后再取出其下真正保存数据的data对象，因为我们的数据一开始是被我们进行了封装的，因此我们取的时候也是要拆除掉那些封装，拿出我们真正需要的东西

然后这时我们页面上进行刷新，可以看到我们的数据真的被取出来了，此时就说明我们的列表功能就完成了

## 添加功能

接着我们来实现添加功能，我们的添加功能当然是从我们的案例中的添加按键中执行的，我们首先查看我的源代码的新建功能

```
<el-button type="primary" class="butT" @click="handleCreate()">新建</el-button>
```

可以看到其已经绑定了一个handleCreate()的事件，然后我们查看其事件，会发现这个事件函数啥也没写，那么我们就要从头开始构造这个功能了

首先我们的点击新建功能时，我们肯定希望其弹出一个添加窗口，而这个添加窗口其已经帮我们准备好了

```
<div class="add-form">

    <el-dialog title="新增图书" :visible.sync="dialogFormVisible">

        <el-form ref="dataAddForm" :model="formData" :rules="rules" label-position="right" label-width="100px">

            <el-row>

                <el-col :span="12">

                    <el-form-item label="图书类别" prop="type">

                        <el-input v-model="formData.type"/>

                    </el-form-item>

                </el-col>

                <el-col :span="12">

                    <el-form-item label="图书名称" prop="name">

                        <el-input v-model="formData.name"/>

                    </el-form-item>

                </el-col>

            </el-row>


            <el-row>

                <el-col :span="24">

                    <el-form-item label="描述">

                        <el-input v-model="formData.description" type="textarea"></el-input>

                    </el-form-item>

                </el-col>

            </el-row>

        </el-form>

        <div slot="footer" class="dialog-footer">

            <el-button @click="cancel()">取消</el-button>

            <el-button type="primary" @click="handleAdd()">确定</el-button>

        </div>

    </el-dialog>

</div>
```

这个添加窗口因为我们的一开始设置展示属性为false，因此其不展示。而我们可以看到里面的model属性定义了其对象为formData，且在下面都有对应的代码将用户提交的数据封装到这个对象里面，那么用户提交的数据就保存在formData对象里了

```
dialogFormVisible: false,//添加表单是否可见
```

那么我们可以再在我们的handleCreate函数中将该属性设置为true，这样其就可以展示了。同时我们每次进入添加页面时，我们都希望先清除掉预留在该窗口中的数据，因此我们让用户每次进入对应的添加窗口时就调用resetForm重置表单参数

```
//弹出添加窗口
handleCreate() {
    this.dialogFormVisible = true;
    this.resetForm();
},
```

可以看到重置表单的参数内容也很简单，就是将对应的对象内容给赋值为空而已

```
//重置表单
resetForm() {
    this.formData = {};
},
```

然后我们可以具体写入我们的添加和取消窗口的代码如下，我们这里利用axios向服务器发送post请求，我们这里同时将用户提交的数据也一并传过去了，接着我们获得其传过来的响应对象，然后先判断当前操作是否成功，若成功则关闭弹层并提示添加成功，若失败就提示添加失败，但无论结果如何，其都必须要重新加载一次我们的数据，这样可以让我们的新增的数据被正确展示，我们的这里重新加载数据的方式就再次调用我们的getAll函数就可以了

```
//添加
handleAdd () {
    axios.post("/books",this.formData).then((res)=>{
        //判断当前操作是否成功
        if(res.data.flag){
            //1.关闭弹层
            this.dialogFormVisible = false;
            this.$message.success("添加成功");
        }else{
            this.$message.error("添加失败");
        }
    }).finally(()=>{
        //2.重新加载数据
        this.getAll();
    });
},

//取消
cancel(){
    this.dialogFormVisible = false;
    this.$message.info("当前操作取消");
},
```

最后我们的当前操作取消的取消函数也是有设置对应的事情的，首先让弹窗关闭，然后提示当前操作取消

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5ec3b6e9ad8fe5dc385dbf3520e69c58.png)

最后有一点值得一提的是，我们的案例不知道为什么写那个逼代码写一半就直接废了，样式都不加载了，后面将问题代码注释掉也没有用，给我蚌埠住了，反正不是什么重要的项目，那就先这样吧

## 删除功能

接着我们来实现我们的删除功能，我们首先找到我们的删除功能绑定的对应事件的代码

```
<el-button type="danger" size="mini" @click="handleDelete(scope.row)">删除</el-button>
```

我们可以看到，我们的删除功能绑定了handleDelete的事件，并且将要删除的该行的数据都传入到事件中了，该对象中有该行的各种数据

然后我们进入删除方法的代码，写入其代码如下

```
// 删除
handleDelete(row) {
    this.$confirm("此操作永久删除当前信息，是否继续？","提示",{type:"info"}).then(()=>{
        axios.delete("/books",row.id).then((res)=>{
            if(res.data.flag){
                this.$message.success("删除成功");
            }else{
                this.$message.error("删除失败");
            }
        }).finally(()=>{
            //2.重新加载数据
            this.getAll();
        });
    }).catch(()=>{
        this.$message.info("取消操作");
    });
},

```

我们这里先进行一个确定，让用户进行一个确定选择，如果用户选择是，我们再执行后面的操作，也就是正式的删除操作，我们的删除操作需要传入要删除的行的id进去，用户进行删除，后面同样是进行数据的加载和取消，这些重复的代码就不提了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE92fac47d48806a040aaf69568df0c1c7.png)

## 修改功能

接着我们来将我们的修改功能，我们首先来看看我们的修改功能绑定的事件，可以看到我们的修改功能绑定的事件是handleUpdate，同时其也向该函数中传入了当前的行对象

```
<el-button type="primary" size="mini" @click="handleUpdate(scope.row)">编辑</el-button>
```

然后我们可以写入其对应的代码如下，我们这里首先发送我们的异步请求，然后我们对返回的值进行了判断（因为可能存在另一个人先删除了数据的情况），然后如果其返回的flag为真且返回的数据不为空时，我们才执行编辑操作，之所以需要一个并的条件，是因为我们查询的语句如果查询不到对应的结果，那么就会返回空数据，但是查询过程本身是执行完毕的，只是没有数据而已，所以我们这里单纯去判断查询方法的成功是不行的，还需要将其返回的数据一起判断

```
//弹出编辑窗口
handleUpdate(row) {
    //发送异步请求
    axios.get("/books",row.id).then((res)=>{
        if(res.data.flag && res.data.data != null){
            this.dialogFormVisible4Edit = true;
            this.formData = res.data.data;
        }else {
            this.$message.error("数据同步失败，自动刷新");
        }
    }).finally(()=>{
        //2.重新加载数据
        this.getAll();
    });
},
```

然后如果其为真，我们就立刻打开编辑的窗口，接着将对应的响应的数据赋予到编辑窗口上，这里我们的编辑窗口的数据对象和添加窗口的数据对象是一样的，都是叫formData，这点其实也很好理解，能复用的东西就不新整一个，要不然不方便。然后我们同样也是让其总是会重新加载我们的数据

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0d0aac4ee0753669f90dae48749a28b9.png)

然后我们来实现我们的修改操作，修改操作绑定的世界是handleEdit，这个函数绑定的是put操作，我们往里面传入我们的数据，接着我们获得其返回对象，同样是进行对应的判断然后是加载数据，没了

```
//修改
handleEdit() {
    axios.put("/books",this.formData).then((res)=>{
        //判断当前操作是否成功
        if(res.data.flag){
            //1.关闭弹层
            this.dialogFormVisible4Edit = false;
            this.$message.success("修改成功");
        }else{
            this.$message.error("修改失败");
        }
    }).finally(()=>{
        //2.重新加载数据
        this.getAll();
    });
},
```

还有一点值得一提的是，我们点击取消的时候当然也需要将我们的编辑窗口也取消，这里我们就我们的取消代码将两个窗口都关闭就可以了，代码上这样写实际产生的效果里不会有什么问题，我们这么方便就这么写就行了

```
//取消
cancel(){
    this.dialogFormVisible = false;
    this.dialogFormVisible4Edit = false;
    this.$message.info("当前操作取消");
},
```

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd0ad5e72a7fd5f12ff7dfaf958becd7c.png)

## 异常消息处理

此时我们还有事情没有做完，就是对异常消息的统一处理，我们在SpringMVC中学过，即使是抛出异常，我们也需要让我们返回给前台的格式保持统一，因此我们需要将我们的异常格式也进行统一

那么我们可以将我们的统一格式类里加一个String属性用于添加异常信息，然后提供其对应的构造方法，接着我们利用注解来构造我们的异常类并进行对应的处理，我们可以写入我们的代码如下

```
package com.itheima.controller.utils;

import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

//作为Springmvc的异常处理器
@RestControllerAdvice
public class ProjectExceptionAdvice {

    //拦截所有的异常信息
    @ExceptionHandler
    public R doException(Exception e){
        //记录日志
        //通知运维
        //通知开发
        e.printStackTrace();
        return new R("服务器故障，请稍微再试");
    }
}
```

可以看到我们这里使用RestControllerAdvice令该类成为我们的异常处理类，这一节的知识如果遗忘了，可以回去看SpringMVC的笔记进行对应的复习。然后我们获得对应的异常处理对象之后将该异常信息打印到后台，然后返回前台一个告知的数据，这里要注意，一定要记住将对应的异常信息进行打印，要不然我们都看到有什么异常，那就完犊子了

然后我们返回给前台的错误提示的信息也不该是我们自己在页面上写的代码，而是我们在异常处理类里写的代码，因此我们可以将其代码修改如下

```
this.$message.error(res.data.msg);
```

接着我们要注意一件事，我们这里成功的时候是通过页面发送的成功信息，而失败的时候则是通过异常类发送的，这里前者是前端，后者是后端，这样一下子前端处理一下子后端处理是不好的，我们最好将其统一。统一的方式一般而言有两种，第一种是全部统一到前端，第二种是全部统一到后端，我们这里选择后者

那么我们就要让我们的后端返回的信息里要不但要返回执行成功的信息，还要返回执行失败的信息，那么我们可以将表现层的代码修改如下

```
@PostMapping
public R save(@RequestBody Book book){
    boolean flag = bookService.save(book);
    return new R(flag,flag ? "添加成功" : "添加失败");
}
```

可以看到我们这里先获得对应的结果，然后结合三目运算符来实现我们的需求，令其返回含有对应信息的对象，然后我们在页面中就全部返回响应对象里的这个字符串数据就可以了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE82ca3b01b4a8894b2e19443480f60ecd.png)

## 分页

接着我们来制作我们的分页内容，首先我们我们的分页内容，其实简单来说，就是我们的页面一开始展示的就是我们分页查询的数据，这样就可以实现分页查询的效果了。

我们先来看看我们的分页查询的代码

```
<!--分页组件-->
<div class="pagination-container">

    <el-pagination
            class="pagiantion"

            @current-change="handleCurrentChange"

            :current-page="pagination.currentPage"

            :page-size="pagination.pageSize"

            layout="total, prev, pager, next, jumper"

            :total="pagination.total">

    </el-pagination>

</div>
```

可以看到上面有当前页码、数据总量等值，这些都是我们以后要用的，所以我们现在先进行对应的了解

```
pagination: {//分页相关模型数据
    currentPage: 1,//当前页码
    pageSize:10,//每页显示的记录数
    total:0//总记录数
}
```

同时我们可以看到我们在对应Vue的构造位置里也先行设定了其初始值

然后我们直接对我们的查询所有的方法进行改造，将其改造为一个分页查询的代码，那么我们可以将代码修改如下

```
//分页查询
getAll() {
    //发送异步请求
    axios.get("/books"+this.pagination.currentPage+"/+"+this.pagination.pageSize).then((res)=>{
        this.pagination.pageSize = res.data.data.size;
        this.pagination.currentPage = res.data.data.current;
        this.pagination.total = res.data.data.total;
        this.dataList = res.data.data.records;
    });
},
```

我们可以看到我们这里分页查询的代码是进行了一个字符串的拼接，发送的请求是我们要定位的页码和总页码数量，然后我们获得的响应对象里，我们将其查询到的分页结果赋我们这里设置的分页属性，然后最后将分页查询到的数据的具体内容赋给我们的展示的位置

然后我们要来设置我们的切换页码的函数

```
//切换页码
handleCurrentChange(currentPage) {
    //修改页码值为当前选中的页码值
    this.pagination.currentPage = currentPage;
    //执行查询
    this.getAll();
},
```

我们这里进行的设置很简单，就是对页码值进行一个修改，然后再次执行一个查询所有的函数，这个很好理解。

最后我们还要解决一个bug，那就是我们的目前的分页情况是，如果用户删除最后一页的最后一个数据，那么就会出现没有数据的情况。之所以会出现这样的情况，是因为我们的总页码此时只有2个了，而用户进行的查询却是从3那边查询的，相当于只有两页的数据，而用户却要查看第三页的数据，那自然是什么都没有了。

```
@GetMapping("{currentPage}/{pageSize}")
public R getPage(@PathVariable int currentPage,@PathVariable int pageSize){
    IPage<Book> page = bookService.getPage(currentPage, pageSize);
    //如果当前页码值大于总页码值，那么重新执行查询操作，使用最大页码值作为当前页码值
    if(currentPage>page.getPages()){
        page = bookService.getPage((int)page.getPages(),pageSize);
    }
    return new R(true,page);
}
```

解决这个问题的方法也很简单，我们直接在表现层里进行一个排查，如果当前页码比总页码还大，那么我们就令其重新查询，并以最大页码值为当前页码值，再将结果返回，这样就能正确的获得最后一个结果了

## 条件查询

然后我们来实现我们SB基础篇的最后一个内容，条件查询。

首先我们来查看下我们的条件查询的数据，我们会发现条件查询的数据有，但是没有绑定什么东西，也没有事先将数据封装到一个对象中，所以我们要自己动手做，将我们的数据封装到某些属性中，这样我们自己才可以在后续的内容中使用他

```
pagination: {//分页相关模型数据
    currentPage: 1,//当前页码
    pageSize:10,//每页显示的记录数
    total:0, //总记录数
    type: "",
    name: "",
    description: ""
}
```

这里我们将我们的属性保存到我们自己定义的type、name、description中，我们将这些数据保存在分页相关的模型对象中，我们保存在这里是因为我们的这些东西总是在分页查询的时候用得到的，所以我们将其定义在此中，实际上定义到其他位置也是可以的，没什么影响。

然后我们将对应的数据绑定到我们的对应的代码位置处

```
<div class="filter-container">
    <el-input placeholder="图书类别" v-model="pagination.type" style="width: 200px;" class="filter-item"></el-input>
    <el-input placeholder="图书名称" v-model="pagination.name" style="width: 200px;" class="filter-item"></el-input>
    <el-input placeholder="图书描述" v-model="pagination.description" style="width: 200px;" class="filter-item"></el-input>
    <el-button @click="getAll()" class="dalfBut">查询</el-button>
    <el-button type="primary" class="butT" @click="handleCreate()">新建</el-button>
</div>
```

可以看到我们这里将对应的代码绑定到了我们的图书类别等具有输入标签的代码中，这样我们用户输入的数据就会被正确保存到我们的预先设定的属性中了

然后我们在我们的分页查询之前，我们先定义一个param参数，接着拼接对应的字符串，最后将该字符串传入到我们的地址中

```
//分页查询
getAll() {

    //组织参数，拼接url请求地址
    let param;
    param = "?type"+this.pagination.type;
    param += "&name"+this.pagination.name;
    param += "&description="+this.pagination.description;

    //发送异步请求
    axios.get("/books"+this.pagination.currentPage+"/+"+this.pagination.pageSize+param).then((res)=>{
```

这里我们值得一提的是，可能会出现用户啥也不传入的情况。此时我们的出现的问题就是，如果用户传入的数据为空，那么我们要不要将对应的数据作为数据传入？这其实传不传都行，不传的话他最多就是将数据传入为空而已，不会对我们的业务有什么实际的影响。实际上我们解决这个问题一般是使用swagger工具类，不过这是我们SB案例之后要学习的内容了，我们这里先按下不表、

然后我们进入到数据层中，先创建对应的新的数据层接口，然后实现该方法，我们往里面直接传入一个Book对象，其会自动将数据封装到该对象中，之所以其会这样，是因为在SpringMVC中，当我们传入参数和实体类的类名一致时，其会将数据自动注入到我们的参数中（这里如果忘了这些知识，可以自己去进行单独的复习，我们这里的内容实际上也是推测的，不保证对，因为我也忘得七七八八了）

```
@Override
public IPage<Book> getPage(int currentPage, int pageSize) {
    IPage page = new Page(currentPage,pageSize);
    bookDao.selectPage(page,null);
    return page;
}

@Override
public IPage<Book> getPage(int currentPage, int pageSize, Book book) {
    LambdaQueryWrapper<Book> lqw = new LambdaQueryWrapper<>();
    lqw.like(Strings.isNotEmpty(book.getType()),Book::getType,book.getType());
    lqw.like(Strings.isNotEmpty(book.getName()),Book::getName,book.getName());
    lqw.like(Strings.isNotEmpty(book.getDescription()),Book::getDescription,book.getDescription());
    IPage page = new Page(currentPage,pageSize);
    bookDao.selectPage(page,null);
    return page;
}
```

然后我们在其下利用LambdaQueryWrapper对象进行对应的模糊查询，然后我们这里设置的查询是，当其传入的参数不为空时我们再执行对应的查询条件，同样，忘记了这个对象怎么用的记得回去复习

最后我们经过测试会发现我们这个方法是可以实现的

# 基础篇总结

那么到这里我们的基础篇就正式完结了，我们最后来做一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE19b7ae069ff92246a7d762b8fd2f7652.png)

