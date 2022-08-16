- 读取ymal的全部数据

如果我们在配置中写了很多的数据，那么我们在单独获取的时候代码量就会增加很多，此时我们有一种方式可以获得所有的数据，并将其封装到一个对象中，SpringBoot给我们提供了这么一个对象，这个对象就是Environment对象，要使用该对象，就要使用Autowired注解，令其自动获得我们的对象

```
//使用自动装配将所有的数据封装到一个对象Environment中
@Autowired
private Environment env;
```

如果要获取里面的数据，就要调用其中的getProperty方法，传入对应的数据的名称的字符串就可以获得了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc3d3e74fa65764b1f0ce3c69a322b453.png)

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf58005298c74fa31dea2fa1e4edbf23e.png)

经过了这节的学习我们也能够理解为什么我们的这里的配置的数据总是上下且带着空格隔开的形式了，上面就代表一个对象，而下面的内容则是上面对象的属性，这个性质对其下的属性也是适用的

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb7ebc0dc45c24737552a41f38b49a404.png)

注意，第一个获取全部对象的方式可以忘记，但是这个不行，这个方法非常重要，因为我现在大多都是使用这个方法来获取到我们的对应的数据的，也是这样来存储数据的

- SpringBoot整合Junit

接着我们来学习SpringBoot整合第三方技术，首先我们来学习整合Junit，先来看看步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEacf3b5d97b75e0f813b4fc7bbe0027a8.png)

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE60c706712e68a7a9c45c5253563e13d5.png)

这是因为我们的测试类默认的注解SpringBootTest，其会在其当前包或者其父包上寻找SpringBootConfiguration注解，如果我们一开始就将这个类的地址放到比我们的引导类还要高的层级上，那么其肯定找不到，那当然会报错。有的同学可能会说，你这引导类里的注解是SpringBootApplication啊，你这样也不是SpringBootConfiguration啊，其实这是因为前者的注解下包含了后者的注解而已，虽然看不见但是的确是有的

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb754f3e20e3d47e0dadab3aea03aafca.png)

想要解决这个问题有两种方式，一种方式是往我们的SpringBootTest中直接传入我们的引导类的字节码文件，另一种方式将其设置为其子级，这个很好理解，就不多提了。其实还是第三种方法，就是使用ContextConfiguration注解然后传入我们的引导类的class文件，不过这个纯属是脱裤子放屁，知道就行了

接着我们来继续学习SB的基础篇，本次我们来学习使用SB整合各种第三方技术

- SpringBoot整合MyBatis

我们先来看看我们整合MyBatis要做的事情

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe2269f82a68c679f03993cfe58797e4d.png)

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8c030971aadd063763cf04996a140e22.png)

最后我们进入测试类进行一个测试就完了，发现的确可以正确获得其数据，那就行了。

然而如果我们的SpringBoot版本比较低的话，那么我们的这个程序就会报出连接错误的异常。这其实是因为我们的SB版本降低导致我们的MySQL版本改为8.X，其会强制要求设置时区，因为我们没有设置时区，所以会报错

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE133067a01a24ea364ad1fbcdb8c3a651.png)

有两种解决方式，一种是修改url，另一种是修改MySQL的数据库配置，我们这里只讲前一种，具体方式就是将代码修改如下就可以了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE3f9d03440054670c9696af44498bc03c.png)

- SpringBoot整合MyBatisPlus

为了这一节的内容我们可是回去学习了一天的MP的，突出一个痛苦不堪，现在学成归来，我们速速将这一节过了。我们先来看看步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf812cf6254370c89a84debf7eab2d1ec.png)

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

- SpringBoot整合Druid

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE175a1549ef76baf10074f75ef34e7875.png)

最后整合了这么多技术，相信各位也从里面看出门路来了，其经典流程其实就下面两步

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc34578b04c39829cb15709d958640404.png)

- SSMP整合案例制作分析

接着我们来讲我们的SSMP的整合案例，其实也就是我们的SpringBoot整合我们的MyBatisPlus以及SpringMVC等技术，先来看看其步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa2b5104222761dbcae73030bbd44bbd4.png)

接着我们先来创建我们的对应的模块，导入对应的坐标，然后引入MP和Druid的坐标，修改配置文件的格式，然后设置端口号

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc2a478d53937647e7f3f8f36f7a43da1.png)

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE79e5828b0e18bb8c31b34fcfda6117db.png)

接着我们来开启MyBatisPlus的调试日志，开启这个可以让我们看到操作执行过程的所有信息

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE0d0765ad39adbcc819d8b0257442a1af.png)

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb75c6ec0aa134c229b9b7a4f1fc59968.png)



![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE97de0d18f4cafb4dacd27095a9db33fc.png)

接着我们来学习业务层的功能，其实也就是多定义一个和实现类的事，这里值得一提的是，我们数据层的方法是非常直接的和以和数据打交道的名字来命名的，而在业务层中则是以业务功能来命名的，这是规范。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE212688a9670e693597024dddb020570e.png)

这里的代码就不传上来了，我是真的嫌麻烦，其也就是做了一个业务层调用数据层的动作而已，其他的没什么

然后我们来学习快速开发Service层的方式，其实这个方式就是MP章节中最后介绍的快速生成的Service的继承类，我们当时提了提，这里我们就认真学学

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa7dfdb808f8d9d874fd4bb5ca1b81732.png)

其接口继承的类是Iserivce，该类类似于BaseMapper，继承该类可以让我们的类获得各种对应的业务层的方法，然后我们要写入其实现类，如果我们写入其实现类的话，就得实现其下的所有抽象方法，那也很麻烦，所以我们可以在其中再继承一个ServiceImpl的类，传入我们的数据层的接口对象和Book实体类对象，就可以自动帮我们生成这些方法

```
@Service
public class BookServiceImpl1 extends ServiceImpl<BookDao, Book> implements IbookService {
}
```

然后经过实际测试我们会发现这个方法是可以使用的，只不过其方法名和我们的有些不同就是了。这里要提一点的是，这里自动生成的分页方法是要我们自己先创建一个分页对象然后传进去的，跟我们之前自己定义的直接传入对应的分页参数就可以运行的方法是不同的

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb875f7d8778e469fedf24bb101e8dc5a.png)

最后我们值得一提的是，如果说其中的提供的方法无法满足我们的业务需求，我们可以自己在接口中定义新的方法来满足我们的需求，注意不要覆盖原始的方法，否则会造成原始提供的功能丢失，我们可以通过Overwrite注解来判断是否对原始方法进行了重写

- 表现层数据开发

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEeeb486ee4110783f329e143880610c04.png)

最后我们来复习下RequestBody注解和PathVariable注解的不同，前者表示接受一个对象，而后者则是接受具体的值，如果我们接受的内容只是一两个值，那么我们建议用后者，其代表从路径中直接获取对应的数据，如果是一个对象，那么就要用前者，其代表的意思是接受传送过来的json数据，并将其转换为所需的对象（当然，我们传过来的内容是通过post封装成json数据传过来的）