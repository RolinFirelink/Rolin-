现在我们来正式学习我们的Swagger，首先我们先来介绍下Swagger的由来

# Swagger简介

这个简单来说就是我们的现在的时代都是前端和后端是分离开来的，这里会有两个团队，分别是前端团队和后端团队，而前端和后端的功能是分开的，是松耦合的形式的。即使没有后端，前端自己也是可以独立跑起来的

而我们的前后端甚至可以部署在不同的服务器上，一般来说，我们是前端用于向用户提供服务，而后端则是向前端提供对应的数据。这里我们的前端获取后端数据的方式一般是通过后端提供的对应的接口来获取的，而前端则会通过利用postman等工具来测试后端提供的接口。这其实也就是我们常说的前后端分离

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1d0436942134c4141b64e1d5f8ea8212.png)

但是这个时候可能就会出现一个沟通上的问题，就是我们的前端和后端的人员怎么知道他们的前后端是正好能合适的呢？早期为了解决这个问题一般会制定对应的计划文档并实时更新，但是对于一些更加大型的项目来说，这也不好用。此时我们的Swagger就应用而生，其就是为了解决前后端的联调问题的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEedc2964600fa6c0ac14bdd54b2e491df.png)

其本身是一个RestFul的Api文档的在线自动生成工具，支持多种语言且可以直接运行，并在线测试我们的API接口，这样前后端都只使用这一个软件就可以解决其联调问题了

最后我们值得一提的是，如果我们想要在项目下使用Swagger，那么我们就要导入对应的jar包，其中以swagger2和swaggerui的jar包最为重要，这两个是我们的核心jar包

# SpringBoot集成Swagger

接着我们来正式学习如何在我们的SpringBoot中集成Swagger，首先我们要引入对应的坐标的依赖，其代码如下

```
    <dependency>
        <groupId>io.springfox</groupId>
        <artifactId>springfox-swagger2</artifactId>
        <version>2.9.2</version>
    </dependency>

    <dependency>
        <groupId>io.springfox</groupId>
        <artifactId>springfox-swagger-ui</artifactId>
        <version>2.9.2</version>
    </dependency>
</dependencies>
```

这引入需要等待的时间还是蛮久的，说明里面需要引用的jar包还是挺多的。另外这里还有一点值得一提的是，由于我们的课程是2019年的，因此我们的springboot的版本也不能太新，我们要用2.5.6的版本，这个版本我们才可以正确使用的，如果用太新的版本，那就会无法启动我们的服务器

然后我们引入对应的jar包之后还需要对我们的swagger进行集成，也就是将我们的swagger在我们启动服务器的时候跟着启动，我们可以创建对应的Swagger配置类并写入其代码如下

```
package com.itheima.config;

import org.springframework.context.annotation.Configuration;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
@EnableSwagger2  //开启Swagger2
public class SwaggerConfig {
}

```

我们这里的Configuratino注解就可以理解为是Component注解，但其不完全相同，具体有啥区别，我也忘了。有时间去复习或者直接百度看看到底有啥不同吧

当这些配置完成之后，我们的swagger服务就启动了，我们进入其对应的网址http://localhost:8080/swagger-ui.html，就可以查看到如下界面

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1016652b1b5255d332d8981b83dc2d1e.png)

我们可以将界面分为四部分，如上图所示，我们接下来就直接开始做我们的案例，这里我们要经常查看源码，因为这玩意没啥正经教程，太简单了，所以咱们要学那还得看源码啊

# 配置Swagger

首先我们要搞清楚，我们之前做的是集成，而不是配置，那么我们现在就要来正式配置我们的Swagger了。我们首先要知道我们的Swagger的bean实例的对象是Docket，所以我们首先定义一个方法，其返回值为Docket，然后其上写入Bean注解，我们先简单整一个令其返回一个新的Docket对象，但是我们会发现其构造方法里会需要传入参数，我们点进去里面的源码里去看看

点进去源码，可以看到我们的这个类是实现了DocumentationPlugin分页组件的，然后其下的第一个成员变量就是默认的成员变量，其字符串的内容为default，其实这个default也对应我们最开始进入对应的页面里的右上角的选组的第一个default

```
public class Docket implements DocumentationPlugin {
    public static final String DEFAULT_GROUP_NAME = "default";
```

然后我们往下拉，会看到这么一个构造方法

```java
public Docket(DocumentationType documentationType) {
    this.apiInfo = ApiInfo.DEFAULT;
    this.groupName = "default";
    this.enabled = true;
    this.genericsNamingStrategy = new DefaultGenericTypeNamingStrategy();
    this.applyDefaultResponseMessages = true;
    this.host = "";
    this.pathMapping = Optional.absent();
    this.apiSelector = ApiSelector.DEFAULT;
    this.enableUrlTemplating = false;
    this.vendorExtensions = Lists.newArrayList();
    this.documentationType = documentationType;
}
```

我们可以看到其下要先传入一个ApiInfo对象，我们点进去这个对象里面，可以看到里面有一个静态代码块，而且其属性变量里还有一个ApiInfo的成员变量

```
static {
    DEFAULT = new ApiInfo("Api Documentation", "Api Documentation", "1.0", "urn:tos", DEFAULT_CONTACT, "Apache 2.0", "http://www.apache.org/licenses/LICENSE-2.0", new ArrayList());
}
```

可以看到这个静态代码块就是给成员变量的对应属性赋予一个默认值的，也就是默认创建的对象，这里显示的放入的内容就会在我们的Swagger信息里进行显示，也就是我们的页面的左侧。

然后我们重新看我们的Docket的构造方法的源码，此时我们就理解了一切。我们可以看到我们这里第一个传入的是我们的左侧要显示的数据的对象，然后我们默认给其设置其默认组名为default，接着我们打开这个展示，令其变为true等等等等。而完成这些事情，只需要传入一个DocumentationType对象就可以了，然后我们再进去这个对象里看看源码

```
public class DocumentationType extends SimplePluginMetadata {
    public static final DocumentationType SWAGGER_12 = new DocumentationType("swagger", "1.2");
    public static final DocumentationType SWAGGER_2 = new DocumentationType("swagger", "2.0");
    public static final DocumentationType SPRING_WEB = new DocumentationType("spring-web", "1.0");
    private final MediaType mediaType;
```

可以看到里面有三个成员变量，分别对应的不同的swagger版本，显然，我们要使用的是2.0的版本，而且由于其是静态方法，我们直接点就可以使用了

所以我们可以想当然的往我们的Docket的构造方法里传入我们的该静态变量所创造出来的对应对象。但是此时，我们的ApiInfo使用的还是默认的，但我们要使用的肯定是希望用我们自己设定的，所以我们要自己创建一个ApiInfo对象然后将其设置到对应的对象中，我们可以使用链式方法直接在后面通过.的形式往里面传入我们的info对象

那么我们当然要首先构造出这个info对象，我们新定义一个方法，令这个方法返回ApiInfo对象就完了，这里我们直接拿这个对象中的静态代码块里的代码来用，new一个新的，往里面传入对应的数据，不过我们可以看到这里我们的DEAFULT_CONTACT是报红的，这是因为这里要传入一个对象，而在我们自己创建的配置类里是没有这个对象的，我们直接进入到ApiInfo中点进入该对象中，查看源码，可以看到这里要求传入三个字符串，分别是名字，地址和邮箱，其实这个就是作者的对应信息而已，我们按照其方法创建出来就好了

```
public class Contact {
    private final String name;
    private final String url;
    private final String email;
```

那么最终我们可以构造出我们的配置代码如下

```
package com.itheima.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.service.Contact;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

import java.util.ArrayList;

@Configuration
@EnableSwagger2  //开启Swagger2
public class SwaggerConfig {

    //配置了Swagger的Docket的bean实例
    @Bean
    public Docket docket(){
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo());
    }

    //配置Swagger的信息apiInfo
    private ApiInfo apiInfo(){

        //作者信息
        Contact contact = new Contact("Rolin","https://www.bilibili.com/","2259286198@qq.com");

        return new ApiInfo(
                "Rolin的SwaggerAPI文档",
                "千里之行，始于足下",
                "1.0",
                "https://space.bilibili.com/33169381?spm_id_from=333.1007.0.0",
                contact,
                "Apache 2.0",
                "http://www.apache.org/licenses/LICENSE-2.0",
                new ArrayList());
    }
}
```

我们这里就令其返回一个对应的Docket对象，只不过这个Docket对象是经过了我们自己的设置的。最后我们这里值得一提的是，我们的ApiInfo对象是没有提供set方法的，我们要创建对应的对象只能通过构造方法，所以即使我们什么都不想写，我们也得放点参数进去，要不然没法整出这个对象来

# Swagger配置扫描接口

 然后现在我们看我们的页面下的内容，可以看到我们这里有显示对应的不同请求（还有我们自己写的hello的请求），请求下还有各种请求方式，但是我们这里可以看到，不但我们的请求有，而且其他的请求，比如错误请求一类的也有，而这个错误请求其实并不是我们一开始想扫描到的，所以我们这里要学习我们自定义扫描包的路径的方法，这样我们就可以让我们的页面上只显示我们想要令其显示的请求了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE06a24debf48bfe84885eadd167b63ba0.png)

我们要设定我们的包，就需要使用到其对应的方法select()，这里同样需要使用链式的操作，我们首先在我们的Docket对象的后面写入.select()，可以看到里面需要一个build，那么我们就给其一个build，往下继续调用build()方法，调用了该方法之后我回去看我们的select()方法，就会发现其下只有paths和apis两个方法可以调用，我们先来看看apis方法，其需要传入一个RequestHandlerSelectors对象，该对象里可以指定我们的扫描接口的方式，我们先选择其下的basePackage方法，其就是指定包进行扫描的方法，往下只需要写入指定的要扫描的包的路径就可以了

那么我们可以将我们的代码改造如下

```
//配置了Swagger的Docket的bean实例
@Bean
public Docket docket(){
    return new Docket(DocumentationType.SWAGGER_2)
            .apiInfo(apiInfo())
            .select()
            .apis(RequestHandlerSelectors.basePackage("com.itheima.controller"))
            .build();
}
```

此时我们启动服务器就会发现，我们下面剩下的能扫描的包只剩下我们的自己的包了，此时就说明我们的自定义扫描就已经成功了

接着我们还有一些用于了解的内容，具体请看下面的注释，这里就不再赘述了

```
//RequestHandlerSelectors，配置要扫描接口的方式
//basePackage:指定要扫描的包
//any():扫描全部
//none():不扫描
//withClassAnnotation:扫描类上的注解，参数是一个注解的反射对象，其实就是注解的class文件
//withClassAnnotation:扫描方法上的注解，参数是一个注解的反射对象，其实就是注解的class文件
```

然后接着我们来讲下Paths，所谓的的pahts方法，其实就是一个过滤器，可以过滤掉一些不符合我们的条件的接口，令其不被扫描。那么假设我们写入代码如下

```
.apis(RequestHandlerSelectors.basePackage("com.itheima.controller"))
//paths()，过滤什么路径
.paths(PathSelectors.ant("/Rolin/**"))
.build();
```

我们这里调用了paths方法进行过滤，我们查看其源码会发现其括号内需要传入一个PathSelectors对象，我们调用其下的ant方法，通过该方法我们可以指定我们只需要哪些路径的方法，比如我们这里指定我们只要路径在Rolin下的方法，但实际上在我们自定义的类中，是没有这种类型的方法的，所以最后我们的结果一定是什么都得不到。同时该对象下还有ant()全不过滤以及none()全过滤的方法，这个作为了解就好了

# 配置是否启动Swagger

这个就更加简单了，还记得我们当初看Docket源码时的enabled成员属性吗？只要该属性为真，那么我们的Swagger就是启动的，如果为flase，那么直接就打不开了

我们可以直接在我们的apiInfo()方法下直接调用enable方法的属性，将该属性赋值为false，然后我们打开我们的服务器时，就会发现对应的页面已经无法访问了。最后提一嘴，我们的将其赋为false的链式方法也可以最后做，我测试过了，没什么影响

这里我们值得一提的是，我们的的页面地址是http://localhost:8080/swagger-ui.html，并不是其本来就是这样的，而是因为我们的页面实际就是预先放在对应的资源路径下，预先提供好给我们的，对于这点我们需要知其所以然

然后我们要完成一个需求，我们希望我们的Swagger在生成环境中展示，而在正式环境中不展示，那么我们应该要怎么办呢？

我们可以将我们的代码改造如下，首先我们需要让我们的方法接受一个Environment对象，然后我们调用这个对象的acceptsProfiles()方法，该方法可以判断我们当前的环境是否是我们所指定的环境，我们指定其环境需要传入一个Profiles对象，那我们就new这么一个对象，我们点进去该对象中查看其源码，可以发现其有Profiles.of方法，该方法可以指定多个字符串进去，简单来说该方法的作用就是可以返回一个Profiles对象，我们往里面指定的字符串都是我们希望其进行展示的环境

最后我们得到返回的布尔类型的结果，然后将结果赋予给其对应的是否启动Swagger的属性就可以了

```
//配置了Swagger的Docket的bean实例
@Bean
public Docket docket(Environment environment){

    //设置要显示的Swagger环境
    Profiles profiles = Profiles.of("dev","test");
    //通过environment.acceptsProfiles(profiles)
    boolean flag = environment.acceptsProfiles(profiles);

    return new Docket(DocumentationType.SWAGGER_2)
            .apiInfo(apiInfo())
            .enable(flag) //enable为是否启动Swagger，如果为False则不启动，反之则启动
            .select()
            .apis(RequestHandlerSelectors.basePackage("com.itheima.controller"))
            .build();
}
```

# 配置API文档的分组

接着我们来将API文档的分组，分组的方法很简单，就是直接利用链式方法调用其下的groupName()方法，然后往括号内传入我们要指定的字符串就行了，然后其就会正确在我们的对应的页面的右上方展示了，如果我们希望我们右上方的下拉框有多个分组的话，就只需要自己定义多个Bean，令其返回多个Docket就可以了，然后我们切换到对应的分组上就可以令其显示对应的数据了

# Model实体类的注入

接着我们来看看我们的页面里的Models框，该框下有我们后台定义的实体类，这个实体类里有对应的描述，那么我们要如何将我们的定义的实体类加入到这里来呢？其实我们根本就不用考虑这些有的没的，他都帮我们做好了，只要我们返回了实体类，那么该实体类就会被加载到Models中

接着还存在的问题是，我们虽然返回了对应的实体类，但是其下还没有任何注释，这样前端人员看到这个内容会一头雾水的。此时我们就需要Api的一类注释

```
package com.itheima.pojo;

import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;

//@Api(注释)
@ApiModel("用户实体类")
public class User {
    @ApiModelProperty("用户名")
    public String username;
    @ApiModelProperty("密码")
    public String password;
}
```

比如在我们上面的代码中，ApiModel是用于给我们的整个实体类加注释的，而ApiModelProperty则是给对应的属性加注释的，对应的注释直接写在小括号内就行了

讲完了Models之后，我们再来讲讲我们的方法的栏框。

具体请看下面的代码

```
package com.itheima.controller;

import com.itheima.pojo.User;
import io.swagger.annotations.ApiOperation;
import io.swagger.annotations.ApiParam;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

//Operation接口
@RestController
public class HelloController {

    //@RequestMapping("/hello")
    @GetMapping(value = "/hello")
    public String hello(){
        return "Hello World";
    }

    //只要我们的接口中，返回值中存在实体类，其就会被扫描到Swagger中
    @PostMapping(value = "/user")
    public User user(){
        return new User();
    }

    //Operation接口，不是放在类上，是方法上
    @ApiOperation("Hello控制类")
    @GetMapping(value = "/hello2")
    public String hello(@ApiParam("用户名") String username){
        return "hello"+username;
    }
}
```

比如我们这里使用了ApiOperation该注解，其可以给我们的对应的请求方法加上注释，注意其只能放到方法上，放到类上会失去效果

然后是ApiParam注解，该注解可以给我们的对应的形参加上注释。最后我们要提一点，如果我们的方法是指定了某一种请求方式的，那么在我们页面的请求框中就只会显示对应的请求方式，且只有一个，但如果我们指定其方法是RequestMapping注解的话，那么其就会给我们展示许多种请求的方式，包括但不限于get和post请求

最后我们在我们的对应的Swagger页面里面，我们可以做很多测试，非常方便，我们需要输入什么数据，其都会自动模拟用户的窗口给我们使用

最后我们来总结一下Swagger的好处

1. 后端程序员可以给Swagger一些比较难理解的属性或者接口添加注释信息

1. 接口文档是在线实时更新的，而且支持多人开发

1. 其支持非常方便快捷的在线测试

最后我们要注意的是，出于安全考虑，我们在我们的项目正式项目发布的时候，一定要关闭Swagger

