- 通知顺序

接着我们来学习通知顺序，我们的通知可能有两种导致出现执行顺序的问题的情况，一种是同一种类型的多个通知同时执行，第二种是不同种通知的同时执行，而这两种情况都会出现顺序问题

顺序问题都是按照一个规则解决的，都是配置文件的顺序是什么，其执行的顺序就是什么。同时我这里的执行顺序都是说不发生异常的情况下的，所以抛出异常后通知这里我们就不作考虑了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb06fa9e506a16e4c66d144cc61d71e72.png)

- 通知中获取参数

我们接着来学习如何在通知中获取我们方法的参数，我们可以在我们的通知上传入一个JoinPoint对象，调用其中的对应的方法，就可以得到其传入的参数了，其返回的对象会是一个object类型的数组

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8c2d6b014a8b74faf9cc1e86d9cc0fbd.png)

接着我们来讲获取参数的第二种方式，这种方式比较麻烦，存在强绑定，所以我们一般不会使用这种方式，我们了解下就差不多得了。这种方式是在对应的通知类中也写入对应的参数，然后在配置文件中使用逻辑运算与&&（当然，要进行转义），配合args参数，在括号内填入对应的在通知里写入的参数名，然后其就能进行一个正确的给对应的通知类的参数赋值的动作。同时再这个配置文件下还有一个arg-names的属性，该属性可以改变参数转入通知的顺序，使用这个参数的话，我们最开始写的参数最先将值赋予到同名的参数中，然后按顺序赋予到通知的参数里。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE56bc444ad8b0d60a217fa2653a353950.png)

- 通知中获取返回值

在讲解本章内容之前，我们要知道，只有能够准确保证每次运行都能获得返回值的通知，我们才会有获得返回值的操作。这里只有两种通知能稳定获得返回值，一种是返回后通知，另一种是回绕通知

我们一个获取返回值的方式是在对应的通知类中写入传入的参数，类型为Object，当然实际上写其他的也可以，但是如果到时候出现类型不匹配的问题就寄了，所以我们这里推荐的还是Object。然后在对应的原始方法里要令其返回其传入的值。然后在对应的AOP配置中，我们需要通过属性returning写入与通知中设定的参数名一样的名字，其代表会将返回值赋予到此参数中，然后我们就可以 打印这个参数了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE4bae82acd9de7f615a59f600a7d0f240.png)

而环绕通知获取参数就比较简单了，直接通过ProceedingJoinPoint对象获取就完了，执行该对象是会获得一个Object的类型的返回值的，我们直接获取该返回值就完了，同时不要忘了最后要需要将该返回值返回。同时我们这里规定回绕通知的返回值一定要是Object，如果我们的返回类型就void，那么就返回null就可以了

别忘了在对应的配置文件里设置对应的returning属性并给其命名

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE70287f94dc45421fec359a1857ac043e.png)

- 通知中获取异常对象

首先同样的，我们要确定一定能够获得异常的通知（如果发生异常的话），这两个通知分别是返回异常后通知和环绕通知

返回异常后通知要获取其异常对象的话，同样是要在对应的方法传入参数中定义一个异常对象，在配置文件里调用throwing属性，写入通知中定义的变量名，就可以让我们的异常对象正确传入

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd427a87e73386957a1e33f08680fa8e1.png)

至于环绕通知就直接通过对应的传入对象直接进行一个trycatch就能获得了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE4ebf01fffa3c378dc11f031e363341b9.png)

- 注解配置AOP

我们之前学习的是配置文件配置AOP，现在我们来学习使用注解配置AOP，要使用注解配置，首先要开启我们的注解驱动支持，因此我们要在对应的配置上先写入对应的头文件，然后原始功能类直接通过注解来注入，接着spring要抽取功能类也用注解来进行注入，接着是进行aop的具体配置，第一个aop:config不需要任何注解，因为其没有实际意义。接着是配置切面的注解（也就是设定那个类是我们的专门存放我们的具体通知的注解），我们想要配置切面直接就把@Aspect注解放在对应的功能类上就完了，然后配置切入点，切入点需要对应的格式，注解中采取的方法是直接定义一个方法，方法名就作为id，而上面加入@Pointcut注解，注解的括号内填入的是我们的对应格式。最后是配置通知与切入点之间的关系，这里我们采用@Before注解，注解的括号内就填入我们的之前定义的指定的切入点的id，这里和配置文件不同的是多了括号，然后Before代表的是前置通知，实际上还有多种通知方式，该注解下的方法就是我们所抽取出来的共性功能。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE1e5d1d2f7749fcc5b35fdffc4ed2a8f6.png)

接着我们就可以来实现我们的案例了，首先我们在注解文件中加入对应的扫描路径以及注解驱动

```
<!--注解扫描路径的配置-->
<context:component-scan base-package="com.itheima"/>
<!--AOP注解的驱动配置-->
<aop:aspectj-autoproxy/>
```

然后我们创建对应的调用层类并写入代码如下

```
package com.itheima.service.impl;

import com.itheima.service.UserService;
import org.springframework.stereotype.Service;

@Service("userService")
public class UserServiceImpl implements UserService {
    @Override
    public int save(int i, int m) {
        System.out.println("user service running..."+i+","+m);
        return 100;
    }
}

```

接着我们创建对应的共性功能AOP类并写入代码如下

```
package com.itheima.aop;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;

@Component
@Aspect
public class AOPAdvice {

    @Pointcut("execution(* *..*(..))")
    public void pt(){}

    @Before("pt()")
    public void before(){
        System.out.println("前置before...");
    }

    @After("pt()")
    public void after(){
        System.out.println("后置before...");
    }

    @AfterReturning("pt()")
    public void afterReturing(){
        System.out.println("返回后afterReturing...");
    }

    @AfterThrowing("pt()")
    public void afterThrowing(){
        System.out.println("抛出异常后afterThrowing...");
    }

    @Around("pt()")
    public Object around(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("环绕前around before...");
        Object ret = pjp.proceed();
        System.out.println("环绕后around after...");
        return ret;
    }

}

```

可以看到我们这里先定义了一个切点，然后我们对该切点进行对应的配置，接着我们只要在对应的方法里定义对应的注解就完了，然后在最上面加上对应的Component注解和Aspect注解就完了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEea421d99bb8ff8eed17f555fbb9d1f1b.png)

最后我们需要注意的是，我们的切入点不可以定义为一个抽象方法，即使里面没有实际内容。然后我们引入切入点是不可以省略后面的()，最后我们的切入点如果定义在当前类中，那么就只能在当前类中使用，如果想引用其他类中定义的切点，那么就要使用类型.方法名()的方式来引用。最后我们可以再通知类型注解后添加参数，实现XML配置中的属性，但是同样要引入切点，而且这里引入切点的属性名为value，而非id（这说明不引入其他属性的时候我们是将value属性给省略了，但实际赋值仍然是赋值到value中），值得一提的是，方法也要做对应的改动，否则会报错

- 注解AOP执行顺序控制

之前我们讲过在XML文件下的通知执行顺序的问题，本节我们来讲解在注解请看下的顺序控制

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE552f0dd921a99e12bfbb18099e2b5cb9.png)

首先在注解请看下，通知执行的顺序是不由顺序决定的，而是以通知类型的方法名排序为准，如果是在不同类中，则是以类名排序为准，同时使用Order注解也可以变更bean的加载顺序来改变通知的加载顺序。最后是一些企业开发的知识，有兴趣的可以自己看

- AOP注解驱动

接着最后我们来实现AOP的纯注解驱动实现我们的案例，其实这个案例要实现的功能也就是通过注解来实现我们的Junit

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb7b37edcac26ff3698f1b7e0066348d2.png)

首先我们要引入对应的Junit依赖及其在Spring中的Test依赖

```
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.1.9.RELEASE</version>
</dependency>
```

然后我们要创建一个对应的主注解配置类，在主配置中我们写入经典的固定头文件配置的注解，以及注解扫描的注解，但是这里还需要一个注解就是@EnableAspectJAutoProxy注解，我们的核心注解类中有了这个注解才能正确加载AOP的注解

```
package com.itheima.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

@Configuration
@ComponentScan("com.itheima")
@EnableAspectJAutoProxy
public class SpringConfig {
}

```

最后我们在对应的测试包里进行测试就完了，测试代码如下

```
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfig.class)
public class UserServiceTest {
    @Autowired
    private UserService userService;

    @Test
    public void testSave(){
        int ret = userService.save(888,666);
        Assert.assertEquals(100,ret);
    }
}
```

- 综合案例-业务层接口性能监控案例

那么学习完了所有的内容之后，现在我们来做一个综合案例来加深我们的理解，首先，我们案例的所需要的效果是，对业务层接口的每一个性能进行一个监控

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE55b78667135f75fbfdd892595f680979.png)

我们要测量接口执行的效率，我们的一个简单想法就是获取接口执行前后的执行时间，然后求出执行时长，这里我们最好用环绕通知，然后利用AOP思想动态植入我们的代码，时间我们可以通过proceed()方法来获取

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe6901c27697be9f496218ea940f81ebf.png)

最后我们来看看我们的步骤，注意我们的定义切入点是要绑定到接口上的，而不是在接口实现类上的，如果我们绑定在实现类上，那以后我们换一套实现类就寄了，这怎么行呢，所以我们要我们的切入点绑定到接口上

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE9598a01050b19e55d720788fe04495dc.png)

那么接着我们就要进行一个实际数据的查询，并且测试其性能，那么首先我们构造用于查询到对应的数据的并将其封装为对象的类

```
package com.itheima.domain;

import java.io.Serializable;

public class Account implements Serializable {

    private Integer id;
    private String name;
    private Double money;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Double getMoney() {
        return money;
    }

    public void setMoney(Double money) {
        this.money = money;
    }

    @Override
    public String toString() {
        return "Account{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", money=" + money +
                '}';
    }
}

```

然后我们定义Dao层的接口，接口主要提供查询方法，利用注解实现

```
package com.itheima.dao;

import com.itheima.domain.Account;
import org.apache.ibatis.annotations.Delete;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Select;
import org.apache.ibatis.annotations.Update;
import org.mybatis.spring.SqlSessionFactoryBean;

import java.util.List;

public interface AccountDao {

    @Insert("insert into account(name,money)values(#{name},#{money})")
    void save(Account account);

    @Delete("delete from account where id = #{id} ")
    void delete(Integer id);

    @Update("update account set name = #{name} , money = #{money} where id = #{id} ")
    void update(Account account);

    @Select("select * from account")
    List<Account> findAll();

    @Select("select * from account where id = #{id} ")
    Account findById(Integer id);
}
```

然后定义service层的接口

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

然后具体定义其实现

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

    public void setAccountDao(AccountDao accountDao) {

    }
}

```

搞定了基本内容之后，我们要搞定连接功能，首先我们当然是要导入对应的依赖，请看依赖的具体配置

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>SpringAZ</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.4</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>

        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.16</version>
        </dependency>

        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.3</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>1.3.0</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>

    </dependencies>

</project>
```

上面除了连接池的依赖之外还包含了注解的相关依赖，我们首先配置jdbc对应的连接代码

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://175.178.114.158:3306/spring_db
jdbc.username=root
jdbc.password=itheima
```

然后我们配置具体的连接JDBC的配置类

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
    private String userName;
    @Value("${jdbc.password}")
    private String password;

    @Bean("dataSource")
    public DataSource getDataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName(driver);
        ds.setUrl(url);
        ds.setUsername(userName);
        ds.setPassword(password);
        return ds;
    }
}

```

接着我们创建获取连接池和设置执行对应的查询方法的类

```
package com.itheima.config;

import org.mybatis.spring.SqlSessionFactoryBean;
import org.mybatis.spring.mapper.MapperScannerConfigurer;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;

import javax.sql.DataSource;

public class MyBatisConfig {

    @Bean
    public SqlSessionFactoryBean getSqlSessionFactoryBean(@Autowired DataSource dataSource){
        SqlSessionFactoryBean ssfb = new SqlSessionFactoryBean();
        ssfb.setTypeAliasesPackage("com.itheima.domain");
        ssfb.setDataSource(dataSource);
        return ssfb;
    }

    @Bean
    public MapperScannerConfigurer getMapperScannerConfigurer(){
        MapperScannerConfigurer msc = new MapperScannerConfigurer();
        msc.setBasePackage("com.itheima.dao");
        return msc;
    }

}

```

最后我们设置核心的注解配置文件，将需要的参数一并传入

```
package com.itheima.config;


import org.springframework.context.annotation.*;

@Configuration
@ComponentScan("com.itheima")
@PropertySource("classpath:jdbc.properties")
@Import({JDBCConfig.class,MyBatisConfig.class})
@EnableAspectJAutoProxy
public class SpringConfig {
}

```

最后我们进入对应的测试类里写入对应的代码并进行测试

```
package com.itheima.service;

import com.itheima.config.SpringConfig;
import com.itheima.domain.Account;
import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.EnableAspectJAutoProxy;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

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
    }

    @Test
    public void testFindAll(){
        List<Account> list = accountService.findAll();
        System.out.println(list);
    }

}

```

实际测试发现没有问题，这就说明我们的综合案例已经制作成功了，我们这里采用的是纯注解的方式。这里值得一提的是，一般的企业开发里都不会做一次的查询，都是做许多次的，然后会设置一个阈值，超过这个阈值的方法就会去特别查看，没超过的就不看，就这么简单

更多企业开发的细节可以看下图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE67f0a53defdb8816ad8eb0f630bb82cb.png)

