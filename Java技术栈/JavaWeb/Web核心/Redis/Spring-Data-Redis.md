本节我们来学习使用SpringDataRedis来在idea上对Redis进行对应操作，之前我们虽然学习的Jedis，不过都没人用了，了解下就差不多得了，都什么年代了，谁还用传统Redis啊，大伙们都用SpringBoot框架来开发项目了都

当然这里值得一提是，我们这个内容是必须要在我们学习完SpringBoot相关内容之后再来学习的

先来看看Spring Data Redis中给我们提供了些啥

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE7e67f328f9c866323c366dc08ac8e5e2.png)

然后我们就来正式进行学习，首先我们要创建一个Springboot项目，在其下引入对应的依赖如下

```
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

这里我们引入的依赖分别是测试依赖以及我们的Springdataredis的依赖，前者用于测试，后者用于使用

别忘了我们的Springboot的版本号必须是2.5.4

接着我们在resource文件夹中创建对应的application.yml文件，并且写入其代码如下，下面的代码无非是对我们的Redis的连接做一些设置罢了

```
spring:
  application:
    name: springdataredis_demo
  #Redis相关配置
  redis:
    host: 175.178.114.158
    port: 6379
    password: feigeA.5200....
    database: 0
    jedis:
      #Redis连接池配置
      pool:
        max-active: 8  #最大连接数
        max-wait: 1ms #连接池的最大阻塞等待时间
        max-idle: 4 #连接池的最大空闲连接
        min-idle: 0 #连接池中的最小空闲连接
```

接着我们写入我们的测试类如下

```
package com.example.demo;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.test.context.junit4.SpringRunner;

@SpringBootTest
@RunWith(SpringRunner.class)
public class SpringdataredisDemoApplicationTests {

    @Autowired
    private RedisTemplate redisTemplate;

    @Test
    public void testString(){
        redisTemplate.opsForValue().set("city567","beijing");
    }
}
```

我们这里用自动装配的形式获得RedisTemplate对象，之所以可以自动获得，是因为我们的SpringBoot容器在启动时就会创建该类并管理，然后我们想要往里面写入字符串的数据，则需要调用其下的opsForValue，其是最简单的K-V的数据存入，再调用其下的set方法即可存入，但是这样存入的数据在我们的redis中是乱码的，原因是我们的内容在存入时会使用序列化器，导致我们在redis中无法获得我们想要的内容，对此我们的解决方式是创建一个配置类并写其代码如下

```
@Configuration
public class RedisConfig extends CachingConfigurerSupport {

    @Bean
    public RedisTemplate<Object,Object> redisTemplate(RedisConnectionFactory connectionFactory){

        RedisTemplate<Object,Object> redisTemplate = new RedisTemplate<>();

        //默认的Key序列化器为:JdkSerializationRedisSerializer
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());

        redisTemplate.setConnectionFactory(connectionFactory);

        return redisTemplate;
    }
}
```

可以看到我们这里所做的事是将默认的序列化器改为String类型的序列化器，而且我们只对一些对应对象进行设置，比如我们这里只设置了key，那么value就仍然还是要进行对应的序列化的。我们这样设置，最后我们就可以在redis中得到我们的不乱码的key了

但是我们这里不知道为什么一直报出找不到connectionFactroy的错误，去百度了好久也没找到好使的解决办法

所以我们这里暂时的解决方式就是将我们最初的RedisTemplate对象改为RedisTemplate<String,String>，或者是将RedisTemplate对象改为StringRedisTemplate对象亦可，总之这样改动了之后我们就暂时不需要配置类也可以正确在redis中查看到我们的数据了

那么最后我们可以写入我们的方法如下，我们要进行Redis简单的K-V操作，我们首先要获得redisTemplate中的ValueOperations对象，然后我们可以调用其下的对应方法

```
/**
 * 操作String类型数据
 */
@Test
public void testString(){
    ValueOperations<String, String> valueOperations = redisTemplate.opsForValue();

    valueOperations.set("city567","beijing");

    String s = valueOperations.get("city567");
    System.out.println(s);

    valueOperations.set("key1","value1", 10L, TimeUnit.SECONDS);

    Boolean aBoolean = valueOperations.setIfAbsent("city567", "nanjing");

    System.out.println(aBoolean);
}
```

可以看到我们这里调用了set方法，以及set并且设置超时的方法以及其下的如果存在再设置的方法

然后我们来学习操作Hash类型的数据，我们这里首先要嗲用redisTemplate的opsForHash方法，获得其下的HashOperations对象

```
/**
 * 操作Hash类型数据
 */
@Test
public void testHash(){
    HashOperations hashOperations = redisTemplate.opsForHash();

    //存值
    hashOperations.put("002","name","xiaoming");
    hashOperations.put("002","age","20");
    hashOperations.put("002","address","bj");

    //取值
    String age = (String) hashOperations.get("002","age");

    System.out.println(age);

    //获取hash结构中的所有key字段
    Set keys = hashOperations.keys("002");
    for (Object key:keys) {
        System.out.println(key);
    }

    //获得hash结构中的所有值
    List values = hashOperations.values("002");
    for (Object value:values) {
        System.out.println(value);
    }
}
```

容易看到我们这里做的事情也就是往里面存值取值而已，这些我们就不多赘述了

接着我们来学习如何往list中存入数据，首先我们要调用其下的opsForList方法，获得ListOperations对象，然后我们可以调用里面的左推和右推方法来往里面添加数据，也可以一次推送多个数据，当然也可以取值和从中弹出数据，这些都是老东西了，不多谈了

```
/**
 * 操作List类型的数据
 */
@Test
public void testList(){
    ListOperations listOperations = redisTemplate.opsForList();

    //存值
    listOperations.leftPush("mylist","a");
    listOperations.leftPushAll("mylist","b","c","d");

    //取值
    List<String> mylist = listOperations.range("mylist",0,-1);
    for (String s:mylist) {
        System.out.println(s);
    }

    //获得列表长度 llen
    Long size = listOperations.size("mylist");
    int lSize = size.intValue();
    for (int i = 0; i < lSize; i++) {
        //出队列
        String element = (String) listOperations.rightPop("mylist");
        System.out.println(element);
    }
}
```

然后是操作Set类型的数据，这个我们自己看就行了

```
/**
 * 操作Set类型的数据
 */
@Test
public void testSet(){
    SetOperations setOperations = redisTemplate.opsForSet();

    //存值
    setOperations.add("myset","a","b","c","a");

    //取值
    Set<String> myset = setOperations.members("myset");
    for (String s:myset) {
        System.out.println(s);
    }

    //删除成员
    setOperations.remove("myset","a","b");

    //取值
    myset = setOperations.members("myset");
    for (String s:myset) {
        System.out.println(s);
    }
}
```

接着是Zset集合，这里值得一提的是，该数据集合的默认排序是从小到大的

```
/**
 * 操作ZSet类型的数据
 */
@Test
public void testZset(){
    ZSetOperations zSetOperations = redisTemplate.opsForZSet();

    //存值
    zSetOperations.add("myZset","a",10.0);
    zSetOperations.add("myZset","b",11.0);
    zSetOperations.add("myZset","c",12.0);
    zSetOperations.add("myZset","a",13.0);

    //取值
    Set<String> myZset = zSetOperations.range("myZset",0,-1);
    for (String s:myZset) {
        System.out.println(s);
    }

    //修改分数
    zSetOperations.incrementScore("myZset","b",20.0);

    //取值
    myZset = zSetOperations.range("myZset",0,-1);
    for (String s:myZset) {
        System.out.println(s);
    }

    //删除成员
    zSetOperations.remove("myZset","a","b");

    //取值
    myZset = zSetOperations.range("myZset",0,-1);
    for (String s:myZset) {
        System.out.println(s);
    }
}
```

最后则是一些通用的操作，是一些针对全局的命令，我们了解下就好

```
/**
 * 通用操作，针对不同的数据类型都可以操作
 */
@Test
public void testCommon(){
    //获取Redis中所有的key
    Set<String> keys = redisTemplate.keys("*");
    for (String key : keys) {
        System.out.println(key);
    }

    //判断某个key是否存在
    Boolean itcast = redisTemplate.hasKey("itcast");
    System.out.println(itcast);

    //删除指定key
    redisTemplate.delete("myZset");

    //获取指定key对应的value的数据类型
    DataType dataType = redisTemplate.type("myset");
    System.out.println(dataType.name());
}
```

