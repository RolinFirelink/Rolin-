本章节我们来学习Jedis，什么是Jedis呢？其实Jedis就是连接我们的Redis的一种Java程序的称呼

- HelloWorld

Jedis用于Java语言连接redis服务，并提供对应的操作API

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6a5b91a3786bc486f062ddbbdf43b9a6.png)

除了Jedis以外，还有很多的Java语言可以连接redis服务，不过我们这里就不提了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEd9605f6b744431333b79b1905f94402c.png)

接下来我们要连接到我们的redis中，我们首先要导入对应的jar包，然后我们调用其对应的方法获取其连接对象，那么我们容易构造代码如下

```
public class JedisTest {
    public static void main(String[] args) {
        //1.获取连接对象
        Jedis jedis = new Jedis("127.0.0.1",6379);
        //2.执行操作
        jedis.set("name","itheima");
        //3.关闭连接
        jedis.close();
    }
}

```

但是这一份代码却会报错，这是为什么呢？这是因为我的Redis是装载在虚拟机上，其有一个对外的虚拟ip，我们要用这个虚拟ip连接才可以，进入Xshell，输入ip addr，可以查找到我们的虚拟ip是10.0.8.11，那么我们可以将我们的代码改造如下

```
public class JedisTest {
    public static void main(String[] args) {
        //1.获取连接对象
        Jedis jedis = new Jedis("10.0.8.11",6379);
        //2.执行操作
        jedis.set("name","itheima");
        //3.关闭连接
        jedis.close();
    }
}
```

不过由于我们是云服务器而非虚拟机，因此我们的代码其实是应该修改如下

```
package com.itheima;

import redis.clients.jedis.Jedis;

public class JedisTest {
    public static void main(String[] args) {
        //1.获取连接对象
        Jedis jedis = new Jedis("175.178.114.158",6379);
        //2.执行操作
        jedis.set("name","itheima");
        //3.关闭连接
        jedis.close();
    }
}
```

但是即使如此，我们的这份代码还是不能连接，会报另外一个错误，其会说我们的Redis在运行时被保护了，那我们应该怎么办呢？这其实是因为最开始我们的Redis服务的配置文件搞了保护所致，我们只要将对应的redis服务的配置文件的第一个bind端口号设置成其虚拟端口号就行了。

最后我们经过测试会发现这个的确是可以使用的，没有问题的，此时就说明我们的Redis已经成功被我们的Jedis连接了

最后我们的Jedis有一个基于maven的配置文件，这个先看看就行，因为以后我们要学maven的

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6b43b1736ec449835a0cccec13e8851b.png)

最后我们来看看我们的步骤总结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE1697bbb76276603a51acfd4216ef1076.png)

- 工具类开发

我们之前学习了连接redis，但是我们之前的连接都是将我们的代码写死的，这样当然不便于我们的开发，我们有一个想法就是将其写在一个工具类中，不但如此，而且还要提供连接池的内容，那么最后可以构建代码如下

```
package com.itheima.util;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.JedisPoolConfig;

import java.util.ResourceBundle;

public class JedisUtils {

    private static int maxTotal;
    private static int maxIdel;
    private static String host;
    private static int port;

    private static JedisPoolConfig jpc;

    private static JedisPool jp;

    static {
        ResourceBundle bundle = ResourceBundle.getBundle("redis");
        maxTotal = Integer.parseInt(bundle.getString("redis.maxTotal"));
        maxIdel = Integer.parseInt(bundle.getString("redis.maxIdel"));
        host = bundle.getString("redis.host");
        port = Integer.parseInt(bundle.getString("redis.port"));

        //Jedis连接池配置
        jpc = new JedisPoolConfig();

        jpc.setMaxTotal(maxTotal);//设置连接池的最大的数量
        jpc.setMaxIdle(maxIdel);//设置连接池里的活动连接数量

        //连接池对象
        jp = new JedisPool(jpc,host,port);
    }

    public static Jedis getJedis() {

        //主机IP
        //String host = "175.178.114.158";
        //端口
        //int port = 6379;


        return jp.getResource();
    }
}

```

这里我们用创建一个配置文件的方式引入我们的连接，配置文件的内容如下

```
redis.maxTotal=50
redis.maxIdel=10
redis.host=175.178.114.158
redis.port=6379
```

这里将对应的给连接池设置的代码，设置连接池里数量的代码，创建连接池的代码都放在了静态代码块里，而只留下返回连接池中连接的对象的代码到方法中，这样才是符合我们开发时要设置的代码方式

最后我们来看看总结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE09b6a3ba843a4305727754ee8ede2c89.png)



![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE113e66cbbd8549a592bf1212a9957679.png)



![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE79a857f56130d6792b8dae4f4611613c.png)



![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe502d92c0157f233a2565aca42531454.png)

- 可视化工具

这个工具的名字就叫做Redis Desktop Manager，这个可视化工具的使用很简单，我相信以后也有更加简单的可视化工具给我们使用，这里我们只是提一提有这种工具就行了

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEda389f4cea4059b25ecedc9bdeebf33c.png)

