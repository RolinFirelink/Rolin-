- CSS的引入方式，CSS的引入方式总共有三种，分别是内联样式，内部样式和外部样式

先来讲讲内联样式，内联样式是在标签中直接通过style属性来控制样式，比较简单，但是缺点是只能影响当前这一行，对其他行无效

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa8df9d62affb9fa5f8534d5a53b44cd3.png)

根据上文所述我们可以写入演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>引入方式1</title>
</head>
<body>
    <!--内联样式-->
    <h1 style="color: red; font-size: 20px">Hello World</h1>
    <h1>CSS</h1>
</body>
</html>
```

接着我们来讲第二种方式，内部样式，其实内部样式我们已经用多了，就是在<head>标签下加入style标签进行设置而已，这里就不再赘述了。其比起第一种方式来说更加麻烦，但是灵活性更好

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE1a84a87ab2eb70f3b796aaab61705e11.png)

我们可以写入演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>引入方式2</title>
    <!--内部样式-->
    <style>
        div{
            color: red;
            font-size: 20px;
        }
    </style>
</head>
<body>
    <div>div1</div>
    <div>div2</div>
</body>
</html>
```

那么现在我们来讲第三种方式，外部样式，外部样式是在标签中通过<link>标签来引入独立的css文件，可以影响不同的文件，其具有最高级别的灵活性，也是我们最为推荐的css引入方式

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE448a40028896e3ef442d1a0e5e5758c9.png)

使用这种方式之前我们必须要先创建一个名叫css的文件夹，里面存放各种css格式的文件，我们就在css文件中编写我们的样式，比方说我们编写样式如下（在css文件中可以直接编写样式，无需写入style标签）

```
div{
    color: red;
    font-size: 20px;
}
```

接着我们在HTML文件中使用外部样式引入这个css文件，其演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>外部样式</title>
    <!--外部样式-->
    <link rel="stylesheet" href="css/01.css">
</head>
<body>
    <div>div1</div>
    <div>div2</div>
</body>
</html>
```

这里我们利用外部样式时要注意的是我们的link标签是定义在头部的，而且其下的rel属性统一写stylesheet，接着href标签则直接写入我们的css文件的地址就可以引入了

我们平时我们都推荐使用第三种方式来使用css，因为其灵活性是最高的

- CSS的注释，这个没什么特别只得讲的，直接说重点。CSS的注释方式是/* */，如果我们的注释是上面的形式，则说明目前我们是在对css进行修改，反之则说明我们是在对HTML进行修改

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE96cde722b61a23a50f3e4c021ad0f90e.png)

- CSS的选择器

什么是选择器呢？简而言之就是用于选择HTML的指定元素（标签）的

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEfe09232542467f1705134bab74f4a07a.png)

CSS的选择器也不止有一种，这里我们先来介绍基本选择器，基本选择器有三种，分别是以标签名来选择的元素选择器，以class属性值来选择的类选择器，以id属性值来选择的id选择器，其优先级是逐渐增加的，最后的优先级最高（简单记法就是越具有通用性的选择器其优先级越低）

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEa48696ec6d0ab600de0afa497db9f40d.png)

我们可以写出演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>基本选择器</title>
    <style>
        /*元素选择器*/
        div{
            color: red;
        }

        /*类选择器*/
        .class{
            color: blue;
        }

        /*id选择器*/
        #d1{
            color: green;
        }

        #d2{
            color: pink;
        }

        #d3{
            color: aqua;
        }

    </style>
</head>
<body>
    <div>div1</div>

    <div class="class" id="d3">div2</div>
    <div class="class">div3</div>

    <div id="d1">div4</div>
    <div id="d2">div5</div>
</body>
</html>
```

接着我来讲CSS的第二种选择器，属性选择器

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE9850929b5afbccad273ad36fbb2ad576.png)

属性选择器也分为两种，第一种方式直接选择属性的方式会将所有具有对应属性的元素进行样式修改，第二种则只会将具有对应元素且内容也相同的元素进行样式修改

我们可以写出演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>属性选择器</title>
    <style>
        [type]{
            color: red;
        }

        [type="password"]{
            color: blue;
        }
    </style>
</head>
<body>
    用户名:<input type="text"/> <br/>
    密码：<input type="password"/> <br/>
    邮箱:<input type="email"/> <br/>
</body>
</html>
```

然后我们来讲第三种选择器，伪类选择器，该选择器一般与超链接进行配合使用

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCEbdeaa69df04278c1d3cee4df9bebc32f.png)

那什么是伪类选择器呢？直接举例吧，比如说我们想要让超链接未被点击时显示黑色，鼠标悬浮在超链接上显示红色，鼠标点击超链接时显示黄色，超链接被点击之后选择蓝色，这些都要通过伪类选择器来进行实现

我们可以写入演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>伪类选择器</title>
    <style>
        a{
            text-decoration: none;
        }

        /*未访问的状态*/
        a:link{
            color: black;
        }

        /*已访问的状态*/
        a:visited{
            color: blue;
        }

        /*鼠标悬浮的状态*/
        a:hover{
            color: red;
        }

        /*已选中的状态*/
        a:active{
            color: yellow;
        }
    </style>
</head>
<body>
    <a href="http://www.bilibili.com" target="_blank">哔哩哔哩</a>
</body>
</html>
```

最后我们来学习组合选择器

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE1eebc0f6bc7cc1da787044785dc3a9ff.png)

组合选择器有两种，一个是可以用于选择某个标签内的更下一级的标签的后代选择器，另外一个是可以同时选择多个元素的分组选择器

接着我们可以写入演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>组合选择器</title>
    <style>
        /*后代选择器*/
        .center li{
            color: red;
        }

        /*分组选择器*/
        span,p{
            color: blue;
        }
    </style>
</head>
<body>
    <div class="top">
        <o1>
            <li>aa</li>
            <li>bb</li>
        </o1>
    </div>

    <br/>

    <div class="center">
        <o1>
            <li>cc</li>
            <li>dd</li>
        </o1>
    </div>

    <span>span</span> <br/>
    <p>段落</p>
</body>
</html>
```

最后我们可以对本章做一个小结

![](D:/Rolin的学习笔记/youdaonote-pull/youdaonote/youdaonote-images/WEBRESOURCE6fe13444b79511354ada9166fc6760ec.png)

