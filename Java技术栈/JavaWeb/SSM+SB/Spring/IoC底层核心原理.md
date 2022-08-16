- IoC核心接口

我们点开BeanFactory，然后查看里面内部的方法，我们容易看到BeanFactory是一个接口，其下有许多的方法，且其有三个子接口类以及一个实现类

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd44b7e49cfec8a0f2d58ad5b3ad1f0df.png)

我们可以看看在这个接口里都定义了哪些操作，因此我们来看看其对应的方法，我们首先就能看到其getBean的各种重载方法，其定义了多种获取Bean的方式，比如通过名称获取，通过类型获取等，其实我们曾经学过的按名称自动装配和按类型自动装配都和这些方法有关。

接着getBeanProvider是用于获得Bean的提供对象的方法，后面的方法则是分别判断Bean对象是否存在，是否是单例，是否是非单例，获得其类型，获得其配置的别名等等一系列的方法，都在这里了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEf6fe344be9031f63cf7748aa24723535.png)

其实在BeanFactory里的方法就是定义了一大堆有关于Bean的基础信息的方法

如果我们不断点开接口，最后能到达其实现类，但是所谓的实现类其实也就是实现了接口对应的方法，因此我们要查看方法不必去查看其实现类，直接查看对应的接口就行了，这样便于我们的理解和查看

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8a65b58c1b882258bdce87b840a875a9.png)

接着我们来说下，我们可以看到在我们的BeanFactory下有三个子接口，而接口下还有接口，为什么要整这么多接口呢？这其实是为了模拟实际项目中的分层调用的情况。我们都知道在我们的实际项目里是分为三层的，分别是数据层，业务层以及表现层，后者依赖于前者，且只能由后者调用前者，前者处于底层而后者处于高层。我们其实可以将这三层关系看做是一个继承的关系，比如我们可以认为是业务层继承数据层，这样业务层就可以使用数据层的数据，实际上业务层也的确可以使用数据层的数据不是。因此在对应的接口中，我们也做了这样的分层，将我们的容器分为了三层（数据层、业务层和表现层分别一个容器），也就是三个接口，其有着对应的继承关系，这样底层的接口就可以调用其父接口的对应方法，这是一个设计理念，了解了这个理念我们就知道为什么其要将接口搞得这么复杂

然后再来看HierarchicalBeanFactory，先来看看方法，我们可以看到containsLocalBean方法，该方法就是用于判断对应的Bean类型是否在同一层级中的方法。使用这个方法就可以做一个层级划分

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE948e266ab10cc666bf812fa0b945ee45.png)

接着我们来说AutowireCapableBeanFactory接口，光靠名字我们都可以猜到这个接口的主要作用就是用于自动装配的，我们不妨来看看里面的方法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE02d44eef63637b58ef95d79bfd3209ff.png)

我们可以看到的创建Bean的方法，然后是自动装配的方法，其下还可以设置参数，可以看到0是不装配，1是按名称自动装配，2是按类型自动装配，3是按构造器自动装配。我们容易知道如果我们写入@Autowire然后不指定参数那么其就是按类型自动装配，如果给定名称那么就会按名称自动装配

最后我们来看下其主要接口中的最后一个接口，也就是ListableBeanFactory接口，其翻译过来的意思就是可以列表化的Bean工厂，先来看看其方法

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb2827f2bd2b58fee1693a64c6519d944.png)

只要看到其方法，我们就啥都明白了，可以看到其下有通过Bean的类型返回其数量，通过Bean的类型返回其名字，或者是通过注解返回被该注解修饰过的Bean的数量等等，证明这个接口的主要作用就是来获得Bean的各种列表信息，用于查询整体的Bean的情况的一个接口

接着我们来看看HierarchicalBeanFactory的子接口，第一个接口ConfigurableBeanFactory的主要作用就是用于配置的工程对象，我们的工程对象也是需要配置的，比如它的范围。然后其下还有ConfigurableListableBeanFactory接口，这个接口其实就是可列表化的进一步实现。同样作为HierarchicalBeanFactory的子接口的还有ApplicationContext接口，其作用就是在BeanFactory之上又追加了一些功能，比如说国际化，就是通过这个接口进行实现的

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEb480474c5ba36969e8d86583c74705bc.png)

剩下的两个接口的子接口的内容都大差不差了，这里就不再提了，最后我们可以做一个总结

首先是BeanFactory接口

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEfbac4785c8983b6045e8917707b57889.png)

然后是HierarchicalBeanFactory接口

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc0731d77326233b321ec725603eed807.png)

接着是AurowireCapableBeanFactory

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE9201249a05f3cbaf66a14560c7a10e00.png)

最后是ListableBeanFactory

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE609bb01fc478dbbc0a4e84bfdf71df07.png)

- 组件扫描过滤器

在我们的开发过程中，有时候我们只需要测试一部分的Bean，而另一部分的Bean是不用加载的，甚至是不能加载的，因为其还没有开发完全，一加载就报错。在实际的开发场景中，这是经常可能会发生的，因此我们要认真学本节的内容。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6ac312dcd642c2189efe65b14e0c12a7.png)

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE0d8f46c36c3bf820359e85aab9bae8d1.png)

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd83818208274906144da95a11b602c7a.png)

- 自定义导入器

回忆一下我们之前导入bean的方式，不是通过注解就是通过配置文件，这两种方式的特点都是一个个导入的，但是都是一个个类导入的，我们现在期待一种能够群体导入的方式，此时就需要自定义一个导入器了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe14de4b934d10bd5916be685f4e132c2.png)

那么我们就怎么自定义一个导入器呢？实现ImportSelector接口就可以了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2c784639a98cacfd53f6d459c22fe1cf.png)

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

- 自定义注册器

我们之前使用@ComponentScan("com.itheima")来定义我们注解使用的位置并获取注解指定的对象，现在我们要通过一个自定义的注册器来模拟这个注解的功能，其需要实现ImportBeanDefinitionRegistrar接口

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe6e873aef47b93e6fc5bddb4a7dea5fd.png)

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

- bean初始化过程解析

本章的最后一个内容，就是对bean的初始化过程进行一个解析。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc439444d4688184f246328e11a38f789.png)

我们不妨先回忆下我们的创建对应的bean的过程，我们首先要创建一个类，然后对其进行初始化，提供对应的set方法（如果它需要提供的话），还有无参构造方法，然后我们的Bean每次初始化时都要创建执行初始化动作，也就是initMethod。那么假设现在有这么一个需求，我们需要每次创建Bean和创建Bean结束的时候都对Bean执行一个同样的动作，那么为了满足这个需求，其就为我们提供了BeanPostProcessor接口，该接口就可以实现这个功能，同时为了防止我们忘了实现这个功能，其还为每个初始化功能提供了一个InitializingBean接口，这个接口的作用就是防止初始化过程中我们忘记去给对应的初始化指定对应的功能而存在的接口，其与initMethod的作用可以说是相同的，但是他们并不冲突，可以共存，不过一般我们只写一个，同时前者必须写，后者可写可不写，且后者的名字是不固定的。同时为了满足工厂对象初始化时执行一个固定动作的需求，其还提供了BeanFactoryPostProcessor接口，该接口可以满足此要求

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE95562ee79f3a168cbb3af57ab9a09b3e.png)

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE0291bb06d5676be92b3a388283eb4b1d.png)



![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8cce0be0e1c9a37f1d8af203b0ccbff1.png)

这个了解下就好了吧，直接看图吧，这里就不带着大家做一遍了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE864f87a921d79718ed8a7ba9c7773b66.png)

