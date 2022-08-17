# IoC底层核心原理

## IoC核心接口

我们点开BeanFactory，然后查看里面内部的方法，我们容易看到BeanFactory是一个接口，其下有许多的方法，且其有三个子接口类以及一个实现类

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd44b7e49cfec8a0f2d58ad5b3ad1f0df.png)

我们可以看看在这个接口里都定义了哪些操作，因此我们来看看其对应的方法，我们首先就能看到其getBean的各种重载方法，其定义了多种获取Bean的方式，比如通过名称获取，通过类型获取等，其实我们曾经学过的按名称自动装配和按类型自动装配都和这些方法有关。

接着getBeanProvider是用于获得Bean的提供对象的方法，后面的方法则是分别判断Bean对象是否存在，是否是单例，是否是非单例，获得其类型，获得其配置的别名等等一系列的方法，都在这里了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8a65b58c1b882258bdce87b840a875a9.png)

其实在BeanFactory里的方法就是定义了一大堆有关于Bean的基础信息的方法

如果我们不断点开接口，最后能到达其实现类，但是所谓的实现类其实也就是实现了接口对应的方法，因此我们要查看方法不必去查看其实现类，直接查看对应的接口就行了，这样便于我们的理解和查看

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9b5f1f7029d5c764ad5f2710ef5c089e.png)

接着我们来说下，我们可以看到在我们的BeanFactory下有三个子接口，而接口下还有接口，为什么要整这么多接口呢？这其实是为了模拟实际项目中的分层调用的情况。我们都知道在我们的实际项目里是分为三层的，分别是数据层，业务层以及表现层，后者依赖于前者，且只能由后者调用前者，前者处于底层而后者处于高层。我们其实可以将这三层关系看做是一个继承的关系，比如我们可以认为是业务层继承数据层，这样业务层就可以使用数据层的数据，实际上业务层也的确可以使用数据层的数据不是。因此在对应的接口中，我们也做了这样的分层，将我们的容器分为了三层（数据层、业务层和表现层分别一个容器），也就是三个接口，其有着对应的继承关系，这样底层的接口就可以调用其父接口的对应方法，这是一个设计理念，了解了这个理念我们就知道为什么其要将接口搞得这么复杂

然后再来看HierarchicalBeanFactory，先来看看方法，我们可以看到containsLocalBean方法，该方法就是用于判断对应的Bean类型是否在同一层级中的方法。使用这个方法就可以做一个层级划分

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE948e266ab10cc666bf812fa0b945ee45.png)

接着我们来说AutowireCapableBeanFactory接口，光靠名字我们都可以猜到这个接口的主要作用就是用于自动装配的，我们不妨来看看里面的方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb480474c5ba36969e8d86583c74705bc.png)

我们可以看到的创建Bean的方法，然后是自动装配的方法，其下还可以设置参数，可以看到0是不装配，1是按名称自动装配，2是按类型自动装配，3是按构造器自动装配。我们容易知道如果我们写入@Autowire然后不指定参数那么其就是按类型自动装配，如果给定名称那么就会按名称自动装配

最后我们来看下其主要接口中的最后一个接口，也就是ListableBeanFactory接口，其翻译过来的意思就是可以列表化的Bean工厂，先来看看其方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb2827f2bd2b58fee1693a64c6519d944.png)

只要看到其方法，我们就啥都明白了，可以看到其下有通过Bean的类型返回其数量，通过Bean的类型返回其名字，或者是通过注解返回被该注解修饰过的Bean的数量等等，证明这个接口的主要作用就是来获得Bean的各种列表信息，用于查询整体的Bean的情况的一个接口

接着我们来看看HierarchicalBeanFactory的子接口，第一个接口ConfigurableBeanFactory的主要作用就是用于配置的工程对象，我们的工程对象也是需要配置的，比如它的范围。然后其下还有ConfigurableListableBeanFactory接口，这个接口其实就是可列表化的进一步实现。同样作为HierarchicalBeanFactory的子接口的还有ApplicationContext接口，其作用就是在BeanFactory之上又追加了一些功能，比如说国际化，就是通过这个接口进行实现的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE02d44eef63637b58ef95d79bfd3209ff.png)

剩下的两个接口的子接口的内容都大差不差了，这里就不再提了，最后我们可以做一个总结

首先是BeanFactory接口

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9201249a05f3cbaf66a14560c7a10e00.png)

然后是HierarchicalBeanFactory接口

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEfbac4785c8983b6045e8917707b57889.png)

接着是AurowireCapableBeanFactory

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE609bb01fc478dbbc0a4e84bfdf71df07.png)

最后是ListableBeanFactory

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc0731d77326233b321ec725603eed807.png)

## 组件扫描过滤器

在我们的开发过程中，有时候我们只需要测试一部分的Bean，而另一部分的Bean是不用加载的，甚至是不能加载的，因为其还没有开发完全，一加载就报错。在实际的开发场景中，这是经常可能会发生的，因此我们要认真学本节的内容。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6ac312dcd642c2189efe65b14e0c12a7.png)

那么最后我们就有一个需求，我们希望在开发场景中，根据需求加载必要的bean，排除指定的bean，这就是我们的目标，那么我们应该要怎么去实现呢？这就是我们今天要将的内容，扫描过滤器了。我们将使用一个案例来讲解我们本节的内容，在操作之前，我们先来对我们的案例环境做一个整体的介绍

我们的案例环境是maven工程，引入了对应的依赖，然后我们创建了Dao层的接口和实现类并给其添加了对应的Component注解，然后给了SpringConfig核心配置文件，文件内写入了下面两行代码

```
@Configuration
@ComponentScan("com.itheima")
```

相当于指定了其固定开头以及所有存在注释的类所在的路径

接着我们在测试类里进行service层实现类调用Dao层的测试，这里我们为了切合我们的分层思想，因此我们的service的实现类使用@Serivce注解，然后我们测试下（Dao和Service都有对应的save方法，内部打印一些语句），发现可以成功运行

如果我们现在将UserServiceImpl类中上的Component注解给注释掉，再运行我们的类的话，就会报这个异常，其代表的意义是为有这个对应的Bean，NoSuchBeanDefinitionException，之所以会报这个异常是因为我们没有将对应类添加到容器中。

那么接下来我们要做的事情就是过滤，我们希望我们可以将我们的对应类的注解去掉，就能让我们的程序报出没有找到这个类的异常，那我们要怎么做呢？首先我们容易想到，我们的注解类都是通过ComponentScan注解扫描进来的，因此我们肯定要对这个注解动手脚，我们在value值的后面加一个逗号，然后可以看到其对应的过滤器的属性，我们可以选择排除过滤器，也可以选择包含过滤器，我们这里选择排除过滤器，也就是excludeFilters，然后我们要指定一个过滤器，该过滤器也是一个注解，而且是Component注解的注解，我们引用时就先输入@ComponentScan.Filter的方式来引用其过滤器注解，然后接着我们要给其指定过滤规则，其下的属性type就是用于指定过滤器规则的，type需要传入FilterType的枚举类，其下可以按注解排除的ANNOTATION和自定义排除CUSTOM，当然还有其他的，但是我们这里只讲解这两个，我们先来讲按注解排除，设定了按注解排除之后我们还有设定需要排除的注解，这里要用classes属性，传入Service注解的class对象，其就会排除掉所有的Service注解类了，由于我们的UserServiceImpl是Service层的，使用的是Service注解，所以此时能正确排除。最后我们可以在SpringConfig中写入代码如下

```
//1.设置排除bean，排除的规则是注解修饰的（FilterType.ANNOTATION）的bean，具体的注解为（Service.class）
/*
@ComponentScan(
    value = "com.itheima",
    excludeFilters = @ComponentScan.Filter(
        type= FilterType.ANNOTATION,
        classes = Service.class
        )
    )
*/
```

最后我们来看看上面的知识的总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd83818208274906144da95a11b602c7a.png)

接着我们来讲解自定义类型排除过滤，我们要实现自定义类型排除过滤，首先我们在springConfig上的注解中将ANNOTAION改为CUSTOM，然后我们要创建一个自定义的过滤器类，然后令其实现TypeFilter接口，重写内部的方法。这里如果我们令其返回false，其代表不过滤，返回true则代表过滤，那么接着我们就要设定我们的过滤规则，我们的过滤规则很简单，就是判断传入的对象是不是我们要过滤的对象，是的话我们就将其过滤，那我们要如何拿到传入的对象的信息呢？我们注意看里面传入的参数，可以看到里面有元数据类型对象和工厂元数据类型对象，我们这里只需要元数据就可以拿到我们所需要的用于判断的，首先调用元数据对象的getClassMetadata()方法可以获得其对应的传入对象，然后调用该对象的getClassName()方法可以获得对象的全路径名，我们将该对象名与我们想要将其过滤的全路径名进行一个比较，若是就返回true，否则就返回false，那么我们可以构造其代码如下

```
package config.filter;

import org.springframework.core.type.ClassMetadata;
import org.springframework.core.type.classreading.MetadataReader;
import org.springframework.core.type.classreading.MetadataReaderFactory;
import org.springframework.core.type.filter.TypeFilter;

import java.io.IOException;

public class MyTypeFilter implements TypeFilter {
    @Override
    //加载的类满足要求，匹配成功
    public boolean match(MetadataReader metadataReader, MetadataReaderFactory metadataReaderFactory) throws IOException {
        //通过参数获取加载的类的元数据
        ClassMetadata classMetadata = metadataReader.getClassMetadata();
        //通过类的元数据获取类的名称
        String className = classMetadata.getClassName();
        //如果加载的类名满足过滤器要求，返回匹配成功
        if(className.equals("com.itheima.service.impl.UserServiceImpl")){
            //返回true表示匹配成功，返回false表示匹配失败。此处仅确认匹配结果，不会确认是排除还是加入，排除/加入由配置项决定，与此处无关
            return true;
        }
        return false;
    }
}

```

最后我们在SpringConfig中的@Component注解中的classes属性中传入我们自定义类的class对象

```
//2.设置排除bean，排除的规则是自定义规则（FilterType.CUSTOM），具体的规则定义为（MyTypeFilter.class）
/*
@ComponentScan(
        value = "com.itheima",
        excludeFilters = @ComponentScan.Filter(
                type= FilterType.CUSTOM,
                classes = MyTypeFilter.class
        )
)
*/
```

最后我们来看看自定义过滤类型的总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0d8f46c36c3bf820359e85aab9bae8d1.png)

## 自定义导入器

回忆一下我们之前导入bean的方式，不是通过注解就是通过配置文件，这两种方式的特点都是一个个导入的，但是都是一个个类导入的，我们现在期待一种能够群体导入的方式，此时就需要自定义一个导入器了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe6e873aef47b93e6fc5bddb4a7dea5fd.png)

那么我们就怎么自定义一个导入器呢？实现ImportSelector接口就可以了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe14de4b934d10bd5916be685f4e132c2.png)

我们这里定义一个加载类，然后实现ImportSelector接口，其下要返回一个String[]类型的数组，该数组的内容要是我们需要导入的类的全类名，我们可以直接往里面写入全类名，但是一般而言，我们都将我们要导入的全类名写到一个配置类中会比较好，因此我们创建一个import的properties文件，然后将其加载，接着我们获得其对应的类名，然后将其返回。我们也可以在properties的文件中传入多个类名，用，分隔开，然后加载文件时，通过split方法返回一个String[]数组

```
package config.selector;

import org.springframework.context.annotation.ImportSelector;
import org.springframework.core.type.AnnotationMetadata;

import java.util.ResourceBundle;

public class MyImportSelector implements ImportSelector {
    @Override
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {
//      1.编程形式加载一个类
//      return new String[]{"com.itheima.dao.impl.BookDaoImpl"};

//      2.加载import.properties文件中的单个类名
//      ResourceBundle bundle = ResourceBundle.getBundle("import");
//      String className = bundle.getString("className");

//      3.加载import.properties文件中的多个类名
        ResourceBundle bundle = ResourceBundle.getBundle("import");
        String className = bundle.getString("className");
        return className.split(",");
    }
}

```

那么有的同学就会问了，我们就不可以加载全部的所需要导入的类进去吗？这只需要输入对应的包名就可以了，一次解决多个问题，这当然可以，但是我们这样需要对我们的传入的名字进行一个特殊处理，这需要用到工具类，我们这里直接给出这个工具类了，就不带大家写一遍了

```
package config.selector;

import org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.ImportSelector;
import org.springframework.core.io.support.PropertiesLoaderUtils;
import org.springframework.core.type.AnnotationMetadata;
import org.springframework.core.type.filter.AspectJTypeFilter;
import org.springframework.core.type.filter.TypeFilter;

import java.io.IOException;
import java.util.HashSet;
import java.util.Map;
import java.util.Properties;
import java.util.Set;

public class CustomerImportSelector implements ImportSelector {

    private String expression;

    public CustomerImportSelector(){
        try {
            //初始化时指定加载的properties文件名
            Properties loadAllProperties = PropertiesLoaderUtils.loadAllProperties("import.properties");
            //设定加载的属性名
            expression = loadAllProperties.getProperty("path");
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }

    @Override
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {
        //1.定义扫描包的名称
        String[] basePackages = null;
        //2.判断有@Import注解的类上是否有@ComponentScan注解
        if (importingClassMetadata.hasAnnotation(ComponentScan.class.getName())) {
            //3.取出@ComponentScan注解的属性
            Map<String, Object> annotationAttributes = importingClassMetadata.getAnnotationAttributes(ComponentScan.class.getName());
            //4.取出属性名称为basePackages属性的值
            basePackages = (String[]) annotationAttributes.get("basePackages");
        }
        //5.判断是否有此属性（如果没有ComponentScan注解则属性值为null，如果有ComponentScan注解，则basePackages默认为空数组）
        if (basePackages == null || basePackages.length == 0) {
            String basePackage = null;
            try {
                //6.取出包含@Import注解类的包名
                basePackage = Class.forName(importingClassMetadata.getClassName()).getPackage().getName();
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            }
            //7.存入数组中
            basePackages = new String[] {basePackage};
        }
        //8.创建类路径扫描器
        ClassPathScanningCandidateComponentProvider scanner = new ClassPathScanningCandidateComponentProvider(false);
        //9.创建类型过滤器(此处使用切入点表达式类型过滤器)
        TypeFilter typeFilter = new AspectJTypeFilter(expression,this.getClass().getClassLoader());
        //10.给扫描器加入类型过滤器
        scanner.addIncludeFilter(typeFilter);
        //11.创建存放全限定类名的集合
        Set<String> classes = new HashSet<>();
        //12.填充集合数据
        for (String basePackage : basePackages) {
            scanner.findCandidateComponents(basePackage).forEach(beanDefinition -> classes.add(beanDefinition.getBeanClassName()));
        }
        //13.按照规则返回
        return classes.toArray(new String[classes.size()]);
    }
}

```

接着我们来看看我们的import.properties的代码

```
#2.加载import.properties文件中的单个类名
#className=com.itheima.dao.impl.BookDaoImpl

#3.加载import.properties文件中的多个类名
#className=com.itheima.dao.impl.BookDaoImpl,com.itheima.dao.impl.AccountDaoImpl

path=com.itheima.dao.impl.*

```

最后不要忘了，要使用对应的导入时，需要在SpringConfig类中使用对应的注解

```
//3.自定义导入器
//@Import(MyImportSelector.class)
```

## 自定义注册器

我们之前使用@ComponentScan("com.itheima")来定义我们注解使用的位置并获取注解指定的对象，现在我们要通过一个自定义的注册器来模拟这个注解的功能，其需要实现ImportBeanDefinitionRegistrar接口

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2c784639a98cacfd53f6d459c22fe1cf.png)

我们实现这个接口需要重写里面的方法，我们在重写的该方法中要开启类路径的bean定会扫描器ClassPathBeanDefinitionScanner，需要传入一个Bean定义注册器和一个布尔类型的变量，注册器可以由我们重写的方法中的参数传入，而true和false则代表我们要不要使用其默认的过滤器，这里我们使用我们自己的过滤器，因此我们传入false。然后我们调用其对应的方法设置类型过滤器，我们可以设置排除过滤器也可以设置包含过滤器和排除过滤器，这里我们设置包含过滤器，过滤方法设置为全部通过，简单粗暴。最后我们设置一个扫描路径，此时我们这个类的功能就和我们之前的注解一模一样了，我们在SpringConfig将这个注册类用import注解导入，能实现一样的功能

```
package config.registrar;

import org.springframework.beans.factory.support.BeanDefinitionRegistry;
import org.springframework.context.annotation.ClassPathBeanDefinitionScanner;
import org.springframework.context.annotation.ImportBeanDefinitionRegistrar;
import org.springframework.core.type.AnnotationMetadata;
import org.springframework.core.type.classreading.MetadataReader;
import org.springframework.core.type.classreading.MetadataReaderFactory;
import org.springframework.core.type.filter.TypeFilter;

import java.io.IOException;

public class MyImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {
    @Override
    public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
        //自定义注册器
        //1.开启类路径bean定义扫描器，需要参数bean定义注册器BeanDefinitionRegistry，需要制定是否使用默认类型过滤器
        ClassPathBeanDefinitionScanner scanner = new ClassPathBeanDefinitionScanner(registry,false);
        //2.添加包含性加载类型过滤器（可选，也可以设置为排除性加载类型过滤器）
        scanner.addIncludeFilter(new TypeFilter() {
            @Override
            public boolean match(MetadataReader metadataReader, MetadataReaderFactory metadataReaderFactory) throws IOException {
                //所有匹配全部成功，此处应该添加实际的业务判定条件
                return true;
            }
        });
        //设置扫描路径
        scanner.scan("com.itheima");
    }
}

```

但是这个注册类的实现还是非常简单粗暴的，因此我们这里提供了一个比较不错的注册工具类

```
package config.registrar;

import org.springframework.beans.factory.support.BeanDefinitionRegistry;
import org.springframework.context.annotation.ClassPathBeanDefinitionScanner;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.ImportBeanDefinitionRegistrar;
import org.springframework.core.io.support.PropertiesLoaderUtils;
import org.springframework.core.type.AnnotationMetadata;
import org.springframework.core.type.filter.AspectJTypeFilter;
import org.springframework.core.type.filter.TypeFilter;

import java.io.IOException;
import java.util.Map;
import java.util.Properties;

public class CustomeImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {

    private String expression;

    public CustomeImportBeanDefinitionRegistrar(){
        try {
            //初始化时指定加载的properties文件名
            Properties loadAllProperties = PropertiesLoaderUtils.loadAllProperties("import.properties");
            //设定加载的属性名
            expression = loadAllProperties.getProperty("path");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
        //1.定义扫描包的名称
        String[] basePackages = null;
        //2.判断有@Import注解的类上是否有@ComponentScan注解
        if (importingClassMetadata.hasAnnotation(ComponentScan.class.getName())) {
            //3.取出@ComponentScan注解的属性
            Map<String, Object> annotationAttributes = importingClassMetadata.getAnnotationAttributes(ComponentScan.class.getName());
            //4.取出属性名称为basePackages属性的值
            basePackages = (String[]) annotationAttributes.get("basePackages");
        }
        //5.判断是否有此属性（如果没有ComponentScan注解则属性值为null，如果有ComponentScan注解，则basePackages默认为空数组）
        if (basePackages == null || basePackages.length == 0) {
            String basePackage = null;
            try {
                //6.取出包含@Import注解类的包名
                basePackage = Class.forName(importingClassMetadata.getClassName()).getPackage().getName();
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            }
            //7.存入数组中
            basePackages = new String[] {basePackage};
        }
        //8.创建类路径扫描器
        ClassPathBeanDefinitionScanner scanner = new ClassPathBeanDefinitionScanner(registry, false);
        //9.创建类型过滤器(此处使用切入点表达式类型过滤器)
        TypeFilter typeFilter = new AspectJTypeFilter(expression,this.getClass().getClassLoader());
        //10.给扫描器加入类型过滤器
        scanner.addIncludeFilter(typeFilter);
        //11.扫描指定包
        scanner.scan(basePackages);
    }
}
```

我们直接使用这个工具类就可以了，值得一提的是这个工具类只加载Dao层里面的实现类，这是因为我们最开始定义的import文件里我们就是加载dao层的注解的地址，所以如果我们运行我们的程序的话，还需要一个注解用于去加载service层中的注解的注解，这点不要忘了。

## bean初始化过程解析

本章的最后一个内容，就是对bean的初始化过程进行一个解析。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0291bb06d5676be92b3a388283eb4b1d.png)

我们不妨先回忆下我们的创建对应的bean的过程，我们首先要创建一个类，然后对其进行初始化，提供对应的set方法（如果它需要提供的话），还有无参构造方法，然后我们的Bean每次初始化时都要创建执行初始化动作，也就是initMethod。那么假设现在有这么一个需求，我们需要每次创建Bean和创建Bean结束的时候都对Bean执行一个同样的动作，那么为了满足这个需求，其就为我们提供了BeanPostProcessor接口，该接口就可以实现这个功能，同时为了防止我们忘了实现这个功能，其还为每个初始化功能提供了一个InitializingBean接口，这个接口的作用就是防止初始化过程中我们忘记去给对应的初始化指定对应的功能而存在的接口，其与initMethod的作用可以说是相同的，但是他们并不冲突，可以共存，不过一般我们只写一个，同时前者必须写，后者可写可不写，且后者的名字是不固定的。同时为了满足工厂对象初始化时执行一个固定动作的需求，其还提供了BeanFactoryPostProcessor接口，该接口可以满足此要求

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE95562ee79f3a168cbb3af57ab9a09b3e.png)

这三个接口可以同时使用，但是有一点要注意，就是其都需要被spring容器加载才可以运行，否则是行不通的

接着我们通过一个案例来实现上面所讲解的知识，首先我们先来实现工厂创建时要执行的动作的类，我们创建一个类令其实现对应的接口，然后写入重写其方法然后再写入测试代码就可以了

```
package config.postprocessor;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanFactoryPostProcessor;
import org.springframework.beans.factory.config.ConfigurableListableBeanFactory;

public class MyBeanFactory implements BeanFactoryPostProcessor {
    @Override
    //工厂后处理bean接口核心操作
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
        System.out.println("bean工厂制作好了，还有什么事情需要处理");
    }
}

```

可以看到我们这里自定义了一个类令其实现了BeanFactoryPostProcessor接口，内部我们打印一个语句就没了。经过测试我们会发现这个是可以运行的，接着我们来实现Bean对象初始化前后要执行的动作的方法，我们创建一个类并令其实现BeanPostProcessor接口，我们会发现其内部的方法是并不强制要求重写的，这是因为其自身已经提供了一个默认的实现了，我们可以选择实现与不实现，我们这里当然是实现了。

```
package config.postprocessor;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;

public class MyBean implements BeanPostProcessor {
    @Override
    //所有bean初始化前置操作
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("bean之前巴拉巴拉");
        System.out.println(beanName);
        return bean;
    }

    @Override
    //所有bean初始化后置操作
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("bean之后巴拉巴拉");
        return bean;
    }
}

```

我们可以看到其传入的参数分别是对象本身和其名字，我们需要在处理完之后将这个对象返回，否则Spring接受不到返回的对象就无法进行后面的对应的工作了

最后我们来实现最后一个接口，也就是对应的为了防止我们初始化时忘记执行动作的接口，我们要实现这个接口需要到对应的实现类中去，令其实现InitializingBean接口并重写其中的方法，然后对其进行我们想要的处理就可以了，其作用等同于我们之前的init-method属性配置，我们可以将其理解为我们采用统一接口实现初始化方法前的一个早期实现方式，了解下就可以了

```
package com.itheima.dao.impl;

import com.itheima.dao.EquipmentDao;
import org.mybatis.spring.SqlSessionFactoryBean;
import org.springframework.beans.factory.InitializingBean;
import org.springframework.stereotype.Component;


public class EquipmentDaoImpl implements EquipmentDao,InitializingBean {

    public void save() {
        System.out.println("equipment dao running...");
    }

    @Override
    //定义当前bean初始化操作，功效等同于init-method属性配置
    public void afterPropertiesSet() throws Exception {
        SqlSessionFactoryBean fb;
        System.out.println("EquipmentDaoImpl......bean ...init......");
    }
}

```

最后我们来学习最后一个接口FactoryBean，该接口可以用于处理单个Bean中的特殊处理的情况，因为我们实际的业务开发中，可能其他bean的前期处理都很简单，但是偏偏有一个Bean，其需要大量的特殊处理，此时为了提高效率，简化配置就需要用到这个接口

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8cce0be0e1c9a37f1d8af203b0ccbff1.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE864f87a921d79718ed8a7ba9c7773b66.png)

这个了解下就好了吧，直接看图吧，这里就不带着大家做一遍了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6cb50d29791a4eee18a77870a90fd38e.png)

# AOP配置

本章节我们来讲解一个重量级内容，就是AOP，我们之前学习Spring的时候也介绍过AOP了，其是我们的Spring的核心功能，因此我们要重点学习，同时本节偏理论

## AOP概念

在我们正式讲解AOP之前，我们先来讲讲OOP，我们以前OOP开发我们的项目时，我们总是先看准我们的类然后实现其三层的接口，最后实现我们的整个功能，换言之，我们OOP开发关注的是层级

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc439444d4688184f246328e11a38f789.png)

我们OOP的开发总是会充斥着许多重复代码，尽管我们可以创建一个工具类将重复的代码抽取出来，但是这种抽取只能抽取一部分，而且还有许多代码压根抽不出来。此时我们的AOP功能就出场了，AOP简而言之是将相同的代码做成一个共性功能，当我们编写代码时，可以忽略这些共性功能的代码，而当这些代码真正执行时，其会自动将这些共性功能加入进去，这样就可以让我们的代码变得简洁，同时提高我们代码的复用性

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa520baa0979cb52228eeb0181d6de619.png)

接下来我们正式来介绍下AOP的概念，AOP全称是Aspect Oriented Programing，意为面向切面编程，是一种编程范式，也就是一种编程规范。其弥补了OOP的不足

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6d020c7d4ce6d63a605dd9cf843b8288.png)

AOP可以将代码转化为共性功能，从而完成业务的开发，其唯一的不足在于，由于各行各业的开发规范并不是完全统一的，因此AOP的使用总是不那么顺利，如果两个项目的开发并不完全相同，那么结合时就会出现种种问题，当哪天我们的开发规范全部都一样的时候，我们就可以完全使用AOP实现半自动化甚至全自动化的开发了

## AOP作用

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd7aa2f1af018e4cd31b990e1c542bc88.png)

最后我们来看看AOP的好处

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE65d3310bcaac2f21cce7c518634b111e.png)

- AOP核心概念

接着我们来学习AOP的相关的核心概念，然后我们来做一个案例去加深我们对AOP的理解

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE257924db4b0f82e2a9167375a44f1559.png)

首先是连接点，我们平常开发时的所有方法都是连接点

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc135887cf871d8f1fd1366521e817aa1.png)

然后我们开发的方法里，如果内部有共性功能，那么该方法就叫做切入点，共性功能本身被称为通知，由于通知本身也是存在差异的，因此不同的通知之间又分有不同的通知类型。切入点和通知之间，也就是被提取出代码之后留白部分，我们称之为切面。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe0830d229b1074131b9d77b0ae97480a.png)

在具体运行时的原始的对象我们称之为是目标对象，原始的目标对象不能够运行出结果，将通知放入切面的过程中我们称之为植入，植入的位置其实是通过原始的目标对象新创建处一个类，然后运行这个类得到的结果，这个新创建对象的过程我们称之为代理，同时这个代理还支持添加一些不存在目标对象中的新功能，我们称之为引入

最后我们来看一个总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEafcdb1fa55819a6d541545756aeab7f0.png)

接着我们就来正式去实现这个案例，实现之前，我们先对这个案例进行分析，因为并不是所有的功能都需要我们去手动完成的，比如说像连接点，我们肯定要做，但这并不属于AOP的内容，实际上我们要做的又属于是AOP的内容就只有三个，分别是切入点、通知和切面

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc9f94ad3980219811474e71f15c77eb3.png)

## 案例制作

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0f184bff1933281f00460523f18c38c2.png)

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

## AOP基本配置

接着我们来具体说说我们之前配置里的内容，首先是Aspect，其意为切面，用于描述切入点与通知间的关系，是AOP编程中的一个概念。而Aspectj则是Aspect的基于java语言的实现

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc05cbe53ce6a635b0962adaed1f71b8a.png)

学习完了这个我们就可以解释我们之前引入的坐标是什么了，可以看到我们这里引入的坐标依赖，其实就是aspectj，也就是java语言对Aspect的实验

```
<dependency>
  <groupId>org.aspectj</groupId>
  <artifactId>aspectjweaver</artifactId>
  <version>1.9.4</version>
</dependency>
```

首先我们来说说aop:config标签，该标签可以用于设置AOP，也就是设置具体的共性功能的加入位置，这里值得一提的是，一个beans标签中可以配置多个aop:config标签，并且他们都能够起作用

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE114d416c4f0cfbc74b7c613b7ab27433.png)

然后我们来讲上一个标签下的子标签，aop:aspect标签，该标签用于设置具体的AOP通知对应的切入点，可以让AOP通知正确地加到我们的切入点上。同样可以设置多个相同的标签，并且他们都会生效

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE512664012555118598eeef4afd513cae.png)

自最后我们来讲aop:pointcut标签，该标签用于设置具体的切入点，其下有两个属性，一个是id，另一个是expression，前者是切入点的唯一标识，后者是对应的切入点表达式。我们可以在一个aop:config标签中配置多个pointcut标签，同时每一个point标签都可以被正确使用，注意这里是正确使用，而不是同时生效

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEec06fc3956a60c0b6782ce595a4fe916.png)

## 切入点表达式

本节我们来讲解我们的切入点表达式，首先切入点表达式是一种快如匹配方法描述的通配格式，其类似于正则表达式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa717bcb95d87b5bfd5da7eb0d5398f75.png)

 其表达式组成为关键字 (访问修饰符 返回值 包名.类名.方法名 (参数) 异常名)。其中关键字描述表达式的匹配模式，其有多种，但是一般都是使用execution，访问修饰符描述的是方法的访问控制权限修饰符，其实就是public那些，而且public可是省略，其中异常名是可以不写的，其他的都一定要写

接着我们来讲切入点表达式的通配符，通配符有三种，分别是*，..和+，其中*是可以单个独立任何符号，也可以独立出现，也可以作为前缀或者后缀的匹配符出现，其意义为一个任意符号，用法非常多，后面我们会慢慢介绍

第二个..，其可以独立出现，常用于简化包名和参数的书写，其意义为多个连续任意符号

最后是+符号，该符号只能用于匹配子类类型，也就是说，使用了这个符号那么就只有继承了某个父类或者是实现了某个接口的类才能够被识别

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4aaffc2054309a94e7f51f93a8c0ffed.png)

我们就以上图中的三个例子为例，我们的第一个例子中，我们能够识别的类是public的范围的，返回值不限的，且在包com.itheima下的任意一个包下的UserService类中的以find开头结尾任意的方法，传入参数有一个，类型不限定

第二个例子中识别的类是public范围的，返回类型为User的在com包下的任意个包下的UserService类中的findByid下的，传入参数的多少和类型都不限定的方法

第三个例子将public省略，识别返回类型不限，在任意包下都可以的以Service结尾的类同时其必须是某一个类的子类，最后方法名不限定，传入的参数的数量和类型都不限定的方法

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE234237c566a5eb49eb0d3d932126c163.png)

切入点表达式还有对应的逻辑运算符，也就是&&、||、和!，&&表达的是连接两个切入点表达式，如果这两个表达式都成立那么就匹配，第二个是任意一个匹配就匹配成功，最后一个是一旦不匹配某一个我们指定的格式，那么其对于我们的而言就是要植入的类。这个其实了解下就好了，不是很重要

值得一提的是，我们最后填写的类不但可以是实现类，还可以是接口，当然能这么做的前提是我们的测试类里写的类型也是接口，不然就寄。

## 三种切入点的配置方式

接着我们来说说切入点的三种配置方式，第一种配置方式是配置公共切入点，其配置在aop:config标签下，其配置要给予id和expression属性，公共切入点的配置的好处在于，一旦配置了一个公共切入点，后面的切点引入都可以使用同一个公共切入点。

第二种配置方式是配置局部切入点，其配置方式和配置公共切入点是一样的，其要配置在aop:aspect下，配置局部切入点是其配置只能够在这一个局部使用

第三种配置方式是直接配置切入点，其直接配置在aop:before等标签内，利用pointcut属性，内部填入我们的切点表达式，这种配置方式只能自己用

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEee7efefc2c9a873effccd05ac8b66704.png)

最后我们来讲下切入点的配置经验，我们企业开发要使用AOP，一定要严格遵循规范文档进行开发，否则就寄了，压根玩不转这个。我们开发时一般都是先配置局部切入点，配置完之后再抽取其中的公共切入点，最后再抽取全局切入点，以此来实现我们代码的复用。最后我们在代码走查，也就是自查过程中，我们要看我们的切入点是否存在越界性包含，也就是不该包含的，我们给包含了，第二个是要检查我们应该包含的是不是没包含到，也就是是否存在非包含性进驻，最后我们要设定AOP执行检测程序，在单元测试监控通知被执行次数与预计次数是否匹配（不过这个测试只是一个测试程序，不能保证完全准确），最后设定完毕的切入点如果发生调整需要进行回归测试，以上规则都是使用于XML配置格式的，当然有一些内容注解中也是适用的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa0f4df09faef377d1ab5be9251dbbdd9.png)

## 五种通知类型配置

接着我们就来讲讲我们的AOP的通知类型，我们之前一直使用的类型都是前置通知类型，也就是在方法执行前，先将通知执行，实际上，AOP的通知类型共有五种，分别是前置通知、后置通知、返回后通知、抛出异常后通知以及环绕通知。具体的不同请看图，接下来我们将对这五种通知进行逐一的讲解

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf2f40ffefc15b402e5020b6911307bf6.png)

首先是前置通知，前置通知是可以配置通过的，其会在原始方法执行前执行，如果在通知过程中抛出异常，那么会阻止原始方法的执行

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE7319e98d00d03feea69dbfb3ee6bbc6d.png)

然后是后置通知，其会在原始方法执行后执行，无论原始方法执行时是否抛出异常，后置通知都会执行

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbbffe58aed007f1202453535147913bf.png)

接着是返回后通知，其也是在原始方法执行后再执行，但是不同的是，如果原始方法中出现了异常，那么该方法就不会执行

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf1103a765a3b562eacdad902650d7e63.png)

然后是抛出异常后通知，这个通知的不同之处在于，如果原始代码中没有抛出异常，那么其就不执行

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6a9742356f7d84393d056d128ff3c6cc.png)

最后是环绕通知

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb06fa9e506a16e4c66d144cc61d71e72.png)

这里我们需要重点讲一下的通知是环绕通知，其是这五种通知中兼容性最强的通知，通过特殊的处理，可以让最后一个通知实现前四个通知的作用。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE21693f92fdc72c693e79cb1948f28e69.png)

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

## 通知顺序

接着我们来学习通知顺序，我们的通知可能有两种导致出现执行顺序的问题的情况，一种是同一种类型的多个通知同时执行，第二种是不同种通知的同时执行，而这两种情况都会出现顺序问题

顺序问题都是按照一个规则解决的，都是配置文件的顺序是什么，其执行的顺序就是什么。同时我这里的执行顺序都是说不发生异常的情况下的，所以抛出异常后通知这里我们就不作考虑了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8c2d6b014a8b74faf9cc1e86d9cc0fbd.png)

## 通知中获取参数

我们接着来学习如何在通知中获取我们方法的参数，我们可以在我们的通知上传入一个JoinPoint对象，调用其中的对应的方法，就可以得到其传入的参数了，其返回的对象会是一个object类型的数组

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE56bc444ad8b0d60a217fa2653a353950.png)

接着我们来讲获取参数的第二种方式，这种方式比较麻烦，存在强绑定，所以我们一般不会使用这种方式，我们了解下就差不多得了。这种方式是在对应的通知类中也写入对应的参数，然后在配置文件中使用逻辑运算与&&（当然，要进行转义），配合args参数，在括号内填入对应的在通知里写入的参数名，然后其就能进行一个正确的给对应的通知类的参数赋值的动作。同时再这个配置文件下还有一个arg-names的属性，该属性可以改变参数转入通知的顺序，使用这个参数的话，我们最开始写的参数最先将值赋予到同名的参数中，然后按顺序赋予到通知的参数里。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4bae82acd9de7f615a59f600a7d0f240.png)

## 通知中获取返回值

在讲解本章内容之前，我们要知道，只有能够准确保证每次运行都能获得返回值的通知，我们才会有获得返回值的操作。这里只有两种通知能稳定获得返回值，一种是返回后通知，另一种是回绕通知

我们一个获取返回值的方式是在对应的通知类中写入传入的参数，类型为Object，当然实际上写其他的也可以，但是如果到时候出现类型不匹配的问题就寄了，所以我们这里推荐的还是Object。然后在对应的原始方法里要令其返回其传入的值。然后在对应的AOP配置中，我们需要通过属性returning写入与通知中设定的参数名一样的名字，其代表会将返回值赋予到此参数中，然后我们就可以 打印这个参数了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE70287f94dc45421fec359a1857ac043e.png)

而环绕通知获取参数就比较简单了，直接通过ProceedingJoinPoint对象获取就完了，执行该对象是会获得一个Object的类型的返回值的，我们直接获取该返回值就完了，同时不要忘了最后要需要将该返回值返回。同时我们这里规定回绕通知的返回值一定要是Object，如果我们的返回类型就void，那么就返回null就可以了

别忘了在对应的配置文件里设置对应的returning属性并给其命名

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd427a87e73386957a1e33f08680fa8e1.png)

## 通知中获取异常对象

首先同样的，我们要确定一定能够获得异常的通知（如果发生异常的话），这两个通知分别是返回异常后通知和环绕通知

返回异常后通知要获取其异常对象的话，同样是要在对应的方法传入参数中定义一个异常对象，在配置文件里调用throwing属性，写入通知中定义的变量名，就可以让我们的异常对象正确传入

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE18eb76127966d435a83f44648297e544.png)

至于环绕通知就直接通过对应的传入对象直接进行一个trycatch就能获得了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4ebf01fffa3c378dc11f031e363341b9.png)

## 注解配置AOP

我们之前学习的是配置文件配置AOP，现在我们来学习使用注解配置AOP，要使用注解配置，首先要开启我们的注解驱动支持，因此我们要在对应的配置上先写入对应的头文件，然后原始功能类直接通过注解来注入，接着spring要抽取功能类也用注解来进行注入，接着是进行aop的具体配置，第一个aop:config不需要任何注解，因为其没有实际意义。接着是配置切面的注解（也就是设定那个类是我们的专门存放我们的具体通知的注解），我们想要配置切面直接就把@Aspect注解放在对应的功能类上就完了，然后配置切入点，切入点需要对应的格式，注解中采取的方法是直接定义一个方法，方法名就作为id，而上面加入@Pointcut注解，注解的括号内填入的是我们的对应格式。最后是配置通知与切入点之间的关系，这里我们采用@Before注解，注解的括号内就填入我们的之前定义的指定的切入点的id，这里和配置文件不同的是多了括号，然后Before代表的是前置通知，实际上还有多种通知方式，该注解下的方法就是我们所抽取出来的共性功能。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1e5d1d2f7749fcc5b35fdffc4ed2a8f6.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE55b78667135f75fbfdd892595f680979.png)

最后我们需要注意的是，我们的切入点不可以定义为一个抽象方法，即使里面没有实际内容。然后我们引入切入点是不可以省略后面的()，最后我们的切入点如果定义在当前类中，那么就只能在当前类中使用，如果想引用其他类中定义的切点，那么就要使用类型.方法名()的方式来引用。最后我们可以再通知类型注解后添加参数，实现XML配置中的属性，但是同样要引入切点，而且这里引入切点的属性名为value，而非id（这说明不引入其他属性的时候我们是将value属性给省略了，但实际赋值仍然是赋值到value中），值得一提的是，方法也要做对应的改动，否则会报错

## 注解AOP执行顺序控制

之前我们讲过在XML文件下的通知执行顺序的问题，本节我们来讲解在注解请看下的顺序控制

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb7b37edcac26ff3698f1b7e0066348d2.png)

首先在注解请看下，通知执行的顺序是不由顺序决定的，而是以通知类型的方法名排序为准，如果是在不同类中，则是以类名排序为准，同时使用Order注解也可以变更bean的加载顺序来改变通知的加载顺序。最后是一些企业开发的知识，有兴趣的可以自己看

## AOP注解驱动

接着最后我们来实现AOP的纯注解驱动实现我们的案例，其实这个案例要实现的功能也就是通过注解来实现我们的Junit

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEea421d99bb8ff8eed17f555fbb9d1f1b.png)

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

## 业务层接口性能监控案例

那么学习完了所有的内容之后，现在我们来做一个综合案例来加深我们的理解，首先，我们案例的所需要的效果是，对业务层接口的每一个性能进行一个监控

### 案例分析

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9598a01050b19e55d720788fe04495dc.png)

我们要测量接口执行的效率，我们的一个简单想法就是获取接口执行前后的执行时间，然后求出执行时长，这里我们最好用环绕通知，然后利用AOP思想动态植入我们的代码，时间我们可以通过proceed()方法来获取

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE67f0a53defdb8816ad8eb0f630bb82cb.png)

最后我们来看看我们的步骤，注意我们的定义切入点是要绑定到接口上的，而不是在接口实现类上的，如果我们绑定在实现类上，那以后我们换一套实现类就寄了，这怎么行呢，所以我们要我们的切入点绑定到接口上

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE552f0dd921a99e12bfbb18099e2b5cb9.png)

### 具体实现

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe6901c27697be9f496218ea940f81ebf.png)

# AOP底层原理

本节我们来讲AOP的三种代理方式，分别是静态代理、Proxy动态代理和CGLIB动态代理。

## 装饰模式

我们先来讲静态代理中的装饰者模式，其意为在不惊动原始设计的基础上，为其添加功能。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE617514a7f6e36eef660339bb09cf9fed.png)

这个东西的案例制作其实非常简单，我们创建一个新对象，然后在新对象中提供一个传入原对象的构造方法，然后我们不使用原对象，以后使用新对象，同时新对象中允许调用原对象的方法，省时间我们可以令其在构造方法中自动调用。这就是装饰者模式，这很好理解

## JDKProxy动态代理

接着我们来讲JDKProxy动态代理的方式，这个方式我们已经在连接池中学习过了，这里我们简单过一遍，如果忘记了对应的内容，可以去连接池的笔记中复习。首先我们去创建对应的UserService接口和其对应的实现类，接着我们进行一个动态代理。所谓动态代理即使将原来的对象由我们的JDKProxy进行代理，其会通过原来的对象生成一个新的存在于内存中的对象，并通过该对象执行对应的增强动作。那么我们可以写入其动态代理代码如下

```
package base.proxy;

import com.itheima.service.UserService;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class UserServiceJDKProxy {

    public static UserService createUserServiceJDKProxy(final UserService userService){

        ClassLoader cl = userService.getClass().getClassLoader();
        //Class[] classes = userService.getClass().getInterfaces();
        Class[] classes = new Class[]{UserService.class};
        InvocationHandler ih = new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                Object ret = method.invoke(userService, args);
                System.out.println("刮大白2");
                System.out.println("贴壁纸2");
                return ret;
            }
        };
        UserService service = (UserService) Proxy.newProxyInstance(cl,classes,ih);
        return service;
    }

}

```

动态代理需要利用Proxy.newProxyInstance方法，该方法内需要传入类加载器，接口数组以及InvocationHandler对象，类加载器和接口都可以通过原来的类获得，而InvocationHandler对象则需要自己去创建，直接重写里面的invoke方法，调用里面的method属性获得一个对象，获得该对象的第19行代码同时也是保证了原来的方法先执行，然后我们执行了后面我们自己的方法之后再将对象传回去，最后我们调用对应的Proxy.newProxyInstance方法之后，就可以获得我们的增强对象，将增强对象返回就行了，接着在测试类中调用方法会正确执行我们的增强的代码

## Cglib动态代理

接着我们就要来讲我们的重量级内容了，就是我们的Cglib动态代理，这是我们本章的最重点。

首先我们来看看CGLIB的好处，首先CGLIB也是一种动态代理的方式，但是其不限定是否具有接口，其可以对任意操作进行增强，且其不需要原始的被代理对象，其可以动态创建处新的代理对象

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE89942d67ffb61fd9bf4cb02e0a445c58.png)

接下来我们来看看其动态代理的过程，首先其需要继承我们的原来的类，这是为了给其后面生成对象做准备，他不去继承原来的类他怎么知道他要创建哪个类不是，然后其生成新的对象，然后其会拦截原来的对象的对应方法，其过程是前执行前增强，然后拦截，接着执行后增强，这样的顺序

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE835bba7457601d2bc9510016525bfc93.png)

那么我们可以构造我们的对应的动态代理代码如下，首先我们创建对应的Enhance对象，然后我们指定其父类，这里为了要指定其父类，因此我们最开始调用该动态代理的方法时就要传入对应的实现类的class文件才可以。但即使是这样我们仍然不能够运行，这是因为我们还没有调用原来的方法，不调用的话原来的方法都不运行那调用个几把。所以我们调用其setCallback方法，该方法可以调用原来的对象的方法，里面需要传入一个 MethodInterceptor对象用于调用，我们直接采用匿名内部类的方式，我们要调用就要调用其父类的对象，因为其父类的对象的方法才是原来的方法，而且我们需要的是代理之后的新的方法，因此我们这里选择methodProxy.invokeSuper方法，该方法可以通过传入对应的新的实现类方法的对象和参数，就可以实现对原始方法的调用。

```
public class UserServiceCglibProxy {

    public static UserService createUserServiceCglibProxy(Class clazz){
        //创建Enhancer对象（可以理解为内存中动态创建了一个类的字节码）
        Enhancer enhancer = new Enhancer();
        //设置Enhancer对象的父类是指定类型UserServerImpl
        enhancer.setSuperclass(clazz);
        //设置回调方法
        enhancer.setCallback(new MethodInterceptor() {
            public Object intercept(Object o, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
                //通过调用父类的方法实现对原始方法的调用
                Object ret = methodProxy.invokeSuper(o, args);
                //后置增强内容，与JDKProxy区别：JDKProxy仅对接口方法做增强，cglib对所有方法做增强，包括Object类中的方法
                if(method.getName().equals("save")) {
                    System.out.println("刮大白3");
                    System.out.println("贴墙纸3");
                }
                return ret;
            }
        });
        //使用Enhancer对象创建对应的对象
        return (UserService) enhancer.create();
    }
}
```

这里我们要注意，我们内部有四个对象，o是代理之后的新的对象，而args是其各种参数，methodProxy则是代理之后的新的类的所有方法的对象，而method是没代理之前的类的所有的方法的对象，我们这里通过调用methodProxy.invokeSuper方法，传入对应的新的对象和参数就可以获得一个新创建的对象，将这个对象返回，最后调用enhancer.create()方法就可以创建出我们想要的增强之后的对象了

这里值得一提的是，如果我们不做任何判断，那么就会对所有方法进行同样的增强，我们可以通过method的对象的getName方法进行判断，令其只对我们的特定方法进行一个增强，这里之所以使用method原来的方法的对象而不是新创建的方法的对象，我猜测是因为新创建的方法的对象的方法名也发生了微妙的改变，因此我们这里需要原来的方法的对象，这样能便于我们的判断

其实我们做完这个我们会发现这个和我之前学习的动态代理方式是非常相似的，那么有两种动态代理方式，我们平时是使用哪种的呢？

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEfc463de9fd3383a0472e7a1cc3a96ba2.png)

我们其实是可以通过配置来选择使用哪种的，注解是通过proxyTargetClass属性，其默认为false，代表使用Proxy代理方式，可以手动改为true，代表使用Cglib代理方式，XML则是proxy-target-class属性，其他的没什么变化，不赘述了

## 植入时机

接着我们来学习本章的最后一个内容，植入时机，具体内容直接看图吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE239ea315445006a60c568738bde6dae3.png)

我们spring中采用的植入时机就是在运行期中植入的，这是一个了解为主的概念，这里就不多提了

**其实本质还得靠反射，反射永远的神**

# 事务管理

那么接着我们学习Spring的内容，数据管理，首先我们对事务这一概念进行一个复习

## 事务基础概念的回顾

首先是关于事务本身的定义

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE446d28e9761e8e3e5f53f9fdd83a4f88.png)

然后是事务的作用

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE36d3d22446b29ca32c8087b1dc5d4f7f.png)

接着我们来了解下事务的特征

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE91a6624e06d2c03e5355aca4a2275809.png)

最后是事务的隔离级别，以及不同隔离级别下所会产生的问题

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe9d962226f16372f2fc655f6cd8a5574.png)

## spring核心事务对象介绍

接着我们来介绍下spring中事务核心对象的分层，对于简单的业务层转调数据层的单一操作，我们放在数据层还是放在业务层都是可以的，但是一旦我们的业务中包含多个对数据层的调用，那么我们就要将事务放在业务层中，这样才能实现一起执行成功，一起执行失败的效果。为了实现这个效果spring给业务层提供了整套了事务解决方案

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE20e8bab47e103d7cf993c795bcddae03.png)

我们首先来讲下平台事务管理器的实现类，spring给我们提供了一个接口，该接口是PlatformTransactionManager接口，该接口的作用就是用于管理平台事务，我们要使用该接口需要使用其具体的实现类，这里我们一般使用DataSourceTransactionManager实现类，其他的实现类以了解为主，具体的区别和作用自己看图吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE4f5537af425a29a385b656aef342b613.png)

我们的接口的实现类内部自然是定义了事务的基本操作，譬如获取事务，提交事务和回滚事务那些。但是这里的获取事务需要我们传入一个事务定义对象，传入之后其会返回一个事务状态对象给我们，然后我们要通过事务状态对象来进行事务的提交和回滚，这是怎么回事呢？其实这里也很好理解，我们要进行提交事务，肯定得保证事务的状态是可以提交的是吧，什么修改都没做提交个几把，回滚也是一个道理，而且这些判断就是要通过事务的状态对象来进行的，通过传入事务的定义对象，就可以获得当时事务的状态对象

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE01caf24edb8836cfd676a098ce64c191.png)

然后我们来看看事务的定义对象，内部有许多最基本的方法，比如获取事务的定义名称，获取事务的读写属性和隔离级别等。这里值得一提的是，一般我们调用这些方法都是用于获取的，而不是修改的，这点要记住。最后像事务的隔离级别，其内部是有各种状态码来表示其不同的隔离级别的，事务传播行为特征这个先按下不表

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3e41e3d82f571066e56040ccc5c42ace.png)

然后我们来看看事务的状态对象，内部有是否处于开启新事物的状态的方法，获取事务是否处于已完成的方法，反正都是很基础又很实用的方法，后续我们会在案例中演示他们，现在具体就直接看下图吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE73c6a53cc99a6e01ffad76be7a370807.png)

## 案例环境介绍

接着我们就来正式制作一个案例来加深我们的理解，首先我们先对我们的案例环境进行一个绍的介。首先我们的事务控制方式有三种方式，分别是编程式、XML声明式以及注解声明式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE07cf9ec7f37147740021942693f440e7.png)

接着我们来看看我们的案例要实现的功能，我们案例要实现的功能就是简单的资金转移，就是A从B转账，非常简单

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3fa89e21e4907f15b86cb5e2674d8ed0.png)

## 具体实现

接着我们就来正式实现这个案例，这需要连接我们的数据库，因此我们首先要在数据中建立对应的数据，这里我们假设已经建立了。首先我们创建对应的domain包，往内部放入我们要将数据封装为对应对象的类，写入代码如下

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

然后创建对应的dao层数据层的包，注意，dao包其实就相当于是mapper包，知道这个知识的主要作用就是可以去看别人的讲解的时候知道他说的mapper包是个什么玩意，我们dao包内写入对应的入账和出账方法，并利用注解传入对应的数据

```
package com.itheima.dao;

import com.itheima.domain.Account;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Update;

import java.util.List;

public interface AccountDao {
    /**
     * 入账操作
     * @param name      入账用户名
     * @param money     入账金额
     */
    void inMoney(@Param("name") String name, @Param("money") Double money);

    /**
     * 出账操作
     * @param name      出账用户名
     * @param money     出账金额
     */
    void outMoney(@Param("name") String name, @Param("money") Double money);

}
```

接着我们定义service层的接口，并写入转账的方法

```
package com.itheima.service;

public interface AccountService {

    /**
     * 转账操作
     * @param outName   出账用户名
     * @param inName    入账用户名
     * @param money     转账金额
     */
    public void transfer(String outName,String inName,Double money);
}
```

然后我们写入对应的实现类，该实现类内有accountDao对象，并提供了对应的setAccountDao方法，内部的转账方法的作用的同时调用一个人的出账的另一个人的入账来实现我们的目标

```
public class AccountServiceImpl implements AccountService {

    private AccountDao accountDao;
    public void setAccountDao(AccountDao accountDao) {
        this.accountDao = accountDao;
    }
    
    public void transfer(String outName, String inName, Double money) {
        accountDao.inMoney(outName,money);
        //int i = 1/0;
        accountDao.outMoney(inName,money);
    }
}
```

然后我们写入对应的配置文件如下，我们这里首先引入了对应的命名空间，然后我们引入了对应的加载文件的驱动，接着对对应的连接类进行了一个注入

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <context:property-placeholder location="classpath:*.properties"/>

    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

    <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
        <property name="accountDao" ref="accountDao"/>
        <!--<property name="dataSource" ref="dataSource"/>-->
    </bean>

    <bean class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <property name="typeAliasesPackage" value="com.itheima.domain"/>
    </bean>

    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.itheima.dao"/>
    </bean>

</beans>
```

然后利用配置给我们的数据类配置对应的标签令其加载到容器中并给予其唯一标识便于后面使用，后面两个bean标签是连接和映射所需要的内容，这里就不多提了，这个懂的都懂

然后是我们jdbc的代码

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://175.178.114.158:3306/spring_db
jdbc.username=root
jdbc.password=itheima
```

最后是我们的具体用于查询的语句的代码，这里我们要注意的是，我们的配置文件的名字一定要是对应的dao包下的类的全名加.xml，而且创建的包的文件夹数量和关系也要和mian中的dao包一致，我们想要命名的话，在resources文件夹中可以用目录/目录/wenjian.xml的形式来创建，其会自动创建对应的目录并生成我们想要的文件，且/还会自动变成.，注意我们这里不能直接用.的形式来代表文件夹的命名，否则的话，其只是生成一个文件夹，只是其名字多了.而已。想必最开始我就是因为这个问题导致我总是出现绑定问题

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.itheima.dao.AccountDao">

    <update id="inMoney">
        update account set money = money + #{money} where name = #{name}
    </update>

    <update id="outMoney">
        update account set money = money - #{money} where name = #{name}
    </update>

</mapper>
```

最后我们可以构造一个App类，写入其测试代码如下

```
package com.itheima;

import com.itheima.domain.Account;
import com.itheima.service.AccountService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.transaction.PlatformTransactionManager;

public class App {
    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        AccountService accountService = (AccountService) ctx.getBean("accountService");
        accountService.transfer("Jock1","Jock2",100D);
    }
}

```

我们运行这个测试代码，我们可以发现这个测试代码是可以运行的，是没有问题的，实际我们的数据也确实发生了改变。但是我们这个程序现在存在的最大的一个问题就是我们这里没有开启事务，如果我们的转账过程中出现了异常，那么我们的数据仍然会发生改变

## 编程式事务

那么接着我们就要实现我们的事务，首先我们要开启事务，开启事务我们需要使用事务管理器PlatformTransactionManager，其具体的实现类取决于我们要正在使用什么框架进行事务管理，我们这里使用的是Mybatis框架，因此我们要使用DataSourceTransactionManager的实现类，该实现类就是专门用于实现Mybatis的。这里开启事务需要将我们的数据源传给我们的实现类，我们传入数据源有两种方式，第一种方式是通过构造方法传入数据源，第二种是通过其实现类的set方法传入数据源，由于我们这里定义的对象是父类对象，其没有设置方法，因此我们这里只能使用构造方法传入数据源。

```
private DataSource dataSource;
public void setDataSource(DataSource dataSource) {
    this.dataSource = dataSource;
}

public void transfer(String outName, String inName, Double money) {
    //开启事务
    PlatformTransactionManager ptm = new DataSourceTransactionManager(dataSource);
    //事务定义
    TransactionDefinition td = new DefaultTransactionDefinition();
    //事务状态
    TransactionStatus ts = ptm.getTransaction(td);

    accountDao.inMoney(outName,money);
    //int i = 1/0;
    accountDao.outMoney(inName,money);

    //提交事务
    ptm.commit(ts);
}
```

传入数据源之后我们要进行事务的定义，事务的定义可以自己定义，也可以使用默认值，所谓事务定义就是指的是事务最开始的隔离级别，超时时间一类的表示规则的设置。得到了事务定义之后，我们通过事务管理器对象ptm中的getTransaction方法，将td传入，然后我们可以返回一个事务状态的对象，该事务状态里各种状态的表示就是按照我们最开始的事务定义的表示来的，同时其具体的状态内容取决于我们数据库中的实际情况。然后我们再执行对应的修改操作，执行完成之后再进行提交事务的操作，提交事务需要调用ptm的commit方法，传入ts事务状态对象就可以成功提交了

当然，不要忘记传入了数据源之后，要在对应的配置文件中给对应标签下表示的类中的数据源对象也就是dataSource进行一个注入的动作，否则会报错

然后我们只要将15行的异常打开，再执行修改动作，我们就会发现我们的数据没有发生改动，此时我们的事务操作就成功了

## AOP改造编程式事务

现在我们的事务还有一个问题，那就是如果我们要以后还有写入新的事务操作，那我们又要复制黏贴，突出一个麻烦低效率，所以我们要使用我们的AOP来改造我们的编程式事务。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEba6e3e8674a351c217fdc0d62d9fe525.png)

首先我们分析我们的原来的程序，我们容易知道，我们无论是执行什么修改事务，开头总是要开启事务，定义事务和获取事务状态的，最后我们总是要进行提交事务，因此我们可以将这一部分的代码都抽取出来做成一个通知，通知的类型当然是采取round环绕通知比较好

然后我们可以创建一个新的通知类，写入其代码如下

```
package com.itheima.aop;

import org.aspectj.lang.ProceedingJoinPoint;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;
import org.springframework.transaction.PlatformTransactionManager;
import org.springframework.transaction.TransactionDefinition;
import org.springframework.transaction.TransactionStatus;
import org.springframework.transaction.support.DefaultTransactionDefinition;

import javax.sql.DataSource;

public class TxAdvice {

    private DataSource dataSource;
    public void setDataSource(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    //开启事务
    public Object transactionManager(ProceedingJoinPoint pjp) throws Throwable {
        //开启事务
        PlatformTransactionManager ptm = new DataSourceTransactionManager(dataSource);
        //事务定义
        TransactionDefinition td = new DefaultTransactionDefinition();
        //事务状态
        TransactionStatus ts = ptm.getTransaction(td);

        Object ret = pjp.proceed(pjp.getArgs());

        //提交事务
        ptm.commit(ts);

        return ret;
    }
}

```

这份代码值得一提的是第28行，我们这里调用pjp.proceed()的同时还往里传入了pjp.getArgs()的参数，实际上这个参数即使不传入也可以，我们最后得到的对象仍然是可以拿到我们原来传入对象的所有参数的，只是这里我们主动传入是一种比较标准的做法，因此我们这么做。还有就是因为我们开启事务还需要数据源对象，因此这里同样将数据源部分的的代码也抽取出来

创建了对应的类之后，不要忘了还要进行对应的配置

```
<bean id="txAdvice" class="com.itheima.aop.TxAdvice">
    <property name="dataSource" ref="dataSource"/>
</bean>

<aop:config>
    <aop:pointcut id="pt" expression="execution(* *..transfer(..))"/>
    <aop:aspect ref="txAdvice">
        <aop:around method="transactionManager" pointcut-ref="pt"/>
    </aop:aspect>
</aop:config>

```

我们的配置里首先对新创建类中的数据源进行了注入，然后我们进行我们的通知类的配置，首先我们配置我们的切点，我们给其唯一标识为pt，然后其所有的切点是任意路径下的具有transfer关键字的方法，接着我们开始配置切面，切面内我们首先引入我们的通知，即是前面配置好的txAdvice，然后我们正式配置配置我们的通知的注入位置，我们这里采取的是环绕通知，要添加的方法就是通知类中的transactionManager方法，要添加通知的位置就是我们之前配置过的pt切点位置

最后我们经过测试会发现我们的事务是成功了的，那么这时我们的AOP重写案例就做好了，此时我们无论写入几个事务，都可以正确运行，当然，这里的前提是，我们的命名是符合规范的，具有transfer关键字的，当然随着业务需求的变动，我们的定位切点的查询语句也可以做相应的改动

- 声明式事务（XML格式）（TX命名空间管理事务）

接着我们要思考一件事情，那就是我们构造的AOP通知是否具有普适性？我们其他构造新的事务的时候，还需要对我们原来的代码做修改吗？我们首先来分析我们原来的通知代码，容易知道每一个事务肯定都需要这些代码的，因此其具有普适性，而在对应的位置上的配置怎么解决呢？我们可以做一个jar包，这样以后我们需要使用就直接导入对应的jar包就完了，因此我们的代码是具有普适性的0

而spring早就知道这一点了，因此其提供了更加便利的实现，该实现就是我们本节要学习的内容。spring提供的这个实现可以让我们的通知代码给省略掉，就是TX命名空间，本节我们要实现的内容就是通过TX命名空间来将我们的通知代码进行一个实现。

首先我们要在对应的配置文件上先引入TX命名空间，然后我们创建定义事务管理的通知类，这里我们使用tx命名空间来创建，其会需要我们传入一个具体的事务管理器的实现对象，因此我们要创建对应的事务管理器的实现对象，我们用一个bean标签来进行创建，我们创建的对象的路径是什么完全取决是我们创建的实现类的是什么，也就是我们当前框架下需要使用什么实现类，接着往该对象内部进行一个数据源对象的注入，然后我们的事务管理对象就创建完了，我们取名为txManager，然后将其传入。

```
<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>

<!--定义事务管理的通知类-->
<tx:advice id="txAdvice" transaction-manager="txManager">
    <tx:attributes>
        <tx:method name="transfer" read-only="false"/>
    </tx:attributes>
</tx:advice>

<aop:config>
    <aop:pointcut id="pt" expression="execution(* *..transfer(..))"/>
    <aop:advisor advice-ref="txAdvice" pointcut-ref="pt"/>
</aop:config>
```

然后我们要在下面再调用tx:attributes标签，往内部指定我们要往内部植入通知的切点方法，这个标签下还有许多属性可以调用，这里我们先按下不表。有的同学可能会问我们下面的切点配置里不是已经指定了对应的要植入通知的方法了吗？为何这里又要多此一举呢？第一点来说这是tx的设计规则，另一点来说之所以这样做是有理由的，现在先按下不表。反正先记住要多指定一次就完了，因此我们这里指定我们要植入通知的方法为transfer

同时相信有细心的同学注意到我们这里的配置对应的通知的aop标签中并没有指定切点类的方法，这是因为spring早就知道你们事务都只是创建，获取，提交，就这一回事，什么事务都一样，因此其内部只有这一个方法可以用，所以也不需要去指定什么方法

接着经过测试我们会发现我们的这个方法是可行的，可用的。那么此时我们就将最开始的通知类的代码也给省略了，我们由TX命名空间帮我们配置好了我们的构造通知的代码

但是实际上我们这样构造我们的TX命名空间的代码是不规范的，实际规范的命名方式应该如下所示

```
<!--定义事务管理的通知类-->
<tx:advice id="txAdvice" transaction-manager="txManager">
    <tx:attributes>
        <tx:method name="*" read-only="false"/>
        <tx:method name="get*" read-only="true"/>
        <tx:method name="find*" read-only="true"/>
        <!--<tx:method name="transfer" read-only="false"/>-->
    </tx:attributes>
</tx:advice>

<aop:config>
    <aop:pointcut id="pt" expression="execution(* com.itheima.service.*Service.*(..))"/>
    <aop:advisor advice-ref="txAdvice" pointcut-ref="pt"/>
</aop:config>
```

我们可以看到我们这里先对命名空间指定了对应的要植入通知的方法，首先是所有不是只读的方法要添加事务，其次是所有只读的方法但是是以get或者是以find开头的方法进行事务管理。然后我们要对我们的要搜索对应的接口的位置进行一个修改，因为如果我们还是跟之前一样的简单粗暴地搜索所有的话，那么其会将所有符合的方法都添加事务，这样就太拖慢我们的效率了，因此我们这里对我们的位置进行一个指定，可以看到我们这里指定了的是返回值任意的，在com.itheima.service包下的所有以Service的接口类，其下的任意方法都需要扫描。这样才是一个符合企业开发的一种命名规范，这样也能够解释我们为什么需要一个新的位置来指定我们的切点，因为此时我们的扫描路径的方法只会限定范围，而不会自动指定好我们的切点。

## aop:advice与aop:advisor

接着我们来讲下aop:advice与aop:advisor的区别

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe5f07adc1f1a845b59352eefc4166013.png)

这两者最简单的区别就是后者必须要指定接口或者其接口的实现类，而前者不用。了解下就好了

然后我们来看看tx:advice的标签的总结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE207c5358f68c1f48ab3df0e313461cbe.png)

然后是tx:attributes标签

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1e8fe7bf35983fe73a3d006985a05b13.png)

最后是tx:method标签

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe477dcaf77e43a7be05e1f639e950ac0.png)

- tx:method属性详解

之前我们说过该标签下的属性很多，那么本节我们就要来对其进行一个逐一的讲解。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE965da70dc52c74fc6ed384eabdb3cbcf.png)

其实也没什么特别好讲的，具体的内容自己看上面的图吧，事务传播行为我们下一节会具体讲

## 事务传播行为

本节我们来讲述事务传播行为，讲述事务传播行为之前我们要先讲两个内容，分别是事务管理员和事务协调员，当在一个存在事务的方法内调用另外一个存在事务的方法里时，调用另一个存在事务的方法的事务叫做事务管理员，而被调用的方法的事务称为事务协调员。

所谓事务传播行为描述的是事务协调员对事务管理员所携带的事务的处理态度

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE71fd08de41048ae3c2c269f2bb6c0ff9.png)

那么什么是事务协调员对事务管理员的态度有什么呢？分别有REQUIRED、REQUIRES_NEW、SUPPORTS、NOT_SUPPORTED等。具体有什么不同，就直接看图吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE46d677f8d53425ea03c96b24df5e2c26.png)

这里我们就不演示了，我们接下来看看我们这些的事务传播对象的具体应用场景吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE41218ff327dd03295b3f192827ac67ad.png)

## 声明式事务（注解）

接着我们要使用注解来实现我们的案例，首先我们要使用@Transactional注解来实现我们的指定类的范围的查询，以及将其对应的接口或者是实现类传给事务管理器的动作。其实一般来说，这个注解下可以给其指定的属性，比如是否只读，超时时间，隔离级别等，同时我们的名字就不再需要了，这是因为我们启用了注解之后其会自动将其下的方法进行绑定，不需要通过名字来特别绑定。这里值得一提的是，我们一般是不会将该注解直接绑定到对应的实现类中的，因为这样绑定的范围太小，以后方法一换又要绑定一次，突出一个麻烦。一般是绑定到接口的抽象方法中，这样实现该接口的所有方法都是会被绑定到。我们也可以将其绑定到接口中，这样该接口下的所有方法都会进行绑定，我们可以再其下具体再进行绑定，设置不同的属性，这样新设置的属性就会覆盖之前设置的属性，这个有点类似于全局变量和局部变量的区别，非常好理解，这里就不多提了。

来看看对应的注解类的配置的写法，如果并不想进行具体的设置，那么是需要在指定的接口上加入@Transactional就可以实现绑定了

```
@Transactional(
        readOnly = false,
        timeout = -1,
        isolation = Isolation.DEFAULT,
        rollbackFor = {}, //java.lang.ArithmeticException.class, IOException.class
        noRollbackFor = {},
        propagation = Propagation.REQUIRED
)
```

最后不要忘了在配置文件定义出我们的事务管理器，同样要求自己创建并传入一个具体的实现类对象，然后才能创建出我们所需要的事务管理器。这里要注意的是，该标签同时还兼具开启事务注解驱动的作用。

```
<tx:annotation-driven transaction-manager="txManager"/>
```

最后我们来看看本节的总结，首先是@Transactional注解

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE81505f6bbb58123cd1785a3c862b3396.png)

然后是tx:annotation-driven

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2798b1f0ff01228fec3f7d54ba7ae1b3.png)

- 声明式事务（注解驱动）

接着我们来学习我们的最后一个内容，也就是经典的纯注解开发。首先我们要对我们的dao层的代码进行改变，将我们原来用xml来完成的查询的语句采用注解的形式来进行实现，那么我们可以写入其代码如下

```
package com.itheima.dao;

import com.itheima.domain.Account;
import org.apache.ibatis.annotations.*;
import org.mybatis.spring.SqlSessionFactoryBean;

import java.util.List;

public interface AccountDao {

    @Update("update account set money = money + #{money} where name = #{name}")
    void inMoney(@Param("name") String name, @Param("money") Double money);

    @Update("update account set money = money - #{money} where name = #{name}")
    void outMoney(@Param("name") String name, @Param("money") Double money);

}
```

然后我们对MyBatis进行相应的注释的代码配置，第一个注释用于指定我们得到的数据要封装的对象以及获取数据连接，需要传入数据源对象。第二个注释用于获得映射对象，指定对应的包我们的程序就能执行对应的语句并获得结果。这两个内容都是在之前讲过的，忘了的可以去复习

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

然后我们配置我们的主注解配置文件，主配置文件中引入了配置文件的固定开头，以及我们的扫描路径，然后是我们的配置文件，最后传入了另外两个副注解配置文件到主注解配置文件中

```
package com.itheima.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;
import org.springframework.context.annotation.PropertySource;
import org.springframework.transaction.annotation.EnableTransactionManagement;

@Configuration
@ComponentScan("com.itheima")
@PropertySource("classpath:jdbc.properties")
@Import({JDBCConfig.class,MyBatisConfig.class})
@EnableTransactionManagement
public class SpringConfig {
}

```

最后我们的@EnableTransactionManagement，该注解其实就相当于我们在配置文件中设置的开启注解驱动的代码，但是在开启注解驱动的代码里还要传入一个数据源对象，那我们应该怎么办呢？我们这里直接将我们的数据源对象注入到我们的spring容器中就可以了，我们可以将这个注入动作放到我们的jdbc的配置文件中

```
package com.itheima.config;

import com.alibaba.druid.pool.DruidDataSource;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;
import org.springframework.transaction.PlatformTransactionManager;

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

    @Bean
    public PlatformTransactionManager getTransactionManager(DataSource dataSource){
        return new DataSourceTransactionManager(dataSource);
    }


}

```

然后我们在数据层中加入对应的注解令其能够加入到spring容器中，以及令其对应的属性可以自动匹配到放到spring容器中的对象。不过这里奇怪的一点是，我们明明没有在AccountDao接口中加入任何的注入相关的注解，但是却没有报错，就很怪，实际加入了也是能够正确运行的，说明可能其自动加入了。这个问题先按下不表，以后可以回来再想想这个问题。这个问题的答案就是这个对应的类是会被动态生成的，其已经先进行了配置了，即使不使用加入到容器中的注解也能够正确运行的原因是因为他一开始就会被用于动态生成新的类然后把那个类放到容器那了。其都使用了映射配置的代码，在注解驱动中是下面这行代码起的作用，而在配置文件开发中则是利用对应的配置标签启动的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9761fc3b83f7f8b1b5e81868ec2f570b.png)

```
package com.itheima.service.impl;


import com.itheima.dao.AccountDao;
import com.itheima.service.AccountService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
@Service("accountService")
public class AccountServiceImpl implements AccountService {

    @Autowired
    private AccountDao accountDao;

    public void transfer(String outName, String inName, Double money) {
        accountDao.inMoney(outName,money);
        //int i = 1/0;
        accountDao.outMoney(inName,money);
    }
}
```

然后我们在对应的接口中加入@Transactional，加入该注解可以让我们的实现该接口的方法都开启默认配置的事务

```
package com.itheima.service;

import com.itheima.domain.Account;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;
public interface AccountService {
    @Transactional
    public void transfer(String outName, String inName, Double money);

}

```

最后我们在对应的测试类中设置专用的类加载器，然后设定spring对应上下文的配置

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

import java.util.List;

//设定spring专用的类加载器
@RunWith(SpringJUnit4ClassRunner.class)
//设定加载的spring上下文对应的配置
@ContextConfiguration(classes = SpringConfig.class)
public class UserServiceTest {

    @Autowired
    private AccountService accountService;

    @Test
    public void testTransfer(){
        accountService.transfer("Jock1","Jock2",100D);
    }
}
```

然后我们经过测试会发现这个的确是可行，此时说明我们的案例就已经完成了。

# 模板对象

现在我们来学习我们本章的最后一个内容，也就是模板对象。我们本章节主要讲两个模板对象，分别是JdbcTemplate和RedisTemplate。

## JdbcTemplate

首先，模板是什么呢？简单来说模板就是一个半成品的东西，类似于框架，有了模板我们开发时只需要关注个性化的功能就可以了，而其他的共性功能我们可以不做修改。spring有许多模板对象，具体请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0df5462ced7c2977639210bfe745f390.png)

以后新技术出现了，这个模板也会更新，我们这里就挑几个演示演示，主要让大家知道这是什么玩意，又到底该怎么上手，本身还是一个了解内容，我们快速过一遍。

首先我们要学习的模板是JdbcTemplate，该模板提供了标准的sql语句用于操作API，具体请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8f05cfa6a26b33838e94b90b7d0e40f1.png)

首先来介绍下我们的案例环境，我们这里提供对应的业务层接口和方法，业务层转调数据层，这没什么好说的，提供对应的JDBC的注解获取连接类，这些就不再赘述了，我们接下来讲讲这个项目里的特殊的地方

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa907ce04f057eae415d603d29cccb98d.png)

我们在JDBCConfig里，不但有将数据源获取到并注入到spring容器中，我们还获得JdbcTemplate的模板对象bean，我们获得的方式很简答，就是直接new，只需要传入一个数据源对象就完了。第二个jdbcTemplate2是具名参数的模板，其也有对应的使用和测试案例，其和前者只有格式上的差别，我们这里就不讲了，有兴趣的自己去课件里去测试

```
//注册JdbcTemplate模块对象bean
@Bean("jdbcTemplate")
public JdbcTemplate getJdbcTemplate(@Autowired DataSource dataSource){
    return new JdbcTemplate(dataSource);
}

@Bean("jdbcTemplate2")
public NamedParameterJdbcTemplate getJdbcTemplate2(@Autowired DataSource dataSource){
    return new NamedParameterJdbcTemplate(dataSource);
}
```

然后我们将该模板类对象jdbcTemplate注入到spring容器中之后，我们就将其注入到我们的数据层实现类中，然后调用其对应的方法。

```
package com.itheima.dao.impl;

import com.itheima.dao.AccountDao;
import com.itheima.domain.Account;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.stereotype.Repository;

import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.List;
//dao注册为bean
@Repository("accountDao")
public class AccountDaoImpl implements AccountDao {

    //注入模板对象
    @Autowired
    private JdbcTemplate jdbcTemplate;

    //调用增加方法，只需要传入对应的sql语句，并传入对应的用于查询的值，用于查询的值通过对象传入
    public void save(Account account) {
        String sql = "insert into account(name,money)values(?,?)";
        jdbcTemplate.update(sql,account.getName(),account.getMoney());
    }

    //删除方法同理
    public void delete(Integer id) {
        String sql = "delete from account where id = ?";
        jdbcTemplate.update(sql,id);
    }

    //更新方法同理，注意传入参数的顺序要保持一致
    public void update(Account account) {
        String sql = "update account set name = ? , money = ? where id = ?";
        jdbcTemplate.update(sql, account.getName(),account.getMoney(),account.getId());
    }

    //单个查询方法调用其queryForObject语句，由于其不知道其要给我们的对象是什么好，因此同时要需要传入我们需要其返回的对象
    public String findNameById(Integer id) {
        String sql = "select name from account where id = ? ";
        //单字段查询可以使用专用的查询方法，必须制定查询出的数据类型，例如name为String类型
        return jdbcTemplate.queryForObject(sql,String.class,id );
    }

    //同样是单个查询方法，但是返回值要求是一个账户对象。由于其需要创建一个用户对象并返回给我们，因此其需要一个将数据映射到对应对象的属性的规则
    //因此我们对其进行了映射解析器的设置，最后将该映射解析器传入即可，其就会按照映射解析器的规则将数据映射上去并返回我们所需要的对象
    public Account findById(Integer id) {
        String sql = "select * from account where id = ? ";
        //支持自定义行映射解析器
        RowMapper<Account> rm = new RowMapper<Account>() {
            public Account mapRow(ResultSet rs, int rowNum) throws SQLException {
                Account account = new Account();
                account.setId(rs.getInt("id"));
                account.setName(rs.getString("name"));
                account.setMoney(rs.getDouble("money"));
                return account;
            }
        };
        return jdbcTemplate.queryForObject(sql,rm,id);
    }

    //实际上我们还可以使用spring自带的映射解析器，直接传入想要的进行映射的对象，其就会自动帮我们完成映射规则的配置
    //能使用的前提是我们的对象按照我们的开发标准的顺序进行了属性的放置
    public List<Account> findAll() {
        String sql = "select * from account";
        //使用spring自带的行映射解析器，要求必须是标准封装
        return jdbcTemplate.query(sql,new BeanPropertyRowMapper<Account>(Account.class));
    }

    //同理
    public List<Account> findAll(int pageNum, int preNum) {
        String sql = "select * from account limit ?,?";
        //分页数据通过查询参数赋值
        return jdbcTemplate.query(sql,new BeanPropertyRowMapper<Account>(Account.class),(pageNum-1)*preNum,preNum);
    }

    //同理
    public Long getCount() {
        String sql = "select count(id) from account ";
        //单字段查询可以使用专用的查询方法，必须制定查询出的数据类型，例如数据总量为Long类型
        return jdbcTemplate.queryForObject(sql,Long.class);
    }
}

```

对应的解释都已经写在了注释上了，有需要就自己去看吧，这里就不再赘述了

最后我们拉看看我们测试程序的代码

```
package com.itheima.service;

import com.itheima.config.SpringConfig;
import com.itheima.domain.Account;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import java.util.List;

import static org.junit.Assert.*;
//设定spring专用的类加载器
@RunWith(SpringJUnit4ClassRunner.class)
//设定加载的spring上下文对应的配置
@ContextConfiguration(classes = SpringConfig.class)
public class AccountServiceTest {
    @Autowired
    private AccountService accountService;

    @Test
    public void testSave() {
        Account account = new Account();
        account.setName("阿尔萨斯");
        account.setMoney(999.99d);
        accountService.save(account);
    }

    @Test
    public void testDelete() {
        accountService.delete(6);
    }

    @Test
    public void testUpdate() {
        Account account = new Account();
        account.setId(7);
        account.setName("itheima");
        account.setMoney(6666666666.66d);
        accountService.update(account);
    }

    @Test
    public void testFindNameById() {
        String name = accountService.findNameById(2);
        System.out.println(name);
    }

    @Test
    public void testFindById() {
        Account account = accountService.findById(2);
        System.out.println(account);
    }

    @Test
    public void testFindAll() {
        List<Account> list = accountService.findAll();
        System.out.println(list);
    }

    @Test
    public void testFindAll1() {
        List<Account> list = accountService.findAll(1, 2);
        System.out.println(list);
    }

    @Test
    public void testGetCount() {
        Long count = accountService.getCount();
        System.out.println(count);
    }
}
```

实际经过测试会发现这些代码都是可以正确使用，就说明的我们的案例已经成功实现了

## RedisTemplate

- RedisTemplate的环境准备

本节的内容其实就是将怎么在java中连接上我们的Reids，我们的Redis因为会被人攻击的问题已经寄了，所以这里就不搞了，我们看看就差不多得了。

这里要解决的问题是需要关闭放在我们Linux中的Redis的保护模式，然后还要绑定对应的ip地址，然后测试下可以成功连接并加入对应的值，那就成功了。

- RedisTemplate

虽然我们花了很多力气将我们的Redis重新设置了一遍且令其终于能被Jedis连接了，但是很不幸的是，由于导入配置的问题，我们仍然无法实际去完成这个案例，尽管我们尝试了许多方法去解决这个问题，但仍然无济于事，只能说我尽力了，这都没法那就实在是没法了，我们接下来直接来讲对应的知识点吧。

首先我们来介绍下我们的案例环境

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEdead10fe3e8cd80d33ba25f0d13d7f21.png)

能注意到我们这里没有dao层，也就是数据层，因为我们的业务代码是直接做到业务层上的。为什么我们这里是直接做到业务层上的，这是因为在redis中，我们的数据不需要进行什么额外的封装，换言之即是数据层本身没什么内容要写了，因此我们这里直接将业务做到我们的业务层上，数据层就给省略掉了。

然后我们来看看我们的RedisConfig配置文件

```
package com.itheima.config;

import org.apache.commons.pool2.impl.GenericObjectPoolConfig;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.PropertySource;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.connection.RedisStandaloneConfiguration;
import org.springframework.data.redis.connection.jedis.JedisClientConfiguration;
import org.springframework.data.redis.connection.jedis.JedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.RedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

@PropertySource("redis.properties")
public class RedisConfig {

    @Value("${redis.host}")
    private String hostName;

    @Value("${redis.port}")
    private Integer port;

    @Value("${redis.password}")
    private String password;

    @Value("${redis.maxActive}")
    private Integer maxActive;
    @Value("${redis.minIdle}")
    private Integer minIdle;
    @Value("${redis.maxIdle}")
    private Integer maxIdle;
    @Value("${redis.maxWait}")
    private Integer maxWait;



    @Bean
    //配置RedisTemplate
    public RedisTemplate createRedisTemplate(RedisConnectionFactory redisConnectionFactory){
        //1.创建对象
        RedisTemplate redisTemplate = new RedisTemplate();
        //2.设置连接工厂
        redisTemplate.setConnectionFactory(redisConnectionFactory);
        //3.设置redis生成的key的序列化器，对key编码进行处理
        RedisSerializer stringSerializer = new StringRedisSerializer();
        redisTemplate.setKeySerializer(stringSerializer);
        redisTemplate.setHashKeySerializer(stringSerializer);
        //4.返回
        return redisTemplate;
    }

    @Bean
    //配置Redis连接工厂
    public RedisConnectionFactory createRedisConnectionFactory(RedisStandaloneConfiguration redisStandaloneConfiguration,GenericObjectPoolConfig genericObjectPoolConfig){
        //1.创建配置构建器，它是基于池的思想管理Jedis连接的
        JedisClientConfiguration.JedisPoolingClientConfigurationBuilder builder = (JedisClientConfiguration.JedisPoolingClientConfigurationBuilder)JedisClientConfiguration.builder();
        //2.设置池的配置信息对象
        builder.poolConfig(genericObjectPoolConfig);
        //3.创建Jedis连接工厂
        JedisConnectionFactory jedisConnectionFactory = new JedisConnectionFactory(redisStandaloneConfiguration,builder.build());
        //4.返回
        return jedisConnectionFactory;
    }

    @Bean
    //配置spring提供的Redis连接池信息
    public GenericObjectPoolConfig createGenericObjectPoolConfig(){
        //1.创建Jedis连接池的配置对象
        GenericObjectPoolConfig genericObjectPoolConfig = new GenericObjectPoolConfig();
        //2.设置连接池信息
        genericObjectPoolConfig.setMaxTotal(maxActive);
        genericObjectPoolConfig.setMinIdle(minIdle);
        genericObjectPoolConfig.setMaxIdle(maxIdle);
        genericObjectPoolConfig.setMaxWaitMillis(maxWait);
        //3.返回
        return genericObjectPoolConfig;
    }


    @Bean
    //配置Redis标准连接配置对象
    public RedisStandaloneConfiguration createRedisStandaloneConfiguration(){
        //1.创建Redis服务器配置信息对象
        RedisStandaloneConfiguration redisStandaloneConfiguration = new RedisStandaloneConfiguration();
        //2.设置Redis服务器地址，端口和密码（如果有密码的话）
        redisStandaloneConfiguration.setHostName(hostName);
        redisStandaloneConfiguration.setPort(port);
//        redisStandaloneConfiguration.setPassword(RedisPassword.of(password));
        //3.返回
        return redisStandaloneConfiguration;
    }

}
```

我们这里搞这么多@Bean都是为了给我们的创建对应的redisTemplate注入其所需的对象，这里就不一一解释了。最后值得一提的是，其实我们可以用其对应的Factory工厂对象来简化我们的代码，不过我们这样做也是可以的。

不要忘记了，我们同时还要提供对应的properties文件用于提供对应的设置数据

```
# redis服务器主机地址
redis.host=175.178.114.158
#redis服务器主机端口
redis.port=6379

#redis服务器登录密码
#redis.password=feigeA.5200....

#最大活动连接
redis.maxActive=20
#最大空闲连接
redis.maxIdle=10
#最小空闲连接
redis.minIdle=0
#最大等待时间
redis.maxWait=-1
```

然后我们就在讲其下所有的方法了，我们通过图片来演示吧，代码上的演示就不做了，我们只要知道其在代码上可以调用这些对应的方法就可以了。

首先我们的RedisTemplate对象结构内部提供了四个操作，分别是客户端基本操作、*Operations、Bound*Operations、以及其他操作。

我们先来看看客户端基本操作都有什么方法，其下的方法大多是做一些改动或者得到某些信息

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc00e69f84809cee2c6544bf2f6ef37c4.png)

而*Operations则可以得到我们的具体的数据结构的设置方法，在其后在可以继续通过点的形式来调用对应的数据结构内部的方法进行数据的设置

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE8503acbaefdf15b5c687e8560b7cf17f.png)

剩下两个自己看吧，本身就用得少，了解下就得了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa4d9375acf3400477ff4a0b2994e70d9.png)



![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEac4b5c571985a7129a01f48f215e6a9d.png)

看完了上面的图之后，我们也容易知道其实我们的redisTemplate就是一个比较方便的工具类，我们可以利用快捷地做到通过java程序连接我们的Linux中的Redis，从而实现对数据的修改

那么我们就可以在对应的业务层中写入其对应的设置代码如下

```
package com.itheima.service.impl;


import com.itheima.domain.Account;
import com.itheima.service.AccountService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Service;
@Service("accountService")
public class AccountServiceImpl implements AccountService {

    @Autowired
    private RedisTemplate redisTemplate;

    public void save(Account account) {
    }

    public void changeMoney(Integer id, Double money) {
        //等同于redis中set account:id:1 100
        redisTemplate.opsForValue().set("account:id:"+id,money);
    }

    public Double findMondyById(Integer id) {
        //等同于redis中get account:id:1
        Object money = redisTemplate.opsForValue().get("account:id:" + id);
        return new Double(money.toString());
    }
}
//        redisTemplate.type()
//        redisTemplate.persist()
//        redisTemplate.move()
//        redisTemplate.hasKey()
//        redisTemplate.getExpire()
//        redisTemplate.expire()
//        redisTemplate.delete()
//        redisTemplate.rename();
//
//        redisTemplate.opsForValue().;
//        redisTemplate.opsForHash().;
//        redisTemplate.opsForList().;
//        redisTemplate.opsForSet().;
//        redisTemplate.opsForZSet();
//
//
//        redisTemplate.boundValueOps().;
//
//        redisTemplate.slaveOf();
//        redisTemplate.slaveOfNoOne();
//
//        redisTemplate.opsForCluster()
```

在这里我们试下了最基本的设置set数据和查找set数据的业务代码，然后我们写入测试代码如下

```
package com.itheima.service;

import com.itheima.config.SpringConfig;
import com.itheima.domain.Account;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import redis.clients.jedis.Jedis;

import java.util.List;

import static org.junit.Assert.*;
//设定spring专用的类加载器
@RunWith(SpringJUnit4ClassRunner.class)
//设定加载的spring上下文对应的配置
@ContextConfiguration(classes = SpringConfig.class)
public class AccountServiceTest {
    @Autowired
    private AccountService accountService;

    @Test
    public void test(){
        Jedis jedis = new Jedis("175.178.114.158",6379);
        jedis.set("name","itheima");
        jedis.close();
    }

    @Test
    public void save(){
        Account account = new Account();
        account.setName("Jock");
        account.setMoney(666.66);

    }

    @Test
    public void changeMoney() {
        accountService.changeMoney(1,200D);
    }

    @Test
    public void findMondyById() {
        Double money = accountService.findMondyById(1);
        System.out.println(money);
    }
}
```

接着我们运行对应的代码，可以看到我们的用例的确成功了。那么就说明我们的案例完成了

## 设计模式

策略模式有很多种，我们可以使用不同的策略对象来实现不同的行为方式。比如在说在我们的第一个JdbcTemplate模板中，我们可以使用自定义的映射解析器，也可以使用spring自带的映射解析器，这两种方式就是两种策略。

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3f6b9e46f35163248c0bbb2a55a37a72.png)

再比如我们的传参中，我们要不要使用具名参数也是一种策略

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEddbbe03bf12cc738bd4a2b2fd4f4a828.png)

在后面的springMVC的学习中，我们还会讲解更多的策略模式，这里我们先了解下。

