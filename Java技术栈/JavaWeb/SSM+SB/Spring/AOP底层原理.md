本节我们来讲AOP的三种代理方式，分别是静态代理、Proxy动态代理和CGLIB动态代理。

- 装饰模式

我们先来讲静态代理中的装饰者模式，其意为在不惊动原始设计的基础上，为其添加功能。

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE617514a7f6e36eef660339bb09cf9fed.png)

这个东西的案例制作其实非常简单，我们创建一个新对象，然后在新对象中提供一个传入原对象的构造方法，然后我们不使用原对象，以后使用新对象，同时新对象中允许调用原对象的方法，省时间我们可以令其在构造方法中自动调用。这就是装饰者模式，这很好理解

- JDKProxy动态代理

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

- Cglib动态代理

接着我们就要来讲我们的重量级内容了，就是我们的Cglib动态代理，这是我们本章的最重点。

首先我们来看看CGLIB的好处，首先CGLIB也是一种动态代理的方式，但是其不限定是否具有接口，其可以对任意操作进行增强，且其不需要原始的被代理对象，其可以动态创建处新的代理对象

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE89942d67ffb61fd9bf4cb02e0a445c58.png)

接下来我们来看看其动态代理的过程，首先其需要继承我们的原来的类，这是为了给其后面生成对象做准备，他不去继承原来的类他怎么知道他要创建哪个类不是，然后其生成新的对象，然后其会拦截原来的对象的对应方法，其过程是前执行前增强，然后拦截，接着执行后增强，这样的顺序

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE835bba7457601d2bc9510016525bfc93.png)

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

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE239ea315445006a60c568738bde6dae3.png)

我们其实是可以通过配置来选择使用哪种的，注解是通过proxyTargetClass属性，其默认为false，代表使用Proxy代理方式，可以手动改为true，代表使用Cglib代理方式，XML则是proxy-target-class属性，其他的没什么变化，不赘述了

- 植入时机

接着我们来学习本章的最后一个内容，植入时机，具体内容直接看图吧

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEfc463de9fd3383a0472e7a1cc3a96ba2.png)

我们spring中采用的植入时机就是在运行期中植入的，这是一个了解为主的概念，这里就不多提了