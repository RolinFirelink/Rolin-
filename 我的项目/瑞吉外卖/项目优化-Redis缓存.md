那么到现在，我们就正式到优化我们的项目的章节了，我们之前虽然实现了我们的项目，但是我们的项目还存在很多问题，其中最明显的问题就是我们的项目所需要的数据总是往我们的数据库中查找，这样我们的用户数量一旦多起来，系统的访问量一大，我们的系统性能就会下降，最直观的感受就是要等好几秒才能加载数据

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE3470b5218e88203e85f7f32afa4439ca.png)

所以我们要对我们的项目进行改造，我们可以用缓存来改造的项目，这样我们的数据都先存放到缓存中，当用户请求服务器时直接返回缓存的数据即可，这样就不需要我们频繁去请求我们的数据库了，我们这里就使用Redis来进行实现我们的缓存功能

- 缓存短信验证码数据

那么在正式实现之前，我们需要对我们的项目本身做一些环境上的构建，首先我们要在pom文件下引入下面的坐标

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6a74acd9d286352be42af4bdb890357c.png)

然后为了防止我们的存入的缓存数据是乱码的情况，我们需要写入如下配置类代码

```
package com.itheima.reggie.config;

import org.springframework.cache.annotation.CachingConfigurerSupport;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.StringRedisSerializer;

@Configuration
public class RedisConfig extends CachingConfigurerSupport {

    @Bean
    public RedisTemplate<Object,Object> redisTemplate(RedisConnectionFactory connectionFactory){

        RedisTemplate<Object,Object> redisTemplate = new RedisTemplate<>();

        //默认的Key序列化器为:JdkSerializationRedisSerializer
        redisTemplate.setKeySerializer(new StringRedisSerializer());

        redisTemplate.setConnectionFactory(connectionFactory);

        return redisTemplate;
    }
}
```

首先我们来看我们的实现思路

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa00e2c7860924542683491903269561a.png)

在正式实现之前，为了便于我们的查询，我们要整一个用于便捷查询我们的Redis内的数据的软件来，这里我们参考这篇文章https://blog.csdn.net/asdfadafd/article/details/124018451，下载了一个比较不错的软件并且连接上了我们的Redis，以后我们就使用这个来查看我们的Redis数据

首先我们在对应的UserController中注入进行Redis操作的属性对象

```
@Autowired
private RedisTemplate redisTemplate;
```

然后我们注释其中的将验证码保存到Session中的代码，转而设置为将生成的验证码缓存到我们的Redis中，并且设置时间为5分钟

```
//将生成的验证码缓存到Redis中，并且设置有效期为5分钟
redisTemplate.opsForValue().set(phone,code,5, TimeUnit.MINUTES);
```

然后我们在登录的方法中，我们将原来的从共享域中获取对象改为从Redis中获取对象然后进行比对

```
//从Redis中获取缓存的验证码
Object codeInSession = redisTemplate.opsForValue().get(phone);
```

最后如果我们登录成功，我们则删除缓存中的验证码数据

```
//如果用户登录成功，删除Redis中缓存的验证码
redisTemplate.delete(phone);
```

那么到此为止，我们缓存短信验证码的工作就做完了

- 缓存菜品数据

接着我们来学习如何缓存我们的菜品数据，先来看看我们的实现思路

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6f32c35e3a70e5b5c308f479e329ae7d.png)

那么我们就可以修改我们的用户端展示菜品的方法，首先我们先创建所需要的对象，然后我们的目的是让我们的请求在查询数据库之前先查询我们的缓存，如果缓存有数据，我们就使用缓存的数据，如果没有，我们再请求服务器的数据，同时将服务器的数据设置到我们的缓存中，那么我们缓存中的数据肯定需要一个key，那么我们的key要如何构造呢？我们这里采取菜品字符串拼接其分类id及其状态的方法来拼接成我们的key，这样的key不但含有关键信息，重要的是使用者一看就懂这是个什么几把玩意，就很不错

所以我们这里的逻辑是先动态构造我们的key，然后我们通过这个key从redis中获取缓存数据，判断其能否获取到，若获取到则直接返回，否则就从数据库中查找数据，然后将数据设置到我们的Redis中，设置缓存的时间为一小时

```
/**
 * 根据条件查询菜品数据(缓存优化)
 * @param dish
 * @return
 */
@GetMapping("/list")
public R<List<DishDto>> list(Dish dish){

    List<DishDto> dishDtoList = null;

    //动态构造Redis中的key,dish_1397844391040167938_1
    String key = "dish_" + dish.getCategoryId() + "_" + dish.getStatus();

    //先从redis中获取缓存数据
    dishDtoList = (List<DishDto>) redisTemplate.opsForValue().get(key);

    if(dishDtoList != null){
        //如果存在，直接返回，无需查询数据库
        return R.success(dishDtoList);
    }

    //需要查询数据库缓存到Redis
    
    //查找数据库中数据的代码，此处省略
    
    //如果不存在，需要查询数据库缓存到Redis
    redisTemplate.opsForValue().set(key,dishDtoList,60, TimeUnit.MINUTES);
    
    return R.success(dishDtoList);
```

接着我们要明确，如果我们对我们的菜品做增删改，那么我们就要清除我们的缓存数据，清除缓存数据的方法有两种，一种是清除所有缓存数据，另一种是精确清除对应分类的缓存数据，我们当然选择第二种，第一种了解下得了

我们先来看看我们的清除所有缓存数据的方法，这就是利用*号拼接字符串无脑清除所有就完了，没啥好说的说实话

第二种方法则是对应拼接上具体的分类id，从而来清楚对应的分类缓存数据，也很好理解，也是我们所推荐的方法

```
        //清理所有菜品的缓存数据
//        Set keys = redisTemplate.keys("dish_*");
//        redisTemplate.delete(keys);

        //清理某个分类下面的菜品数据
        String key = "dish_" + dishDto.getCategoryId() + "_1";
        redisTemplate.delete(key);
```

然后我们对我们的保存方法也放上同样的代码，当然是要放到我们的保存方法完成之后

对于修改或者是批量修改菜品状态的方法，我们要注意的是，我们这里传入的id是菜品id，而我们存入缓存的数据是分类id拼接的字符串，因此我们这里要先查找出对应的菜品对象，获得其分类id然后再进行缓存数据的删除，由于是批量删除，因此我们这里要用stream流的形式来删

```
ids.stream().map((item) ->{
    DishDto dishDto = dishService.getByIdWithFlavor(item);
    //清理某个分类下面的菜品数据
    String key = "dish_" + dishDto.getCategoryId() + "_1";
    redisTemplate.delete(key);
    return item;
}).collect(Collectors.toList());
```

而删除的方法就比较麻烦了，首先我们要知道我们删除的方法也是传入的菜品id，但是我们这里不可能先把菜品删除了之后再去查找对应的菜品，因此我们这里的方法是改造我们的服务层的方法，令其返回菜品对象的集合

```
//删除菜品信息
public List<DishDto> deleteWithFlavor(List<Long> ids);
```

然后我们在其下做的处理是在stream流中先获得菜品对象，然后将对应数据拷贝到新对象中，接着返回新对象集合即可

```
/**
 * 删除或批量删除菜品
 * @param list
 */
@Override
@Transactional
public List<DishDto> deleteWithFlavor(List<Long> list) {

    List<DishDto> dishDtoList= list.stream().map((ids) -> {
        DishDto dishDto = new DishDto();

        Dish byId = this.getById(ids);

        this.removeById(ids);

        BeanUtils.copyProperties(byId,dishDto);

        //清理当前菜品对应口味数据---dish_flavor表的delete操作
        LambdaQueryWrapper<DishFlavor> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(DishFlavor::getDishId,ids);

        dishFlavorService.remove(queryWrapper);

        return dishDto;
    }).collect(Collectors.toList());

    return dishDtoList;
}
```

最后我们在控制层中的所做的事就是利用传过来的集合对象来删除对应缓存数据即可

```
List<DishDto> list = dishService.deleteWithFlavor(ids);

list.stream().map((item) -> {
    //清理某个分类下面的菜品数据
    String key = "dish_" + item.getCategoryId() + "_1";
    redisTemplate.delete(key);
    return item;
}).collect(Collectors.toList());
```

当然，实际上我们直接返回一个分类id的集合也可以完成我们的功能，但是我们还是返回一个对象比较好，因为指不定以后我们用得上是吧

最后我们提一下关于git的问题，我们的git的使用一般是我们创建一个分支，然后我们在分支上进行开发，开发完成之后我们查看分支功能是否没有问题？没问题我们就直接合并到主支master上，合并的方法就是点击右下角，从分支切换到master分支，然后我们能看到我们的代码就变成没开发时候的样子了，接着我们点击分支，到1.0，然后在点击后面出现的merge即可

- Spring Cache

接着我们来学习SpringCache，利用这个框架我们可以简化我们的缓存代码，首先我们来看看Spring Cache的介绍

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE00f0f5f31ca753bf8dd83e948aa01244.png)

针对不同的缓存技术要实现不同的CacheManager，不过我们这里是使用Redis作为缓存技术的，因为我们现在就是在学这玩意不是，然后我们再来看看SpringCache给我们的提供的常用注解

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEbfccf06363295bdb0518d95f3c7d4a9f.png)

然后我们来学习下SpringCache下的各种注解的使用规则，首先是CachePut注解，其作用是将方法的返回值放入缓存，其下我们需要设置value属性，该属性的作用是设置缓存的名称，而key则是我们要填入的内容，key属性里支持SpEL表达式子，我们可以结合这个表达式来往我们的缓存中存入返回对象内的各种属性，比如名字或者是id，比如下图中就通过EL表达式的方式来动态获得了我们的存入数据的id

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE5ad989bf7a4cc4f5ca57fdf8f4211322.png)

接着我们来看看CacheEvict注解，该注解用于删除指定缓存

value的值需要指定redis中存入数据的名称，而key则是用于指定我们需要具体删除其下的什么数据，同样支持SpEL表达式

我们这里user代表直接拿到user对象，p0表示拿到第一个数据对象，我们也可以使用p1来拿到第二个对象，result表示拿到返回对象，使用root.args[0]也是表示拿到第一个数据对象，我们同样可以使用root.args[1]来拿到第二个对象

虽然说方法多种多样而且都是可行的，但是一般我们都推荐使用最直观的，也就是user的那一种形式来完成目标

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE8c3653222369e9ff2f4618495195caa2.png)

接着我们来看看Cacheable注解，该注解的需要设置value和key，value指的是数据存放的名字，而key是要存放的具体数据，使用这个注解时，每次执行方法时都会先从缓存中查找，若查找到，就直接返回缓存数据，若找不到则将数据设置于缓存数据中，同时如果我们查出的数据为null，其也会将对应的数据插入，只不过值为null而已，为了避免这种情况，我们可以调用其下的condition属性，来指定只有当什么情况下我们才将缓存存入，比如我们这里指定只有当返回值不为空时才将结果存储

这里值得一提的是，我们也还有当满足什么结果时不存入的注解属性可供使用，同时key属性内要求填入的数据是字符串类型的数据，其支持字符串拼接的操作

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEe8e5b0711b32ec0bcb8256fbf97e50ed.png)

 最后我们要学习的就是在Springboot项目中使用SpringCache并且要使用Redis来作为缓存，直接看下图的步骤吧

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE2d0d049595f2a23f5d8bb0bc6a8f802d.png)

接着我们来看看我们的具体步骤

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6db64d87f665c14e730625ef1aab68b2.png)

首先我们要在对应的maven文件中导入下面的坐标，否则我们无法使用

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE44bd373e4fc4609263fc7e64b99dbdaf.png)

然后我们要在yml文件中设置具体的配置

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE4ff8974009cf9de58b710775b5170b77.png)

值得一提的是，弹幕上说还要加上下面这一句的配置，否则会一直报找不到的对应名字的错误，我也不知道是不是，反正我一开始就加了

```
cache:
  type: redis
```

然后是如果我们的缓存时间都设置为一样的话，那么到时候缓存一起过期是会引发缓存雪崩的

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEc155044bb701f79dc9a57730a208aefd.png)

但是我们目前主要的目标不是学习这个，反正我知道会引发这么个玩意就行了，后面我们涉及到了我们会专门去学习如何解决这个问题的

然后我们来用注解来实现下我们的缓存，由于我们的套餐返回的R的对象，因此我们要先将R实现序列化接口，否则R压根就没法序列化到时候存入到Redis中是会报错的

然后我们的逻辑很简单，首先是当查询套餐数据时，如果缓存中有套餐数据我们就返回缓存的套餐数据，如果没有就从数据库中查找数据并且将数据设置到我们的Redis中，那么我们只需要在我们的显示套餐的方法中加入这行代码即可

```
@Cacheable(value = "setmealCache",key = "#setmeal.categoryId + '_' + #setmeal.status")
```

本行代码的意思是我们查询的数据的名字是setmealCache，其key是我们拼接的名字，内部运用了SpEL表达式，注意整个字符串都是由大括号括住，如果要使用EL表达式直接用#结合对象的方式即可，如果要加入自定义的字符串就用''来加入，同时需要引入+号，最后我们会在这个key内部存入我们的对象的所有内容，并且是序列化的形式

接着我们要实现的逻辑是，如果我们对套餐数据进行了修改或者是新增，我们都要清除我们的缓存数据，那么我们就要在新增和修改方法上加入这行注解，可以看到我们这行注解的意思很简单，就是先定位setmealCache的文件夹，然后摄氏allEntries属性为true，意为删除其下所有数据，包括其自身，我们就可以实现我们的删除所有数据的逻辑了

```
@CacheEvict(value = "setmealCache",allEntries = true)
```

本来嘛，其实我们还应该用这个注解去实现我们的套餐的其他逻辑的，但是我觉得学太多这一类的内容要是到时候派不上那就亏了，我们现在还是先放着吧，以后我们真的有需要的时候再来实现也不迟