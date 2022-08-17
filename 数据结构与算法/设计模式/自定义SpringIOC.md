学完了所有内容之后，现在我们来学习如果用我们之前学过的设计模式来自定义Spring框架，当然这里只是一个非常简单的自定义Spring框架，主要还是复习一下

在自定义Spring框架前，我们先来回顾一下Spring框架的使用、

# 配置文件使用Spring管理类

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc00e1b55fed43543b7ad57896ad7c9c0.png)

# Spring核心功能结构

我们先来看看Spring核心容器的概述图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEca117d12edff7584f364cded9e50bfd6.png)

核心容器由 beans、core、context 和 expression（Spring Expression Language，SpEL）4个模块组成。

- spring-beans和spring-core模块是Spring框架的核心模块，包含了控制反转（Inversion of Control，IOC）和依赖注入（Dependency Injection，DI）。

- BeanFactory使用控制反转对应用程序的配置和依赖性规范与实际的应用程序代码进行了分离。BeanFactory属于延时加载，也就是说在实例化容器对象后并不会自动实例化Bean，只有当Bean被使用时，BeanFactory才会对该 Bean 进行实例化与依赖关系的装配。



- spring-context模块构架于核心模块之上，扩展了BeanFactory，为它添加了Bean生命周期控制、框架事件体系及资源加载透明化等功能。

- 该模块还提供了许多企业级支持，如邮件访问、远程访问、任务调度等，ApplicationContext 是该模块的核心接口，它的超类是 BeanFactory。与BeanFactory不同，ApplicationContext实例化后会自动对所有的单实例Bean进行实例化与依赖关系的装配，使之处于待用状态。



- spring-context-support模块是对Spring IoC容器及IoC子容器的扩展支持。



- spring-context-indexer模块是Spring的类管理组件和Classpath扫描组件。



- spring-expression 模块是统一表达式语言（EL）的扩展模块，可以查询、管理运行中的对象，同时也可以方便地调用对象方法，以及操作数组、集合等。它的语法类似于传统EL，但提供了额外的功能，最出色的要数函数调用和简单字符串的模板函数。EL的特性是基于Spring产品的需求而设计的，可以非常方便地同Spring IoC进行交互。

接下来我们先来学习下bean

# Bean概述

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

# BeanFactory解析

Spring中Bean的创建是典型的工厂模式，这一系列的Bean工厂，即IoC容器，为开发者管理对象之间的依赖关系提供了很多便利和基础服务，在Spring中有许多IoC容器的实现供用户选择，其相互关系如下图所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE68a15e48c6829375d0bc1cfc16264962.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE66f86703ca5aefe0a8b863d108f215ab.png)

要知道工厂是如何产生对象的，我们需要看具体的IoC容器实现，Spring提供了许多IoC容器实现，比如：

- ClasspathXmlApplicationContext : 根据类路径加载xml配置文件，并创建IOC容器对象。



- FileSystemXmlApplicationContext ：根据系统路径加载xml配置文件，并创建IOC容器对象。



- AnnotationConfigApplicationContext ：加载注解类配置，并创建IOC容器。

然后我们来学习下BeanDefinition

# BeanDefinition解析

Spring IoC容器管理我们定义的各种Bean对象及其相互关系，而Bean对象在Spring实现中是以BeanDefinition来描述的，如下面配置文件

```
<bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"></bean>

bean标签还有很多属性：
scope、init-method、destory-method等。
```

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa5c327585b1b76e24fa4f35581251fcf.png)

简单来说，我们的bean中的标签都是被封装好的，而这些就是通过BeanDefinition封装的

接着我们来学习下BeanDefinitionReader

# BeanDefinitonReader解析

Bean的解析过程非常复杂，功能被分得很细，因为这里需要被扩展的地方很多，必须保证足够的灵活性，以应对可能的变化。Bean的解析主要就是对Spring配置文件的解析。这个解析过程主要通过BeanDefinitionReader来完成，看看Spring中BeanDefinitionReader的类结构图，如下图所示

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE088037c38d3819f91ef6820cb540b480.png)

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

# BeanDefinitionRegistry解析

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7f916f27b31948e12ccf7f8e84c52c42.png)

# 创建容器

最后我们来看看创建容器的过程，简单来说就是调用refresh()方法，对我们的配置文件进行初始化并生成该类到我们的容器中令其管理

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa577bdb165fcbf17585029b86a865482.png)

解析完了Spring框架之后，我们接着来自定义一个SpringIOC的框架，先来看看我们要实现的功能

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd21550e82f0b820263829ef15a6172c9.png)

简单来说我们要自定义一个项目，令其来实现我们之前配置文件里实现的功能

# 定义bean相关的pojo类

那么首先我们要先定义bean相关的pojo的类，首先我们来定义封装bean的属性的PropertyValue类，该类用于封装bean的属性，体现到上面的配置文件就是封装bean标签的子标签property标签数据

那么我们首先创建PropertyValue类看，然后其下封装三个bean标签中的属性，当然，其都是字符串类型的，接着提供给其所需要的构造方法和setandget方法即可

```
/**
 * 用来封装Bean标签下的property属性
 * name属性
 * ref属性
 * value属性 ： 给基本数据类型及String类型数据赋值
 */
public class PropertyValue {

    private String name;
    private String ref;
    private String value;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getRef() {
        return ref;
    }

    public void setRef(String ref) {
        this.ref = ref;
    }

    public String getValue() {
        return value;
    }

    public void setValue(String value) {
        this.value = value;
    }

    public PropertyValue(String name, String ref, String value) {
        this.name = name;
        this.ref = ref;
        this.value = value;
    }

    public PropertyValue() {
    }
}
```

最后我们先来看看我们的包结构吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE52faca299dcd8ff62bcc1bf1a90f969b.png)

# MutablePropertyValues类

一个bean标签可以有多个property子标签，每一个property子标签都会被封装成PropertyValue类，而这些类也是需要存储和管理的

所以我们再定义一个MutablePropertyValues类，用来存储并管理多个PropertyValue对象，同样的定义在bean包下

那么我们可以写入我们的代码如下，我们这里使用到了迭代器模式，令其实现对应的迭代器接口，这样我们就可以返回用于遍历的迭代器对象了，我们存储集合对象，就是用集合对象来保存，加上final关键词修饰，提供无参和有参两个构造方法，给予对应的赋予逻辑保证我们的属性不可能为空

然后我们要重写的返回迭代器的方法，我们可以直接调用List集合中提供的返回迭代器的方法即可

```
package com.itheima.framework.beans;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

/**
 * 用户存储和管理多个PropertyValue对象
 */
public class MutablePropertyValues implements Iterable<PropertyValue>{

    //定义List集合对象，用来存储PropertyValue对象
    private final List<PropertyValue> propertyValuesList;

    public MutablePropertyValues() {
        this.propertyValuesList=new ArrayList<>();
    }

    public MutablePropertyValues(List<PropertyValue> propertyValuesList) {
        if(propertyValuesList==null){
            this.propertyValuesList=new ArrayList<>();
        }else {
            this.propertyValuesList=propertyValuesList;
        }
    }

    //获取所有的PropertyValue对象，返回以数组的形式
    public PropertyValue[] getPropertyValues() {
        //将集合转换为数组并返回
        return propertyValuesList.toArray(new PropertyValue[0]);
    }

    //根据name属性值获取PropertyValue对象
    public PropertyValue getPropertyValue(String propertyName) {
        //遍历集合对象
        for (PropertyValue propertyValue : propertyValuesList) {
            if(propertyValue.getName().equals(propertyName)){
                return propertyValue;
            }
        }
        return null;
    }

    //判断集合是否为空
    public boolean isEmpty() {
        return propertyValuesList.isEmpty();
    }

    //添加PropertyValue对象
    public MutablePropertyValues addPropertyValue(PropertyValue pv) {
        //判断集合中存储的PropertyValue对象是否和传递进来的重复了，如果重复则进行覆盖
        for (int i = 0; i < propertyValuesList.size(); i++) {
            //获取集合中每一个PropertyValue对象
            PropertyValue propertyValue = propertyValuesList.get(i);
            if(propertyValue.getName().equals(pv.getName())){
                propertyValuesList.set(i,pv);
                return this;//返回原来的对象目的就是为了实现链式编程
            }
        }
        propertyValuesList.add(pv);
        return this;//返回原来的对象目的就是为了实现链式编程
    }

    //判断是否有指定name属性值的对象
    public boolean contains(String propertyName) {
        return getPropertyValue(propertyName) != null;
    }

    //获取迭代器对象
    @Override
    public Iterator<PropertyValue> iterator() {
        return propertyValuesList.iterator();
    }
}

```

接着我们提供获取所有的PropertyValue对象并以数组形式返回的方法，这里调用的就是数组中toArray方法，并且我们这里返回的是一个我们指定的数组对象

然后我们再提供一个根据name属性值获得具体的PropertyValue对象的方法，这里就是直接遍历集合然后比对名字即可

接着是判断集合是否为空，添加对象到集合中的方法。这里值得一提是，我们这里为了实现链式编程，所以这里每次添加时都令其返回当前的对象

其次是这里判断是否有指定name属性值的对象的时候调用了同层的另一个方法，我这当然也可以，但是我印象中是同层的方法是不可以互相调用的，这里可能存在不合乎规范的问题

# BeanDefiniton类

BeanDefinition类用来封装bean信息的，主要包含id（即bean对象的名称）、class（需要交由spring管理的类的全类名）及子标签property数据

我们同样是将我们的类创建在beans包下，我们这里提供id和className属性，当然，其下还具有管理PropertyValue对象的MutablePropertyValues对象，我们提供了对应的getandset方法之后再提供一个无参的构造方法，无参的构造方法中，我们需要创建我们的管理对象

```
package com.itheima.framework.beans;

/**
 * 用来封装bean标签属性
 * id属性
 * class属性
 * property子标签的数据
 */
public class BeanDefinition {

    private String id;
    private String className;

    private MutablePropertyValues propertyValues;

    public BeanDefinition() {
        this.propertyValues = new MutablePropertyValues();
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getClassName() {
        return className;
    }

    public void setClassName(String className) {
        this.className = className;
    }

    public MutablePropertyValues getPropertyValues() {
        return propertyValues;
    }

    public void setPropertyValues(MutablePropertyValues propertyValues) {
        this.propertyValues = propertyValues;
    }
}
```

# 定义注册表相关类

BeanDefintionRgistry定义了注册表的相关操作并且定义了注册表的相关操作，具体拥有的功能请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa141aeb39497e0033f1c15c2fca654b9.png)

那么我们首先在beans下的factory的support包内创建BeanDefinitionRegistry接口

```
/**
 * 注册表接口
 */
public interface BeanDefinitionRegistry {

    //注册BeanDefinition对象到注册表中
    void registerBeanDefinition(String beanName, BeanDefinition beanDefinition);

    //从注册表中删除指定名称的BeanDefinition对象
    void removeBeanDefinition(String beanName) throws Exception;

    //根据名称从注册表中获取BeanDefinition对象
    BeanDefinition getBeanDefinition(String beanName) throws Exception;

    boolean containsBeanDefinition(String beanName);

    int getBeanDefinitionCount();

    String[] getBeanDefinitionNames();
}
```

然后创建其子实现类，该实现类内部拥有一个Map用于用于存储BeanDefiniton作为value值，而Stirng作为唯一标识

接着我们提供对应的方法即可，无非就是调用Map中的各种方法而已

```
public class SimpleBeanDefinitionRegistry implements BeanDefinitionRegistry{

    //定义一个容器，用来存储BeanDefinition
    private Map<String,BeanDefinition> beanDefinitionMap = new HashMap<>();

    @Override
    public void registerBeanDefinition(String beanName, BeanDefinition beanDefinition) {
        beanDefinitionMap.put(beanName,beanDefinition);
    }

    @Override
    public void removeBeanDefinition(String beanName) throws Exception {
        beanDefinitionMap.remove(beanName);
    }

    @Override
    public BeanDefinition getBeanDefinition(String beanName) throws Exception {
        return beanDefinitionMap.get(beanName);
    }

    @Override
    public boolean containsBeanDefinition(String beanName) {
        return beanDefinitionMap.containsKey(beanName);
    }

    @Override
    public int getBeanDefinitionCount() {
        return beanDefinitionMap.size();
    }

    @Override
    public String[] getBeanDefinitionNames() {
        return beanDefinitionMap.keySet().toArray(new String[0]);
    }
}
```

最后都学习到这里了，为了防止我们搞混了，我们先来梳理下目前位置我们学习的内容之间的关系

我们都知道每一个property标签都会被封装成propertyValue对象，而且propertyValues对象会被MutableProperty管理，这个对象我们姑且成为property标签管理对象，然后的bean标签本身又具有id和class属性，这些是属性是封装到BeanDefinition对象中的，同时其下还具有标签管理对象的属性，相当于BeanDefiniton就代表了一个bean标签。然后我们还提供了一个类用来保存bean的唯一标识和其具体内容，该类就是BeanDefinitionRegistry类，其下具有一个Map属性，左边对应的用户给被管理类指定提供的唯一标识，右边则是代表了管理一个类的BeanDefiniton类，换言之，我们可以将BeanDefinitonRegistry简单理解为整个容器，其管理了我们托管到容器中的所有类

# 定义解析器相关类

接着我们要来定义解析器的相关类，即使解析配置文件并令其自动将配置文件的信息注册到我们的注册表中，这里我们一般使用先创建一个BeanDefinitonReader接口

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE71cf0dfc05d11e3c1409206d05cc68e2.png)

接口中提供两个方法，一个是获取注册表对象，另外一个是加载配置文件并在注册表中进行注册

至于为什么我们不直接创建实现类而是创建一个接口，这是因为我们的解析器相关类是有多个实现类的，针对不同的配置有不同的实现方式，所以我们这里一般是是一个接口多个实现类的方式，当然我们这里是学习阶段，所以我们这里其实也只搞一个实现类就是了

XmlBeanDefinitionReader类是专门用来解析xml配置文件的。该类实现BeanDefinitionReader接口并实现接口中的两个功能，我们就创建这个类并实现其功能

首先我们要读取配置流我们需要借助到外部的dom4j工具，我们首先引入对应的依赖，dom4j的坐标如下

```
<dependency>
  <groupId>dom4j</groupId>
  <artifactId>dom4j</artifactId>
  <version>1.6.1</version>
</dependency>
```

那么最终我们可以写入我们的实现类的代码如下，首先我们实现类中组合了注册表对象的属性，第一个方法要获得当前的注册表对象，我们直接返回该对象即可，重点在于第二个方法的实现，我们这里能看到我们的类是一开始就有注册表对象的，这是当然的，因为我们的这个类要实现的事情就是将配置的内容自动注册到注册表中，要是我们这个类里一开始就没有注册表对象，那还自动注册个几把

```
/**
 * 针对xml配置文件进行解析的类
 */
public class XmlBeanDefinitionReader implements BeanDefinitionReader {

    //声明注册表对象
    private BeanDefinitionRegistry registry;

    public XmlBeanDefinitionReader() {
        registry = new SimpleBeanDefinitionRegistry();
    }

    @Override
    public BeanDefinitionRegistry getRegistry() {
        return registry;
    }

    @Override
    public void loadBeanDefinitions(String configLocation) throws Exception {
        //使用dom4j进行xml配置文件的解析
        SAXReader reader = new SAXReader();
        //获取类路径下的配置文件
        InputStream ois = XmlBeanDefinitionReader.class.getClassLoader().getResourceAsStream(configLocation);
        Document document = reader.read(ois);
        //根据Document对象获取根标签对象(beans)
        Element rootElement = document.getRootElement();
        //获取根标签下的所有bean标签对象
        List<Element> beanElements = rootElement.elements("bean");
        for (Element beanElement : beanElements) {
            //获取id属性
            String id = beanElement.attributeValue("id");
            //获取class属性
            String className = beanElement.attributeValue("class");
            //将id属性和class属性封装到beanDefinition
            //1.创建BeanDefinition
            BeanDefinition beanDefinition = new BeanDefinition();
            beanDefinition.setId(id);
            beanDefinition.setClassName(className);

            //创建MutablePropertyValues对象
            MutablePropertyValues mutablePropertyValues = new MutablePropertyValues();

            //获取bean标签下所有的property标签对象
            List<Element> propertyElements = beanElement.elements("property");
            for (Element propertyElement : propertyElements) {
                String name = propertyElement.attributeValue("name");
                String ref = propertyElement.attributeValue("ref");
                String value = propertyElement.attributeValue("value");
                PropertyValue propertyValue = new PropertyValue(name,ref,value);
                mutablePropertyValues.addPropertyValue(propertyValue);
            }
            //将MutablePropertyValues对象封装到BeanDefinition对象中
            beanDefinition.setPropertyValues(mutablePropertyValues);

            //将beanDefinition对注册到注册表中
            registry.registerBeanDefinition(id,beanDefinition);
        }
    }
}

```

首先我们使用dom4j，创建对应的SAXReader对象，然后蝴蝶传入的类路径下的配置文件得到其输入流，然后传入到dom4j中可以获取到Document对象，我们可以根据Document对象获取根标签对象beans，调用内部的getRootElement()方法即可，得到的是一个大的beans标签

然后我们调用Element内部的elements方法，传入bean关键字，就可以获得其下所有的bean标签对象，我们用集合承接

接着我们遍历所有的bean标签，获得bean标签内部的id和classname属性，接着我们创建BeanDefinition对象，相当于是我们创建了一个代表bean标签的类，将对应的id和classname都设置到该对象中，接着创建管理bean内的所有property尚需经的管理者对象MutablePropertyValues对象，然后我们获取bean标签下的所有property标签对象

继续遍历该集合，获取其中的name，ref和value属性值，然后创建一个PropertyValue对象并设置对应的属性，其代表了封装一个property属性，然后往管理者对象中添加该对象

这个过程设置完后再往代表整个bean标签的BeanDefiniton对象中设置我们的property管理者对象进去，最后每次循环末位我们都将代表bean标签的对象和唯一标识加入到我们的注册表中

经过这一整个过程，最终我们就实现了我们的注册表的自动注册

值得一提的是，我们这里只是实现了自动注册而已，也就是说我们的注册表BeanDefinitionRegistry对象中已经有我们之前保存的对应的数据了，但是实际的对象还不存在，创建实际对象的方法在IOC容器相关类中实现，并不是这里

那有人会说注册表有个什么几把用？其实注册表存在的意义就是知道后面的IOC到底要创建哪些类到容器中而存在的

# IOC容器相关类

接着我们我们来定义我们的容器相关的类，这也是我们的本次学习最核心的内容，首先我们要定义BeanFactory接口，该接口中定义了IOC容器获取Bean对象的统一规范

```
/**
 * IOC容器父接口
 */
public interface BeanFactory {

    Object getBean(String name) throws Exception;

    <T> T getBean(String name,Class<? extends T> clazz) throws Exception;
}
```

我们第二个方法是使用了泛型，其代表的意义是我们返回的类型是一个传入的泛型，而传入的参数只需要是该泛型或者是该泛型的子实现类即可

然后我们来看看ApplicaitonContext接口

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE475ec645dd1397f9a40e1c7dccc237f7.png)

那么根据上图我们可以定义该接口如下

```
/**
 * 定义非延时加载功能
 */
public interface ApplicationContext extends BeanFactory {

    void refresh() throws Exception;
}
```

接着我们来创造其具体的实现类，先来看看构造该类应该注意的事项

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE54a65ffbdfe1fe89ee907132ea88f5c9.png)

那么最终我们可以构造我们的代码如下，首先其实现了ApplicationContext接口，其下首先声明了解析器的变量、存储对象的容器以及文件路径的属性

```
/**
 * AbstractApplicationContext接口的子实现类，用于立即加载
 */
public abstract class AbstractApplicationContext implements ApplicationContext {

    //声明解析器变量
    protected BeanDefinitionReader beanDefinitionReader;

    //定义用于存储bean对象的map容器
    protected Map<String,Object> singletonObjects = new HashMap<>();

    //声明配置文件路径的变量
    protected String configLocation;

    @Override
    public void refresh() throws Exception {
        //加载BeanDefinition对象
        beanDefinitionReader.loadBeanDefinitions(configLocation);
        
        //初始化bean
        finishBeanInitialization();
    }

    //bean的初始化
    protected void finishBeanInitialization() throws Exception {
        //获取注册表对象
        BeanDefinitionRegistry registry = beanDefinitionReader.getRegistry();

        //获取BeanDefinition对象
        String[] beanNames = registry.getBeanDefinitionNames();
        for (String beanName : beanNames) {
            BeanDefinition beanDefinition = registry.getBeanDefinition(beanName);
            //初始化bean
            getBean(beanName);
        }
    }
}

```

首先重写的当然是refresh方法，该方法内我们要加载beanDefiniton对象，我们直接调用其下的自动注册方法，将对应的配置文件的路径传入即可实现将类自动装配到我们的容器中，然后我们调用初始化bean的方法

提供bean的初始化方法，首先获取到我们的注册表对象，然后获取其中的所有BeanDefiniton对象，接着通过BeanFactory的getBean方法实现对注册的类的初始化，当然，目前这个方法还没创建出来，因为我们这个类还只是抽象类，实现具体的初始化方法还需要靠更下一级的子类来实现，因为只有子类明确到底创建BeanDefinitionReader哪个子实现类对象。

# ClassPathXmlApplicationContext类

现在我们来学其真正的具体实现类，首先我们来看看其要完成的功能

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd6c82aa3bd78d16e2c9a865c90fe6577.png)

为了便于我们后续的操作，我们首先构造一个工具类，该工具类可以将传入的方法进行分割，最终返回我们所需要的set方法类型的字符串，便于我们需要完成的依赖注入操作

```
public class StringUtils {
    private StringUtils() {

    }

    //userDao ==> setUserDao
    public static String getSetterMethodByFieldName(String fieldName){
        String methodName = "set" + fieldName.substring(0, 1).toUpperCase() + fieldName.substring(1);
        return methodName;
    }
}
```

那么最终我们可以构造我们的具体实现类如下，其继承了之前的抽象类，必须要重写获取bean对象的方法，这个我们先按下不表

我先来说说其最开始的构造方法，我们往其中提供了一个构造方法，使用该构造方法必须要传入一个对应的类路径地址，我们会将该地址赋予到类属性的保存地址的属性中，我们在构造方法中会构建一个解析器对象，这就相当于是给我们类中最开始的解析器对象赋值，便于我们后续的使用

然后我们再调用该类中的refresh方法，让我们的注册表自动注入对应的配置信息

```
/**
 * IOC容器具体的子实现类
 * 用于加载类路径下的xml格式的配置文件文件
 */
public class ClassPathXmlApplicationContext extends AbstractApplicationContext{

    public ClassPathXmlApplicationContext(String configLocation) {
        this.configLocation=configLocation;
        //构建解析器对象
        beanDefinitionReader = new XmlBeanDefinitionReader();
        try {
            this.refresh();
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    //根据bean对象的名称获取bean对象
    @Override
    public Object getBean(String name) throws Exception {
        //判断对象容器中是否包含指定名称的bean对象，若包含则返回，反之则创建
        Object obj = singletonObjects.get(name);
        if(obj!=null){
            return obj;
        }
        //获取BeanDefinition对象
        BeanDefinitionRegistry registry = beanDefinitionReader.getRegistry();
        BeanDefinition beanDefinition = registry.getBeanDefinition(name);
        if(beanDefinition == null) {
            return null;
        }
        //获取bean信息中的className
        String className = beanDefinition.getClassName();
        //通过反射创建对象
        Class<?> clazz = Class.forName(className);
        Object beanObj = clazz.newInstance();

        //进行依赖注入操作
        MutablePropertyValues propertyValues = beanDefinition.getPropertyValues();
        for (PropertyValue propertyValue : propertyValues) {
            //获取name属性值
            String propertyName = propertyValue.getName();
            //获取value属性
            String value = propertyValue.getValue();
            //获取ref属性
            String ref = propertyValue.getRef();
            if(ref != null && !"".equals(ref)){
                //获取依赖的bean对象
                Object bean = getBean(ref);
                //拼接方法名
                String methodName = StringUtils.getSetterMethodByFieldName(propertyName);
                //获取所有的方法对象
                Method[] methods = clazz.getMethods();
                for (Method method : methods) {
                    if(methodName.equals(method.getName())){
                        //执行该setter方法
                        method.invoke(beanObj,bean);
                    }
                }
            }

            if(value!=null && !"".equals(value)){
                //拼接方法名
                String methodName = StringUtils.getSetterMethodByFieldName(propertyName);
                //获取method对象
                Method method = clazz.getMethod(methodName, String.class);
                method.invoke(beanObj,value);
            }
        }

        //在返回beanObj对象之前，将对象存储到map容器中
        singletonObjects.put(name,beanObj);
        return beanObj;
    }

    @Override
    public <T> T getBean(String name, Class<? extends T> clazz) throws Exception {
        Object bean = getBean(name);
        if(bean==null){
            return null;
        }
        return clazz.cast(bean);
    }
}

```

接着比较重量级的是就是第一个根据bean对象名称获取bean对象的方法了，首先我们判断我们的容器中是否存在指定名称的bean对象，若不存在我们再创建

要创建我们当然首先要获取到我们的注册表对象，调用容器中的getRegistry即可获取到我们的注册表对象，然后再调用获得具体Bean对象的方法并传入关键字即可获得代表一个bean标签的BeanDefinition对象，然后我们再做一个判断，如果我们的bean对象居然都是空的，那就直接传回空对象即可

然后获取bean信息中的classname，我们通过反射方式来创建对象，这个很好理解，但是接下来的问题在于，我们的创建的这个对象可能依赖了其他的对象，此时就需要进行依赖注入，所以接下来我们需要机械能依赖注入操作

首先如果其需要进行对应的依赖注入操作，必然在配置类中是有对应的配置设置的，所以我们首先获得bean对象中的property管理者对象，然后对其进行比那里，获得其下的三个属性值，首先要判断的是ref是否为空，若为空就根本不需要处理，因为ref代表的是某个具体类的类路径，如果这都没有的话那肯定就是没什么需要注入了的

当其不为空时，我们递归继续获得其依赖的bean对象，相当于是我们进入其子类中再进行子类的创建，创建好子类之后就获得子类返回的bean对象，然后我们调用我们之前的工具类获得我们拼接的方法名，然后再通过class字节码属性获得其所有方法，将方法名与我们构造的方法名进行比对，如果一样则说明其就是我们所需要的设置依赖进入到对应类中的方法，那么我们就执行该方法即可

最后我们还有一个情况要考虑，就是我们的value值可能不为空，若value值不为空则说明我们的value值是有数据的，这些数据应该要被赋予到对应的属性中，所以当value值不为空时，我们就要进行对应的赋值操作，我们同样先进行方法名的拼接，然后获取到对应的方法对象，接着执行对应的set方法，当然，使用反射的方式

最后我们在返回对应的beanObj对象之前，我们需要将对象存储到我们的mpa容器中，不然我们的东西光创建出来不保存也的确也没啥用不是

然后第二个方法是根据名字和类别来获得对应的bean对象，这里我们直接调用上一个方法得到的结果即可，如果不为空则说明的确有这个方法，我们就令该对象强制转换再返回即可

# 功能测试

接着我们来测试下我们的自定义的Spring框架到底有没有用，我们首先将该项目编译安装，然后在对应的pom文件中引入我们的坐标重新运行即可，最后得到的结果是确实可行的

然后我们再给我们UserDaoImpl中加入两个String变量，提供对应的set方法，为了看到效果，我们还往展示的方法中也一并展示这两个数据

```
public class UserDaoImpl implements UserDao {

    private String username;
    private String password;

    public void setUsername(String username) {
        this.username = username;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    @Override
    public void add() {
        System.out.println("UserDao..."+username+"=="+password);
    }
}

```

然后我们往配置类中进行更加具体的配置，可以看到我们这里就是按照我们之前的规则进行的配置而已，即使是在Spring中也是如此配置的

```
<?xml version="1.0" encoding="UTF-8"?>
<beans>

    <bean id="userService" class="com.itheima.service.impl.UserServiceImpl">
        <property name="userDao" ref="userDao"></property>
    </bean>

    <bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl">
        <property name="username" value="zhangsan"></property>
        <property name="password" value="123456"></property>
    </bean>

</beans>
```

然后我们重新测试，发现控制台上的确打印了我们需要的东西，那我们的框架就是没有问题的

# 自定义Spring IOC总结

最后我们来看看其总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9b7e186a1d7453add1e372d9302e1a11.png)

