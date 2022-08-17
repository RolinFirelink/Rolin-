# CSS快速入门

今天我们来学习CSS

## CSS介绍

CSS是Cascading Style Sheets的简写，其中文全称为层叠样式表。是一种用于设置和布局网页的一种计算机语言，可以告知浏览器如何渲染解析界面元素。作为后端的程序员，我们不需要对CSS有太多的深入了解，稍微知道那点就差不多得了，所以我们这里的学习也是从简

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc010074de9297c8cc641e8b1d73d4d1c.png)

## CSS组成

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd7a12ab44400a38af3711076b6f18df8.png)

CSS主要由两个组成，分别是选择器和样式声明，选择器和样式声明，选择器其实就是选择HTML元素的方式，其可以使用标签名、class属性值、id值等多种方式，而样式声明则是声明不同的属性的样式，我们在上图中也可以看到一个CSS的选择器和样式声明的全过程，其实我们在HTML的时候已经用过这玩意了，这里就不再赘述了

## CSS入门案例

现在我们的目标是要在网页中显示一个较大的红色字体显示的今天开始变漂亮，这就是我们的入门案例，我们先来看看对应的实现步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEba84a717e329975312f45ad7f096d381.png)

那么根据上述步骤，我们可以写入演示代码如下

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe732d226af030f9dbddb8202ba608cc9.png)

## 浏览器开发者工具

简单介绍完了CSS之后我们接着介绍浏览器开发者工具，我们这里主要是针对谷歌浏览器而言的教程。

首先我们进入任意一个网页，右键然后点击检查选项，接着我们可以在网页右侧找到三个点的选项，选择中间的可以将右侧显示方式转移到下面，这个浏览器开发者工具一共分左右两部分，我们先来看看左边的部分

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbf31400c412879d833642c9e077cc70b.png)

左侧部分的选中元素选择之后可以在界面中直接选择想看的界面代码，其会自动定位。文档元素则是会显示界面中的界面代码，Console是控制台，Sources是源代码，后面的就如图所示了

接着来看右侧的部分

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd7460a462f7229f6f4e5d35994a561d8.png)

我们这里只需要记住Styles，在其下可以进行临时的样式修改，但只是临时的，相当于是预览，其不会改变我们的源代码

最后我们可以做一个小结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb15d1f82e112c13ad976779a8efe5817.png)

# CSS基本语法

## CSS引入方式

CSS的引入方式总共有三种，分别是内联样式，内部样式和外部样式

### 内联样式

先来讲讲内联样式，内联样式是在标签中直接通过style属性来控制样式，比较简单，但是缺点是只能影响当前这一行，对其他行无效

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1a84a87ab2eb70f3b796aaab61705e11.png)

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

### 内部样式

接着我们来讲第二种方式，内部样式，其实内部样式我们已经用多了，就是在<head>标签下加入style标签进行设置而已，这里就不再赘述了。其比起第一种方式来说更加麻烦，但是灵活性更好

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa8df9d62affb9fa5f8534d5a53b44cd3.png)

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

### 外部样式

那么现在我们来讲第三种方式，外部样式，外部样式是在标签中通过<link>标签来引入独立的css文件，可以影响不同的文件，其具有最高级别的灵活性，也是我们最为推荐的css引入方式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE96cde722b61a23a50f3e4c021ad0f90e.png)

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

## CSS的注释

这个没什么特别只得讲的，直接说重点。CSS的注释方式是/* */，如果我们的注释是上面的形式，则说明目前我们是在对css进行修改，反之则说明我们是在对HTML进行修改

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE448a40028896e3ef442d1a0e5e5758c9.png)

## CSS的选择器

什么是选择器呢？简而言之就是用于选择HTML的指定元素（标签）的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa48696ec6d0ab600de0afa497db9f40d.png)

CSS的选择器也不止有一种，这里我们先来介绍基本选择器，基本选择器有三种，分别是以标签名来选择的元素选择器，以class属性值来选择的类选择器，以id属性值来选择的id选择器，其优先级是逐渐增加的，最后的优先级最高（简单记法就是越具有通用性的选择器其优先级越低）

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEfe09232542467f1705134bab74f4a07a.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbdeaa69df04278c1d3cee4df9bebc32f.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1eebc0f6bc7cc1da787044785dc3a9ff.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9850929b5afbccad273ad36fbb2ad576.png)

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

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6fe13444b79511354ada9166fc6760ec.png)

# CSS头条页面案例

现在我们来用CSS实现头条页面案例，此前我们在HTML里也实现了头条页面，但是那时我们是使用图片来实现的，这一次我们要通过CSS来具体实现

同样我们首先要进行页面的布局，然后我们要填充文本、图片和超链接

在实现页面的布局之前，我们首先要学习边框的样式

## 边框样式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE5cdeca34bdfee95facd8a7cd987c90ca.png)

最后我们可以写入演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>边框样式演示</title>
    <style>
        #d1{
            /*设置所有边框*/
            /*border: 5px solid black;*/

            /*设置上边框*/
            border-top:5px solid black;
            /*设置左边框*/
            border-left:5px double red;
            /*设置右边框*/
            border-right:5px dotted blue;
            /*设置下边框*/
            border-bottom:5px dashed pink;

            width: 150px;
            height: 150px;
        }

        #d2{
            border:5px solid red;
            /*设置边框的弧度*/
            border-radius: 25px;

            width: 150px;
            height: 150px;
        }
    </style>
</head>
<body>
    <div id="d1"></div>
    <br/>
    <div id="d2"></div>
</body>
</html>
```

学完了边框样式之后，我们来学习文本样式

## 文本样式

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE619845b628590fbdd577b45ff4a25e19.png)

color的属性值可以通过颜色单词来定义，也可以通过RGB值来定义，后者我们只作为了解。接着最后一个vertical-align是调整文字的对齐方式的，我们可以直接写入middle令文字居中，有时没有效果，这时我们可以用百分比来代替，写入50%就可以了

那么综上我们可以写入演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>文本样式演示</title>
    <style>
        div{
            /*文本颜色*/
            color: red/*#ff0000*/;

            /*字体*/
            font-family: /*宋体*/微软雅黑;

            /*字体大小*/
            font-size: 25px;

            /*下划线 none:无 underline:下划线 overline:上划线 line-through:删除线*/
            text-decoration: none;

            /*水平对其方式 left:居左 center:居中 right:居右*/
            text-align:center;

            /*行间距*/
            line-height: 60px;
        }

        span{
            /*文字垂直对齐 top:居上 bottom:居下 middle:居中 百分比*/
            vertical-align: 50%;
        }
    </style>
</head>
<body>

    <div>
        我是文字
    </div>
    <div>
        我是文字
    </div>

    <img src="img/snow.gif" width="38px" height="38px"/>
    <span>雪花</span>
</body>
</html>
```

学习完了上面的内容之后，我们就可以正式去实现我们的案例了，由于我们的案例内容比较复杂，因此我们分部分去实现，这里我们先实现实现顶部超链接和导航条，先来看步骤

## 具体实现

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa4f05cafae63d3451adf5e8e933756c5.png)

我们这里就采用编写独立的CSS文件的方式来构造我们的样式，那么接着我们来看我们构造左侧分享的界面的步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbeb6523dd0835342dbf4e373df87ba64.png)

而中文正文和右侧广告图片我们之前已经实现过了，这里我们就不多讲一遍了，复制黏贴就完了

那么我们就只差实现底部超链接的步骤了，我们来一起看看

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE0d8941da8f16b4ed96f42538119ac1c6.png)

那么综上，我们可以写入演示的css文件代码如下

```
/*顶部样式*/
.top{
    background: black; /*背景色*/
    line-height: 40px; /*行高*/
    text-align: right; /*文字水平右对齐*/
    font-size: 20px; /*字体大小*/
}

/*顶部超链接样式*/
.top a{
    color: white; /*超链接颜色*/
}

/*导航条样式*/
.nav{
    line-height: 40px; /*行高*/
}

/*左侧分享样式*/
.left{
    width: 20%; /*宽度*/
    text-align: center; /*文字水平居中对齐*/
    float: left; /*浮动*/
}

/*左侧分享图片样式*/
.left img{
    width: 38px;
    height: 38px;
}

/*左侧文字*/
.left span{
    vertical-align: 50%; /*文字垂直居中对齐*/
}

/*中间正文样式*/
.center{
    width: 60%; /*宽度*/
    float: left; /*浮动*/
}

/*右侧广告图片样式*/
.right{
    width: 20%; /*宽度*/
    float: left; /*浮动*/
}

/*底部页脚超链接样式*/
.footer{
    clear: both; /*清除浮动*/
    background: blue; /*背景色*/
    text-align: center; /*文字水平居中对齐*/
    line-height: 25px;
}

/*底部页脚超链接文字颜色*/
.footer a{
    color: white;
}

/*超链接样式*/
a{
    text-decoration: none; /*去除下划线*/
    color: black; /*超链接颜色*/
}

/*超链接鼠标悬停时的样式*/
a:hover{
    color: red;
}
```

写入演示的HTML文件代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>头条页面</title>
    <link rel="stylesheet" href="css/news.css"/>
</head>
<body>
    <!--顶部登录注册更多-->
    <div class="top">
        <a href="#">登录&nbsp;&nbsp;</a>
        <a href="#">注册&nbsp;&nbsp;</a>
        <a href="#">更多&nbsp;&nbsp;</a>
    </div>

    <!--导航条-->
    <div class="nav">
        <img src="img/logo.png">
        <a href="#">首页&nbsp;&nbsp;</a>/
        <a href="#">科技&nbsp;&nbsp;</a>/
        <font color="gray">正文</font>
        <hr/>
    </div>

    <!--左侧分享-->
    <div class="left">
        <a href="#"> <img src="img/cc.png"/> <span>&nbsp;评论</span> </a>
        <hr/>
        <a href="#"> <img src="img/repost.png"/> <span>&nbsp;评论</span> </a> <br/>
        <a href="#"> <img src="img/snow.gif"/> <span>&nbsp;微博</span> </a> <br/>
        <a href="#"> <img src="img/snow.gif"/> <span>&nbsp;空间</span> </a> <br/>
        <a href="#"> <img src="img/snow.gif"/> <span>&nbsp;微信</span> </a> <br/>
    </div>

    <!--中间正文-->
    <div class="center">
        <!--标题-->
        <div>
            <h1>支付宝特权福利！芝麻分600以上用户惊喜，网友：幸福来得太突然？</h1>
        </div>

        <!--作者-->
        <div>
            <i><font size="2" color="gray">作者：itheima 2088-08-08</font></i>
            <hr/>
        </div>

        <!--副标-->
        <div>
            <h3>支付宝特权福利！芝麻分600以上用户惊喜，网友：幸福来得突然？</h3>
        </div>

        <!--正文-->
        <div>
            <p>这些年，马云的风头正盛，但是上个月他毅然辞去了阿里巴巴的职务。而马云所做的很多事情也的确改变了这个世界，特别是在移动支付领域，更是走在了世界的前列。如今中国的移动支付已经成为老百姓的必备，支付宝对中国社会的变革都带来了深远的影响。不过马云依然没有满足，他认为移动互联网将会成为人类的基础设施，而且这里面的机会和各种挑战还非常多。</p>
            <p>支付宝的诞生就是为了解决淘宝网的客户们的买卖问题，而随着支付宝的用户的不断增加，支付宝也推出了一系列的附加功能。比如生活缴费、转账汇款、还信用卡、车主服务、公益理财等，往简单的说，支付宝既可以满足人们的日常生活，又可以利用芝麻信用进行资金周转服务。除了芝麻分能够进行周转以外，互联网信用体系下的产品多多，我们对比以下几个产品看看区别:
            <ol>
                <li>蚂蚁借呗，芝麻分600并且受到邀请开通福利，这个就是支付宝贷款，直接秒杀了银行贷款和线下金融公司，是现在支付宝用户使用最多的。</li>
                <li>微粒贷：于2015年上线，主要面向QQ和微信征信极好的用户而推出，受到邀请才能申请开通，额度最高有30万，难度较大</li>
                <li>蚂蚁巴士：这个在微信 蚂蚁巴士 公众平台申请,对于信用分要求530分以上才可以,额度1-30万不等，目前非常火爆</li>
            </ol>
            <img src="img/1.jpg" width="100%">
            </p>
            <p>说起支付宝中的芝麻信用功能，相信更是受到了许多人的推崇，因为随着自己使用的不断增多，信用分会慢慢提高，而达到了一个阶段，就可以获得许多的福利。而当我们的芝麻信用分可以达到600分以上的时候，会有令我们想象不到的惊喜，接下来就让我们一起来看看，具体都有哪些惊喜吧。</p>
            <p><b>一、芝麻分600以上福利之信用购。</b>网购相信大家都不陌生，但是很多时候，网购都有一个通病，就是没办法试用，导致很多人买了很多自己不喜欢的东西。但是只要你的支付宝芝麻分在650及以上，就能立马享有0元下单，收到货使用满意了再进行付款。还能享用美食的专属优惠，是不是很耐斯</p>
            <p><b>二、芝麻分600以上福利之信用免押。</b>芝麻信用与木鸟短租联合推出信用住宿服务，芝麻分600及以上的用户可享受免押入住特权。木鸟短租拥有全国50万套房源，是国内领先的短租民宿预订平台。包括大家知道的飞猪信用住，大部分酒店可以免押金入住，离店再交钱。</p>
            <img src="img/2.jpg" width="100%">
            <p><b>三、芝麻分600以上福利之国际驾照。</b>我们经常听说的可能只是中国驾照，但现在芝麻分已经应用到了国际领域，只要你的芝麻分够550就可以免费办理国际驾照，也有不少人非常佩服马云，一个简单的芝麻分居然有如此大的功能，也从侧面反应出来马云在国际上的地位，这个国际驾照是由新西兰、德国、澳大利亚联合认证，可以在全球200多个国家通行，相信大家一定都有一个自驾全球的梦想吧，而现在支付宝就给了你一把钥匙，剩下的就你自己搞定了！有没有想带着你的女神来一次浪漫之旅呢？</p>
            <p>随着互联网对我们生活的改变越来越大，信用这一词也被大家推上风口浪尖，不论是生活出行，还是其他的互联网服务，与信用体系已经密不可分了，马云当初说道，找老婆需要拼芝麻分，如今似乎也要成为现实，那么你们的芝麻分有多少了呢？</p>
        </div>
    </div>

    <!--右侧广告图片-->
    <div class="right">
        <img src="img/ad1.jpg" width="100%">
        <img src="img/ad2.jpg" width="100%">
        <img src="img/ad3.jpg" width="100%">
        <img src="img/ad1.jpg" width="100%">
        <img src="img/ad2.jpg" width="100%">
        <img src="img/ad3.jpg" width="100%">
    </div>

    <!--底部页脚超链接-->
    <div class="footer">
        <a href="#">关于黑马</a>&nbsp;
        <a href="#">帮助中心</a>&nbsp;
        <a href="#">开放平台</a>&nbsp;
        <a href="#">诚聘英才</a>&nbsp;
        <a href="#">联系我们</a>&nbsp;
        <a href="#">法律声明</a>&nbsp;
        <a href="#">隐私政策</a>&nbsp;
        <a href="#">知识产权</a>&nbsp;
        <a href="#">廉政举报</a>&nbsp;
    </div>
</body>
</html>
```

# CSS的登录页面案例

先来看看我们的登录页面的效果

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE66cf4235a2cacea4e511996115d99896.png)

不得不说这个比上一次的几把登录页面舒服太多了，同样的我们先来进行必备知识的学习

我们先来看看我们的表格标签的知识介绍

## 表格标签

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE11b7e814a987ce15e68f5e5269d88029.png)

首先我们的table标签作用是设置一个表格，这个标签具有设置宽度高度边框样式和对齐方式

其次是tr标签，突然标签是行标签，这个行标签可以设置对齐方式

接着是td标签，td标签是单元格标签，也就是列标签，其下有两个属性，可以用于合并行或者是合并列

然后是th标签，其实表头标签，本身就具有加粗和居中的效果

最后是thead、tbody、tfoot这三个标签，分别是表头语义标签、标体语义标签、表脚语义标签，这三个标签的简单来说就是我们在表格标签里将这三个标签再分别放置于表格的头部，身部，脚部的位置，不过只作为了解就可以了，因为即使我们不加这个，我们的代码也不会有什么问题

假设我们要完成如下效果的表格

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE1def873a3b9bcfa7aa8a0e533f0dd2bd.png)

那么我们可以构造演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>表格标签演示</title>
</head>
<body>
    <!--
        表格标签：<table>
        属性：
            width-宽度
            height-高度
            border-边框
            align-对齐方式

            行标签：<tr>
            属性：
                align-对齐方式

            单元格标签：<td>
            属性
               rowspan-合并行
               colspan-合并列

            表头标签：<th>自带居中和加粗效果

            表头语义标签：<thead>
            表体语义标签：<tbody>
            表脚语义标签：<tfoot>
    -->
    <table width="400px" border="1px" align="center">
        <thead>
            <tr>
                <th>姓名</th>
                <th>性别</th>
                <th>年龄</th>
                <th>数学</th>
                <th>语文</th>
            </tr>
        </thead>

        <tbody>
            <tr align="center">
                <td>张三</td>
                <td rowspan="2">男</td>
                <td>23</td>
                <td colspan="2">90</td>
                <!--因为合并列，因此省略该语句 <td>90</td>-->
            </tr>

            <tr align=" center">
                <td>李四</td>
                <!--因为合并行，因此省略该语句 <td>男</td>-->
                <td>24</td>
                <td>95</td>
                <td>90</td>
            </tr>
        </tbody>

        <tfoot>
            <tr>
                <td colspan="4">总分数:</td>
                <td>373</td>
            </tr>
        </tfoot>
    </table>

</body>
</html>
```

接着我们来学习样式控制，先来看看知识列表

## 样式控制

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEbf62497a74daf48ff59cab3126ee6882.png)

首先是background-repeat样式名称是用于确定背景图片是否重复以及是上下重复还是左右重复的，默认是全重复的，这个其实不难理解，不多讲了

接着是outline样式，该样式用于控制输入框的轮廓，常与输入框的轮廓结合在一起使用，其下有四个属性，和我们之前讲过的边框样式非常相似

最后是display样式，该样式可以控制元素的属性，将其转换为内联元素或者是块级元素

根据上文我们可以写入代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>样式演示</title>
    <style>
        /*背景图片重复 no-repeat：不重复 repeat-x：水平重复 repeat-y：垂直重复 repeat：水平+垂直重复*/
        body{
            background: url("img/snow.gif");

            background-repeat: repeat;
        }

        /*轮廓控制 double：双实线 dotted：圆点 dashed：虚线 none：无*/
        input{
            outline: none;
        }
        /*元素显示 inline：内联元素(无换行、无长宽)  block：块级元素(有换行)  inline-block：内联元素(有长宽) none：隐藏元素*/
        div{
            color: white;
            display: inline-block;
            width: 400px;
        }

    </style>
</head>
<body>
    用户名：<input type="text">

    <div>春季</div>
    <div>夏季</div>
    <div>秋季</div>
    <div>冬季</div>
</body>
</html>
```

然后我们来讲CSS中的布局的一个重中之重的点，盒子模型。我们在java里曾经学习过，万物皆可看作对象，那么在CSS里，就是万物皆可看作盒子。先来看下面的例子

## 盒子模型

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc952e7681d95711d38e10fe567d024e3.png)

首先盒子模型是通过设置边框和元素内容的边距来实现布局的，分为内边距和外边距两种方式，如果我们的视角是放在外面的大的红盒子，那么我们设置的就是内边距，如果我们的视角是放在里面的小的蓝盒子，那么我们设置的就是外边距。一般我们都采用设置外边距的方式来实现布局，因为内边距的方式不但不符合人类直觉，而且会导致外框发生变化，不方便

下面是外边距的方法图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf9d79e1d15ae7fc767618544eaf215f4.png)

可以看到外边距里我们可以手动设置各个不同方向的边距，也可以只设置一个，其会默认该距离用于四个方向，还可以直接赋予四个距离，但这个方法要注意的是其方向的赋值方式是上右下左，最后是auto方法，该方法会自动计算外边距并居中

然后是内边距的方法图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE57806d5c39b5195c55aa460528fd83cc.png)

这个其实和外边距的差不多，这里就不赘述了

最后我们可以写入演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>盒子模型演示</title>
    <style>
        .wai{
            border: 1px solid red;
            width: 200px;
            height: 200px;
            /*设置内边距*/
            padding: 50px;
        }

        .nei{
            border: 1px solid blue;
            width: 100px;
            height: 100px;

            /*设置外边距的第一种方式
            margin-top: 50px;
            margin-left: 50px;
            margin-right: 50px;
            margin-bottom: 50px;*/

            /*设置外边距的第二种方式
            margin:50px;*/

            /*上、右、下、左*/
            /*margin: 70px 35px 30px 65px;*/
        }
    </style>
</head>
<body>
    <div class="wai">
        <div class="nei">

        </div>
    </div>
</body>
</html>
```

那么学习完了上面的知识之后，我们现在就可以来实现我们的登录页面案例了，同样的，我们也是编写独立的css文件和HTML文件来完成页面的构造，我们先构造顶部和中间表单，先来看看步骤

## 具体实现

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE950a3e21ed28763124d29c2f64822473.png)

有的同学可能有疑问，这不对吧？我们的案例里没有表格啊，其实是有的，就是我们的登录界面，这个表格分有四行两列，只是边框隐藏了而已，而且还有一些合并行列，比如第一行的账密登录就是合并了行

接着我们来看看中间表单样式的实现步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEa1179ac53f64bf4a01c724605f3356c7.png)

最后我们来看看底部页脚的实现步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE31da98f32652100137055319dbb90979.png)

最后我们可以写出演示的css文件如下

```
/*背景图片*/
body{
    background: url("../img/bg.png");
}

/*中间表单样式*/
.center{
    background: white;  /*背景色*/
    width: 40%;         /*宽度*/
    margin: auto;       /*水平居中外边距*/
    margin-top: 100px;  /*上外边距*/
    border-radius: 15px;/*边框弧度*/
    text-align: center;
}

/*表头样式*/
thead th{
    font-size: 30px; /*字体大小*/
    color: orangered; /*字体颜色*/

}

/*表体提示信息样式*/
tbody label{
    font-size: 20px; /*字体大小*/
}

/*表体输入框*/
tbody input{
    border: 1px solid gray; /*边框*/
    border-radius: 5px; /*边框弧度*/
    width: 90%; /*输入框的宽度*/
    height: 40px;/*输入框的高度*/
    outline: none;/*取消轮廓的样式*/
}

/*表底确定按钮的标签*/
tfoot button{
    border: 1px solid crimson; /*边框*/
    border-radius: 5px; /*边框弧度*/
    width: 95%; /*宽度*/
    height: 40px; /*高度*/
    background: crimson; /*背景色*/
    color: white; /*文字颜色*/
    font-size: 20px; /*字体大小*/
}

/*表行高度*/
tr{
    line-height: 60px;
}

/*底部页脚样式*/
.footer{
    width: 35%; /*宽度*/
    margin: auto; /*水平居中*/
    font-size: 15px; /*字体大小*/
    color: gray      /*字体颜色*/
}

/*超链接样式*/
a{
    text-decoration: none;
    color: blue;
}
```

演示的HTML文件的代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登录页面</title>
    <link rel="stylesheet" href="css/01.css">
</head>
<body>
    <!--顶部公司图标-->
    <div>
        <img src="img/logo.png"/>
    </div>

    <!--中间表单-->
    <div class="center">
        <form action="#" method="get" autocomplete="off">
            <table width="100%">
                <thead>
                    <tr>
                        <th colspan="2">账&nbsp;密&nbsp;登&nbsp;录<hr/></th>
                    </tr>
                </thead>

                <tbody>
                    <tr>
                        <td>
                            <label for="username">账号</label>
                        </td>
                        <td>
                            <input type="text" id="username" name="username" placeholder="请输入账号" required/>
                        </td>
                    </tr>
                    <tr>
                        <td>
                            <label for="password">密码</label>
                        </td>
                        <td>
                            <input type="text" id="password" name="password" placeholder="请输入密码" required/>
                        </td>
                    </tr>
                </tbody>

                <tfoot>
                    <tr>
                        <td colspan="2">
                            <button type="submit">确&nbsp;定</button>
                        </td>
                    </tr>
                </tfoot>
            </table>
        </form>
    </div>

    <!--底部页脚-->
    <div class="footer">
        <br/><br/>
        登录/注册即表示您统一&nbsp;&nbsp;
        <a href="#" target="_blank">用户协议</a>&nbsp;&nbsp;
        和&nbsp;&nbsp;
        <a href="#" target="_blank">隐私条款</a>&nbsp;&nbsp;&nbsp;&nbsp;
        <a href="#" target="_blank">忘记密码</a>
    </div>
</body>
</html>
```

