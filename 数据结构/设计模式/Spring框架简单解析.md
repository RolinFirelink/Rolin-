学完了所有内容之后，现在我们来学习如果用我们之前学过的设计模式来自定义Spring框架，当然这里只是一个非常简单的自定义Spring框架，主要还是复习一下

在自定义Spring框架前，我们先来回顾一下Spring框架的使用、

- 配置文件使用Spring管理类

首先我们要使用Spring管理类首先要引入Spring的依赖，其坐标如下

```
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context</artifactId>
  <version>5.2.0.RELEASE</version>
</dependency>
```

然后我们创建对应的三层架构，首先我们创建dao包，然后创建dao层接口，接着创建其实现类，只输出一句话

```
public class UserDaoImpl implements UserDao {
    @Override
    public void add() {
        System.out.println("UserDao...");
    }
}
```

同样创建服务层接口的实现类，其下拥有Dao类属性，我们提供set方法并调用Dao层的方法

```
public class UserServiceImpl implements UserService {

    //声明一个UserDao类型的变量
    private UserDao userDao;

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    @Override
    public void add() {
        System.out.println("UserService...");
        userDao.add();
    }
}

```

接着我们创建配置文件，名为applicationContext.xml（其实一般也就是这个名字），接着我们通过bean标签让我们的userService的值自动装备，并给其下的Dao属性配置property标签赋值

同理我们还做了userdao的对象的自动注入

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <bean id="userService" class="com.itheima.service.impl.UserServiceImpl">
        <property name="userDao" ref="userDao"></property>
    </bean>

    <bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"></bean>

</beans>
```

最后我们写入控制层的代码如下，我们这里首先获得spring的容器对象，然后获得通过传入类名和字节码文件获得容器中管理的对象，最后调用该对象的方法，可以在控制台上得到我们类运行的内容

```
public class UserController {
    public static void main(String[] args) {
        //1.创建spring的容器对象
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        //2.从容器对象中获取userService对象
        UserService userService = applicationContext.getBean("userService", UserService.class);
        //3.调用userService进行业务逻辑处理
        userService.add();
    }
}
```

此时我们的Spring管理就已经实现了，我们这里没有直接创建我们的类，但是我们却正确调用我们的类中的方法，我们的Spring帮我们自动创建并管理了这些类

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc00e1b55fed43543b7ad57896ad7c9c0.png)

- Spring核心功能结构

我们先来看看Spring核心容器的概述图

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE088037c38d3819f91ef6820cb540b480.png)

核心容器由 beans、core、context 和 expression（Spring Expression Language，SpEL）4个模块组成。

- spring-beans和spring-core模块是Spring框架的核心模块，包含了控制反转（Inversion of Control，IOC）和依赖注入（Dependency Injection，DI）。

- BeanFactory使用控制反转对应用程序的配置和依赖性规范与实际的应用程序代码进行了分离。BeanFactory属于延时加载，也就是说在实例化容器对象后并不会自动实例化Bean，只有当Bean被使用时，BeanFactory才会对该 Bean 进行实例化与依赖关系的装配。



- spring-context模块构架于核心模块之上，扩展了BeanFactory，为它添加了Bean生命周期控制、框架事件体系及资源加载透明化等功能。

- 该模块还提供了许多企业级支持，如邮件访问、远程访问、任务调度等，ApplicationContext 是该模块的核心接口，它的超类是 BeanFactory。与BeanFactory不同，ApplicationContext实例化后会自动对所有的单实例Bean进行实例化与依赖关系的装配，使之处于待用状态。



- spring-context-support模块是对Spring IoC容器及IoC子容器的扩展支持。



- spring-context-indexer模块是Spring的类管理组件和Classpath扫描组件。



- spring-expression 模块是统一表达式语言（EL）的扩展模块，可以查询、管理运行中的对象，同时也可以方便地调用对象方法，以及操作数组、集合等。它的语法类似于传统EL，但提供了额外的功能，最出色的要数函数调用和简单字符串的模板函数。EL的特性是基于Spring产品的需求而设计的，可以非常方便地同Spring IoC进行交互。

接下来我们先来学习下bean

- Bean概述

Spring 就是面向 Bean 的编程（BOP,Bean Oriented Programming），Bean 在 Spring 中处于核心地位。Bean对于Spring的意义就像Object对于OOP的意义一样，Spring中没有Bean也就没有Spring存在的意义。Spring IoC容器通过配置文件或者注解的方式来管理bean对象之间的依赖关系。

spring中bean用于对一个类进行封装。如下面的配置：

```
<bean id="userService" class="com.itheima.service.impl.UserServiceImpl">
<property name="userDao" ref="userDao"></property>
</bean>
<bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"></bean>
```

为什么Bean如此重要呢？

- spring 将bean对象交由一个叫IOC容器进行管理。

- bean对象之间的依赖关系在配置文件中体现，并由spring完成。

接着我们来学习bean里面的BeanFactory

- BeanFactory解析

Spring中Bean的创建是典型的工厂模式，这一系列的Bean工厂，即IoC容器，为开发者管理对象之间的依赖关系提供了很多便利和基础服务，在Spring中有许多IoC容器的实现供用户选择，其相互关系如下图所示

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEca117d12edff7584f364cded9e50bfd6.png)

其中，BeanFactory作为最顶层的一个接口，定义了IoC容器的基本功能规范，BeanFactory有三个重要的子接口：ListableBeanFactory、HierarchicalBeanFactory和AutowireCapableBeanFactory。但是从类图中我们可以发现最终的默认实现类是DefaultListableBeanFactory，它实现了所有的接口。

那么为何要定义这么多层次的接口呢？

每个接口都有它的使用场合，主要是为了区分在Spring内部操作过程中对象的传递和转化，对对象的数据访问所做的限制。例如，

- ListableBeanFactory接口表示这些Bean可列表化。



- HierarchicalBeanFactory表示这些Bean 是有继承关系的，也就是每个 Bean 可能有父 Bean



- AutowireCapableBeanFactory 接口定义Bean的自动装配规则。



这三个接口共同定义了Bean的集合、Bean之间的关系及Bean行为。最基本的IoC容器接口是BeanFactory，来看一下它的源码：

```
public interface BeanFactory {

    String FACTORY_BEAN_PREFIX = "&";

    //根据bean的名称获取IOC容器中的的bean对象
    Object getBean(String name) throws BeansException;
    //根据bean的名称获取IOC容器中的的bean对象，并指定获取到的bean对象的类型，这样我们使用时就不需要进行类型强转了
    <T> T getBean(String name, Class<T> requiredType) throws BeansException;
    Object getBean(String name, Object... args) throws BeansException;
    <T> T getBean(Class<T> requiredType) throws BeansException;
    <T> T getBean(Class<T> requiredType, Object... args) throws BeansException;

    <T> ObjectProvider<T> getBeanProvider(Class<T> requiredType);
    <T> ObjectProvider<T> getBeanProvider(ResolvableType requiredType);

    //判断容器中是否包含指定名称的bean对象
    boolean containsBean(String name);
    //根据bean的名称判断是否是单例
    boolean isSingleton(String name) throws NoSuchBeanDefinitionException;
    boolean isPrototype(String name) throws NoSuchBeanDefinitionException;
    boolean isTypeMatch(String name, ResolvableType typeToMatch) throws NoSuchBeanDefinitionException;
    boolean isTypeMatch(String name, Class<?> typeToMatch) throws NoSuchBeanDefinitionException;
    @Nullable
    Class<?> getType(String name) throws NoSuchBeanDefinitionException;
    String[] getAliases(String name);
}
```

在BeanFactory里只对IoC容器的基本行为做了定义，根本不关心你的Bean是如何定义及怎样加载的。正如我们只关心能从工厂里得到什么产品，不关心工厂是怎么生产这些产品的。

BeanFactory有一个很重要的子接口，就是ApplicationContext接口，该接口主要来规范容器中的bean对象是非延时加载，即在创建容器对象的时候就对象bean进行初始化，并存储到一个容器中。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE7f916f27b31948e12ccf7f8e84c52c42.png)

要知道工厂是如何产生对象的，我们需要看具体的IoC容器实现，Spring提供了许多IoC容器实现，比如：

- ClasspathXmlApplicationContext : 根据类路径加载xml配置文件，并创建IOC容器对象。



- FileSystemXmlApplicationContext ：根据系统路径加载xml配置文件，并创建IOC容器对象。



- AnnotationConfigApplicationContext ：加载注解类配置，并创建IOC容器。

然后我们来学习下BeanDefinition

- BeanDefinition解析

Spring IoC容器管理我们定义的各种Bean对象及其相互关系，而Bean对象在Spring实现中是以BeanDefinition来描述的，如下面配置文件

```
<bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"></bean>

bean标签还有很多属性：
scope、init-method、destory-method等。
```

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE68a15e48c6829375d0bc1cfc16264962.png)

简单来说，我们的bean中的标签都是被封装好的，而这些就是通过BeanDefinition封装的

接着我们来学习下BeanDefinitionReader

- BeanDefinitonReader解析

Bean的解析过程非常复杂，功能被分得很细，因为这里需要被扩展的地方很多，必须保证足够的灵活性，以应对可能的变化。Bean的解析主要就是对Spring配置文件的解析。这个解析过程主要通过BeanDefinitionReader来完成，看看Spring中BeanDefinitionReader的类结构图，如下图所示

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE66f86703ca5aefe0a8b863d108f215ab.png)

上面所说的内容简单来说其实就是我们的BeanDefinitionReader有多个子实现类，分别对应不同的解析情况

接着来看看看看BeanDefinitionReader接义的功能来理解它具体的作用：

```
public interface BeanDefinitionReader {

    //获取BeanDefinitionRegistry注册器对象
    BeanDefinitionRegistry getRegistry();

    @Nullable
    ResourceLoader getResourceLoader();

    @Nullable
    ClassLoader getBeanClassLoader();

    BeanNameGenerator getBeanNameGenerator();

    /*
        下面的loadBeanDefinitions都是加载bean定义，从指定的资源中
    */
    int loadBeanDefinitions(Resource resource) throws BeanDefinitionStoreException;
    int loadBeanDefinitions(Resource... resources) throws BeanDefinitionStoreException;
    int loadBeanDefinitions(String location) throws BeanDefinitionStoreException;
    int loadBeanDefinitions(String... locations) throws BeanDefinitionStoreException;
}
```

简单来说我们可以BeanDefinitionReader中都有可以获得注册器对象的方法，并且其下有加载对应的配置文件的方法

然后我们来学习BeanDefinitionRegistry

- BeanDefinitionRegistry解析

BeanDefinitionReader用来解析bean定义，并封装BeanDefinition对象，而我们定义的配置文件中定义了很多bean标签，所以就有一个问题，解析的BeanDefinition对象存储到哪儿？

答案就是BeanDefinition的注册中心，而该注册中心顶层接口就是BeanDefinitionRegistry

该接口定义了所有子实现类都必须具有的方法

```
public interface BeanDefinitionRegistry extends AliasRegistry {

    //往注册表中注册bean
    void registerBeanDefinition(String beanName, BeanDefinition beanDefinition)
            throws BeanDefinitionStoreException;

    //从注册表中删除指定名称的bean
    void removeBeanDefinition(String beanName) throws NoSuchBeanDefinitionException;

    //获取注册表中指定名称的bean
    BeanDefinition getBeanDefinition(String beanName) throws NoSuchBeanDefinitionException;

    //判断注册表中是否已经注册了指定名称的bean
    boolean containsBeanDefinition(String beanName);

    //获取注册表中所有的bean的名称
    String[] getBeanDefinitionNames();

    int getBeanDefinitionCount();
    boolean isBeanNameInUse(String beanName);
}
```

我们可以来看看其下的继承结构图以及其子类用于保存的属性，都是Map其实，只不过是最初创建的大小不同

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa5c327585b1b76e24fa4f35581251fcf.png)

- 创建容器

最后我们来看看创建容器的过程，简单来说就是调用refresh()方法，对我们的配置文件进行初始化并生成该类到我们的容器中令其管理

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd21550e82f0b820263829ef15a6172c9.png)

