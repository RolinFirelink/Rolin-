# HTML快速入门

## 网页的构成

一个网页主要由三部分技术构造而成，分别是HTML，CSS和JavaScript。其中HTML的作用是制作网页基础内容和基本结构，CSS是用于美化网页，而JavaScipt用于制作网页的动态效果。本节我们主要学习HTML

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2786dab7d3c1ed14a2b8657d8d07e023.png)

## HTML的介绍

HTML其实是HyperText Markup Language（超文本标记语言）的缩写，超文本的意思就是比普通文本更加强大，而标记其实就是标签的意思，可以使用一系列的标签使得网络上的文档格式统一，使得分散的资源连接为一个逻辑整体

2014.10月，万维网联盟HTML5的规则制作完成，其不但提供了一套完整的规范，还提供了一套免费的教学文档，后续学习中我们会使用到这个文档

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd9b8cb8f13764b08c46ab0d597fac99e.png)

## HTML的组成

HTML页面是由一系列的元素组成的，而元素是通过标签创建出来的，因此我们先来讲解标签

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEb9816b4641da1e28d6588b4a98cc8305.png)

那么什么是标签呢？其又有什么作用呢？其实标签简单来说就是两个<自定义内容>包括而成的内容，这个内容和我们以前学习图书馆管理系统的时候接触到的FXML界面相关内容其实挺相似的，他们都是有前必有后而且后者要加</自定义内容>来作为结尾。那么标签有什么作用呢？标签可以用于设置文本样式、图片样式、超链接样式等。一个元素包括一个开始标签和一个结束标签，中间有存放我们定义的内容

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEe935ffec52fa75a3536a5a66a622e3ea.png)

标签的另外一个重要的内容就是属性，属性可以为标签提供更多的信息，但是要注意的是属性只能添加到开始标签中，而且其格式为属性名=属性值，具体使用方式可以看下图的例子

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEac1a1d7554e471971e7c4cc24054733b.png)

## HTML入门案例

学习了上面的内容之后，我们话不多说，速速来试试新技术，趁热打铁。请看我们要实现的案例效果

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE47dafcfe5c0259519e66a2589f547958.png)

可以看到我们在左上角中要显示01-入门案例，在中间居中的地方要显示这是我的第一个HTML入门案例，要实现这个案例，我们要按照以下步骤进行案例构造，这里使用的工具是idea

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9d0c8f3e90cdade6044af33fab18b0ad.png)

注意这里创建的内容是HTML5，不是其他的啊，创建完成之后我们可以看到如下内容

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
</html>
```

这里我们一个个对其代码进行说明，首先第一行的代码是H5的文档声明（有什么作用还不知道），以前的版本文档声明往往很长，H5中对其进行了优化，使其文档声明只有一句，方便不少

第二行是根标签，注意一个文件中只能有一个根标签

第三行<head>是头部标签，其实就是我们的页面的右上角显示的东西，第五行Title内写入什么，就会在右上角的窗口中显示什么，我们这里应该要写入01-入门案例

第七行<body>，是代表身体，我们这里其实可以理解为将一个页面分为了头和身体两部分，头部主要是上面的窗口内容，而身体则是页面中的大窗口的所有的内容，我们的具体构造页面就可以在这个body中进行构造

第九行和第十行时身体和根标签的结束标签

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE898a32601f54628a153b1357f1faf438.png)

那么最后我们可以将代码写成如下形式

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01-入门案例</title>
</head>
<body>
<h1 align="center">这是我的第一个HTML入门案例</h1>
</body>
</html>
```

最后我们只要点击右上角用对应的浏览器打开就可以看到我们所构造的页面了

## HTML概念小结

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE28263e3266c830926a03c1f2be589585.png)

# HTML基本语法

## HTML的注释

我们先来学习HTML的注释相关内容，这里只有一点需要讲，就是快速注释内容的快捷键是ctrl+shift+/，无了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEaeee8fb03997950ea0ccfe9e1f1a3921.png)

## HTML的标签

接着我们来学习HTML的标签，<h1></h1>是一级标题，随着数字的变换，还有二级标题和三级标题

然后是自闭和标签，自闭和标签有<br/>和<hr/>，前者的作用是换行，后者的作用是在界面上加一个下划线

类似于<b>和<u>一类的标签是行内元素，可以给我们的文字加上属性（比如加粗或者是加下划线），其嵌套格式是前对前后对后，i标签是添加斜体

块级元素里<p>的作用是作为一个段落标签

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd8ab0faa4c2200fe98467a3cb561d631.png)

最后我们可以构造一个学习代码

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01-入门案例</title>
</head>
<body>
<!--这是一个注释，它的内容不会被浏览器所解析-->
<h1 align="center">这是我的第一个HTML入门案例，是一级标题</h1>
<h2 align="center">这是我的第二个HTML入门案例，是二级标题</h2>
<br/><!--这是一个自闭和标签，其作用是换行-->
<hr/><!--这是一个自闭和标签，其作用是添加一个下划线-->
<span>这是span行内元素，两个标签添加的文字不会令其换行</span>
<span>这是span行内元素，两个标签添加的文字不会令其换行</span>
<div>这是div块内元素，自己独占一行，后面的元素会自动换行</div>
<div>这是div块内元素，自己独占一行，后面的元素会自动换行</div>
<br/><!--这是一个自闭和标签，其作用是换行-->
<hr/>
<div><u>u的作用是给文字加上下划线</u></div>
<div><b>u的作用是给文字加粗</b></div>
<div><i>u的作用是给文字添加斜体</i></div>
<p>p代表的是一个段落标签</p>
</body>
</html>
```

## HTML的属性

所谓属性就是可以提供不会直接显示在内容中的信息，可以作为改变标签样式或者是提供数据使用，其具体案例在我们学习图书馆管理系统的时候其实已经有所接触了。其定义格式为属性名=属性值，这里要注意的是在同一个标签中属性的名称必须要唯一，即是比方说我们不可以提供了一个中间属性之后再提供一个中间属性，这是不允许的

常用的属性有class、id、name、value、style，其中class的作用是定义元素的类名，用于选择和访问特定元素。而id是定义元素的唯一标识，前者可以重复使用，后者不可以，style可以定义元素的css样式，比如添加背景颜色什么的，更多的其他内容我们用到再解释

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE654b422d7e733bfcc7657980bbf89eda.png)

最后我们可以定义的代码如下：

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>属性</title>
</head>
<body>
    <div class="cls">第一个div</div>
    <div class="cls">第二个div</div><!--可以看到class重复使用cls是允许的-->
    <div id="d1">第三个div</div>
    <div id="d2">第四个div</div><!--如果我们还是定义d1，那么编译会不通过-->
    <div style="background-color: red">第五个div</div>
</body>
</html>
```

## HTML的特殊字符

像< > " ' 空格 &一类的符号都是特殊字符，我们在HTML中无法直接用其本身来表示他们，或者表示完全，因此我们要学习使用等价字符来进行引用

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE564566763867255dc6a86eea298f29f8.png)

最后我们可以写入一个简单的测试代码

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>特殊字符</title>
</head>
<body>
    本网站有     最终解释权 <br/><!--可以看到虽然打入了五个空格，但实际只显示一个-->
    本网站有&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;最终解释权
</body>
</html>
```

# HTML案例

## 新闻文本案例

先来看看我们的新闻布局的大概样子

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE13394d0b989f7e2451f9c49949d678f0.png)

在实现新闻文本之前，我们要先完成对新闻布局的构造，因为我们这里显然能看到我们的新闻文本的整体布局和我们之前学习的简单布局不尽相同。要实现新闻布局的构造，我们可以按照以下步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd44770bee21cac4c08652ae4a05a9e89.png)

注意这里的style标签是写到head标签中的，然后我们这里border指的是显示边框和边框的宽度，而width指的是边框宽度占整个窗口的占比，height指的是边框的高度，margin指的是边框举例浏览器的距离，我们选择auto就会自动将边框自动对其中间

最后我们就可以得到如下的格式代码，注意这里我们在head标签内嵌入style标签，标签内写入了div，这里的div是对应body标签中的div的，如果body标签中有多个div，则会全部照应（一个猜测，不保证对）

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>样式演示</title>
    <style>
        div{
            /*显示边框*/
            border: 1px solid red;

            /*宽度 占屏幕的60％*/
            width: 60%;

            /*高度 500像素*/
            height: 500px;

            /*边框外边距*/
            margin: auto;
        }
    </style>
</head>
<body>
    <div>第一个div</div>
</body>
</html>
```

接着我们就来正式地去实现新闻文本内容，我们可以看到我们的新闻文本有标题，作者，副标题和正文四部分，那么我们实现时也用四个div标签将其分为四部分进行实现，接着我们可以按照以下部分去实现

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEc2114638f24f9c98bdd3282d9470e8a1.png)

## 文本标签的学习

我们接着来学习文本标签，由于文本标签比较多，所以这里就只讲常用的

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE80ba48b306cb24ed4d611231ed2b4072.png)

下面是文本标签的使用案例的代码

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>文本标签演示</title>
</head>
<body>
    <!--
        段落标签：<p>
    -->
    <p>这些年，马云的风头正盛，但是上个月他毅然辞去了阿里巴巴的职务。而马云所做的很多事情也的确改变了这个世界，特别是在移动支付领域，更是走在了世界的前列。如今中国的移动支付已经成为老百姓的必备，支付宝对中国社会的变革都带来了深远的影响。不过马云依然没有满足，他认为移动互联网将会成为人类的基础设施，而且这里面的机会和各种挑战还非常多。</p>
    <p>支付宝的诞生就是为了解决淘宝网的客户们的买卖问题，而随着支付宝的用户的不断增加，支付宝也推出了一系列的附加功能。比如生活缴费、转账汇款、还信用卡、车主服务、公益理财等，往简单的说，支付宝既可以满足人们的日常生活，又可以利用芝麻信用进行资金周转服务。除了芝麻分能够进行周转以外，互联网信用体系下的产品多多，我们对比以下几个产品看看区别:</p>

    <!--
        标题标签：<h1> ~ <h6>
    -->
    <h1>一级标题</h1>
    <h2>二级标题</h2>
    <h3>三级标题</h3>
    <h4>四级标题</h4>
    <h5>五级标题</h5>
    <h6>六级标题</h6>

    <!--
    水平线标签：<hr/>
    属性：
        size-大小
        color-颜色
    -->
    <hr size="4" color="red">

    <!--
    无序列表：<u1>
    属性：type-列表样式(disc实心圆、circle空心圆、square空心方块)
    -->
    <ul type="circle">
        <li>javaEE</li>
        <li>HTML</li>
    </ul>

    <!--
    有序列表：<o1>
    属性：type-列表样式(1数字、A或a字母、I或i罗马字符)    start-起始位置
    列表项：<li>
    -->
    <ol type="1" start="10">
        <li>传智播客</li>
        <li>黑马程序员</li>
    </ol>

    <!--
        斜体标签：<i>    <em>
    -->
    <i>我倾斜了</i>
    <em>我倾斜了</em>
    <br/>

    <!--
        加粗标签：<strong> <b>
    -->
    <strong>加粗文本</strong>
    <b>加粗文本</b>

    <!--
        文字标签：<font>
        属性：
            size-大小
            color-颜色
    -->
    <font size="5" color="yellow">这是一段文字</font>

</body>
</html>
```

最后我们可以实现该案例的代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>新闻文本</title>
    <style>
        div{
            /*宽度 占屏幕的60％*/
            width: 60%;

            /*边框外边距*/
            margin: auto;
        }
    </style>
</head>
<body>
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
        </p>
        <p>说起支付宝中的芝麻信用功能，相信更是受到了许多人的推崇，因为随着自己使用的不断增多，信用分会慢慢提高，而达到了一个阶段，就可以获得许多的福利。而当我们的芝麻信用分可以达到600分以上的时候，会有令我们想象不到的惊喜，接下来就让我们一起来看看，具体都有哪些惊喜吧。</p>
        <p><b>一、芝麻分600以上福利之信用购。</b>网购相信大家都不陌生，但是很多时候，网购都有一个通病，就是没办法试用，导致很多人买了很多自己不喜欢的东西。但是只要你的支付宝芝麻分在650及以上，就能立马享有0元下单，收到货使用满意了再进行付款。还能享用美食的专属优惠，是不是很耐斯</p>
        <p><b>二、芝麻分600以上福利之信用免押。</b>芝麻信用与木鸟短租联合推出信用住宿服务，芝麻分600及以上的用户可享受免押入住特权。木鸟短租拥有全国50万套房源，是国内领先的短租民宿预订平台。包括大家知道的飞猪信用住，大部分酒店可以免押金入住，离店再交钱。</p>
        <p><b>三、芝麻分600以上福利之国际驾照。</b>我们经常听说的可能只是中国驾照，但现在芝麻分已经应用到了国际领域，只要你的芝麻分够550就可以免费办理国际驾照，也有不少人非常佩服马云，一个简单的芝麻分居然有如此大的功能，也从侧面反应出来马云在国际上的地位，这个国际驾照是由新西兰、德国、澳大利亚联合认证，可以在全球200多个国家通行，相信大家一定都有一个自驾全球的梦想吧，而现在支付宝就给了你一把钥匙，剩下的就你自己搞定了！有没有想带着你的女神来一次浪漫之旅呢？</p>
        <p>随着互联网对我们生活的改变越来越大，信用这一词也被大家推上风口浪尖，不论是生活出行，还是其他的互联网服务，与信用体系已经密不可分了，马云当初说道，找老婆需要拼芝麻分，如今似乎也要成为现实，那么你们的芝麻分有多少了呢？</p>
    </div>
</body>
</html>
```

## 头条页面案例

节省空间，这里就不放最终完成的效果图了，我们可以知道的是上面中左右有图片，下方有超链接，中间是正文，正文就是新闻案例的正文

首先我们同样是先来分析布局，这里我们的顶部登录注册，导航条以及底部超链接都是独立占据一行的，我们都可以用div来表示。左侧图片，右侧图片，和中间的正文也是这样的一个思路，但是有一个问题就是我们的div总是独立占据一行的，而这里我们的div的三个片段却都是平行的，这样的话不是和div的特点冲突了吗？为了完成我们的目的，这里我们要使用到div的一个属性，浮动

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd1b79e631ddff357476ddd989e875536.png)

所谓浮动，简而言之就是可以将三个不同行的div给并入到一行中，只要赋予其对应的命令，赋予其left则其会靠左显示，若靠左已经有元素显示了，则另外一个会在靠左的后面继续显示，全部设置成向右也是同理的

值得一提的是，这里要给我们的div加上不同的class属性，这样我们就可以单独对不同的div指定不同的属性，如果不加的话其会默认对所以的div都做一样的动作

我们可以再head标签中通过.class属性的方式来给对应的div指定对应属性值

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd9163dd1abcc70c9b97837bb433f500a.png)

然后我们就可以构造布局代码如下：

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>样例演示</title>
    <style>
        /*给div标签添加边框*/
        div{
            border: 1px solid red;
        }

        /*左侧图片的div样式*/
        .left{
            width: 20%;
            float: left;
            height: 500px;
        }

        /*中间正文的div样式*/
        .center{
            width: 59%;
            float: left;
            height: 500px;
            /*给予高度设置是为了让我们可以再左右中间设置对应的图片或者是文字*/
        }

        /*右侧图片的div样式*/
        .right{
            width: 20%;
            float: left;
            height: 500px;
        }

        /*底部超链接的div样式*/
        .footer{
            /*清除浮动效果*/
            clear: both;
            /*文本对齐方式*/
            text-align: center;
            /*背景颜色*/
            background-color: blue;
        }
    </style>
</head>
<body>
    <!--顶部登录注册-->
    <div>top</div>

    <!--导航条-->
    <div>navibar</div>

    <!--左侧图片-->
    <div class="left">left</div>

    <!--中间正文-->
    <div class="center">center</div>

    <!--右侧广告图片-->
    <div class="right">right</div>

    <!--底部页脚超链接-->
    <div class="footer">footer</div>
</body>
</html>
```

这里要底部要消除浮动的原因我搞不懂，总之不消除会出问题，暂时先记着如果底部代码不需要浮动就要消除上面的浮动吧

接下来我们来讲解关于图片标签的演示，插入图片的标签是img，img中最终要的是src属性，src属性是必要的，其表示图片的地址，而其他属性则可加可不加，具体作用和语法在图中有了，自己看吧

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf2ac0f8c544711e7a8e07545425cf7c5.png)

最后我们可以构造演示代码如下：

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>图片标签演示</title>
</head>
<body>
    <!--
        图片标签：<img>
        属性：
            src-图片的路径
            title-鼠标悬浮时显示的内容
            alt-图片找不到时显示的内容
            width-图片的宽度
            height-图片的高度
    -->
    <img src="img/ad1.jpg" title="广告" alt="找不到目标图片" width="300px" height="300px">
</body>
</html>
```

接着我们来学习超链接标签，超链接标签相对比较简单，a是超链接的标签，而href则是最为重要的超链接指向的网页地址的属性。我们可以通过其他的方法给图片加上超链接，或者是将超链接的样式，字体给进行一些改变

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE3a689f76b6f2c2565fd1ff26ffa0b2b1.png)

下面是我们构造的演示代码

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>超链接标签演示</title>
    <style>
        a{
            /*去掉超链接的下划线*/
            text-decoration: none;
            /*超链接的颜色*/
            color: black;
        }

        /*鼠标悬浮的样式控制*/
        a:hover{
            color: red;
        }
    </style>
</head>
<body>
    <!--
        超链接标签：<a>
        属性：
            href-跳转的地址
            target-跳转的方式(_self为从当前页面进入、_blank为从新标签页进入)
    -->
    <a href="http://www.itcast.cn" target="_blank">传智播客</a> <br/>
    <a href="http://www.itheima.cn" target="_self">黑马程序员</a> <br/>
    <!--将超链接以图片的形式展示,中间原本要填入的文字内容改为填入一个图片-->
    <a href="http://www.itheima.cn" target="_blank"><img src="img/itheima.png" width="150px" height="50px"/></a>
</body>
</html>
```

那么学习了上面的内容之后，我们现在就可以正式去实现这个头条页面了，先来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEad33e0785337285eb21461b8b2a436af.png)

最后我们可以构造代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>头条页面</title>
    <style>
/*        !*给div标签添加边框*!
        div{
            border: 1px solid red;
        }*/

        /*左侧图片的div样式*/
        .left{
            width: 20%;
            float: left;
        }

        /*中间正文的div样式*/
        .center{
            width: 60%;
            float: left;
        }

        /*右侧图片的div样式*/
        .right{
            width: 20%;
            float: left;
        }

        /*底部超链接的div样式*/
        .footer{
            /*清除浮动效果*/
            clear: both;
            /*文本对齐方式*/
            text-align: center;
            /*背景颜色*/
            background-color: blue;
        }

        /*超链接的样式控制*/
        a{
            color: white;
            text-decoration: none;
        }
    </style>
</head>
<body>
    <!--顶部登录注册-->
    <div>
        <img src="j1.png" width="100%">
    </div>

    <!--导航条-->
    <div>
        <img src="j2.png" width="100%">
        <hr/>
    </div>

    <!--左侧图片-->
    <div class="left">
        <img src="j3.png" width="100%">
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
            <img src="1.jpg" width="100%">
            </p>
            <p>说起支付宝中的芝麻信用功能，相信更是受到了许多人的推崇，因为随着自己使用的不断增多，信用分会慢慢提高，而达到了一个阶段，就可以获得许多的福利。而当我们的芝麻信用分可以达到600分以上的时候，会有令我们想象不到的惊喜，接下来就让我们一起来看看，具体都有哪些惊喜吧。</p>
            <p><b>一、芝麻分600以上福利之信用购。</b>网购相信大家都不陌生，但是很多时候，网购都有一个通病，就是没办法试用，导致很多人买了很多自己不喜欢的东西。但是只要你的支付宝芝麻分在650及以上，就能立马享有0元下单，收到货使用满意了再进行付款。还能享用美食的专属优惠，是不是很耐斯</p>
            <p><b>二、芝麻分600以上福利之信用免押。</b>芝麻信用与木鸟短租联合推出信用住宿服务，芝麻分600及以上的用户可享受免押入住特权。木鸟短租拥有全国50万套房源，是国内领先的短租民宿预订平台。包括大家知道的飞猪信用住，大部分酒店可以免押金入住，离店再交钱。</p>
            <img src="2.jpg" width="100%">
            <p><b>三、芝麻分600以上福利之国际驾照。</b>我们经常听说的可能只是中国驾照，但现在芝麻分已经应用到了国际领域，只要你的芝麻分够550就可以免费办理国际驾照，也有不少人非常佩服马云，一个简单的芝麻分居然有如此大的功能，也从侧面反应出来马云在国际上的地位，这个国际驾照是由新西兰、德国、澳大利亚联合认证，可以在全球200多个国家通行，相信大家一定都有一个自驾全球的梦想吧，而现在支付宝就给了你一把钥匙，剩下的就你自己搞定了！有没有想带着你的女神来一次浪漫之旅呢？</p>
            <p>随着互联网对我们生活的改变越来越大，信用这一词也被大家推上风口浪尖，不论是生活出行，还是其他的互联网服务，与信用体系已经密不可分了，马云当初说道，找老婆需要拼芝麻分，如今似乎也要成为现实，那么你们的芝麻分有多少了呢？</p>
        </div>
    </div>

    <!--右侧广告图片-->
    <div class="right">
        <img src="ad1.jpg" width="100%">
        <img src="ad2.jpg" width="100%">
        <img src="ad3.jpg" width="100%">
        <img src="ad1.jpg" width="100%">
        <img src="ad2.jpg" width="100%">
        <img src="ad3.jpg" width="100%">
    </div>

    <!--底部页脚超链接-->
    <div class="footer">
        <!--在href中输入#号可以让超链接不进行任何跳转-->
        <a href="#">关于黑马</a>
    </div>
</body>
</html>
```

## 注册页面案例

我们先来看看页面的效果，这里省略了背景图和一些细节

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEf6ba8a581fae57e5dc73453700fe1767.png)

可以看到这就是一个非常简单明了的一个注册页面，我们要学习的就是如何实现这个注册页面，要实现这个注册页面，首先我们要进行对这个页面的布局，然后使用表单标签完成表单项

我们先来实现堆页面的布局，在我们这里，所谓的页面布局其实就是背景图片，没了，非常简单

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE53251433b4bda60eb99a75dc9ecf3585.png)

我们实现这个布局的一个简单想法就是添加一个div，但是我们注意分析我们可以知道背景图片必然是处于body之内的，而且正好希望覆盖到整个body页面，所以我们可以直接在头部位置里对body进行操作，给其设置一个背景图片，其设置方式和div的设置方式是一样的，最后我们可以写入代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册页面</title>
    <style>
        body{
            /*添加背景图片*/
            background: url("img/bg.png");
        }
    </style>
</head>
<body>

</body>
</html>
```

这个代码构造的界面会正确显示背景图片

接着我们就来学习关于表单标签的内容，请看下图

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE6a6d0f9dc961c5773858a79ed2885dcb.png)

表单标签的标签名是form，期间作用是表示表单标签，注意其作用只是表示表单标签，不是说有了它就立刻可以进行输入提交等动作了，只是其是作为一个前提条件而存在的，有了他才有设置输入提交的可能，没有他就全完了，不是说有了他就啥都好了

其中action属性是用于设定提交数据的路径，因为我们的数据肯定是提交到服务器中保存的，这里我们要设置一个指定的服务器路径，当然我们现在的学习阶段是没有什么服务器的，因此我们写入#

method表示提交表单的方式，有两种，分别是get和post，其区别已经写在图上了，至于为什么其会有这样的区别，这个等到我们后面再做解释

最后是autocomplete属性，这个属性表示是否记录补全。简单来说就是是否开启历史补全，开启的话用户的鼠标一点击到输入框中就会在下方显示之前输出过的内容，on表示开启，off表示关闭

最后我们可以写入代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>表单标签</title>
</head>
<body>
    <!--get方式的表单-->
    <form action="#" method="get" autocomplete="off">
        用户名：<input type="text" name="username"/>
        <button type="submit">提交</button>
    </form>
    <!--post方式的表单-->
    <form action="#" method="post" autocomplete="off">
        用户名：<input type="text" name="username"/>
        <button type="submit">提交</button>
    </form>
</body>
</html>
```

这里有一些代码比如input，button这些表单项，这些的存在能够让我们的页面产生能够正确输入的提交的窗口，我们将在下一节学习这些

那么现在我们来学习表单项

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE490e812a0a89e96ad2f882d525aa0f73.png)

首先我们要知道表单项都是放在form标签下的，不能放在其他标签下。label标签是表单的元素说明，其配合表单单项标签使用，其作用是给表单的元素增添说明，其实看了演示我也没整明白这玩意到底有啥用，反正我们写选项框名字的时候都加上这个吧。值得注意的是其label标签下的for属性的值必须和表单项的id属性值保持一致，如果表单项没有id值，那么就必须添加id值，否则会报错

接着是input标签，这个标签用于接受用户数据，其下有很多属性，type属性用于指定数据的类型，数据的类型是非常多的，我们这里先挑一个最简单的text文本数据类型，后续我们还有专门的章节来讲解type属性的各种可以指定的数据类型。id属性是标签的表示，目前我们知道的id的作用就是可以让一个标签指定另外一个标签。接着是name属性，这个属性是作为提交服务器的标识，如果我们的数据要提交到服务器，那么我们就必须要加上name属性。value属性是默认的数据值，简单来说就是在打开网页的时候默认给你先添加一个预先设定的默认值。placeholder是默认提示信息，当我们的输入框没有任何数据的时候，这个提示信息就会在输入框中显示出来，required属性用于确定是否必须要有数据，就是用于指定输入框内的内容是否必填。

button标签代表按钮，其下有type属性，按钮的功能有submit，代表提交，reset代表重置，而button就是按钮，没有其他功能，以后我们学了其他的技术的话，可以给按钮指定某个功能，这是button属性的含金量就显示出来了

最后我们可以写入如下代码

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>表单项标签</title>
</head>
<body>
    <!--
        表单项标签：<label> 表单元素说明
        属性：for属性，属性值必须和表单项标签id属性值一致

        表单项标签：<input> 多种数据类型
        属性：
            type-数据类型
            id-唯一标识
            name-提交服务器的标识
            value-默认的数据值
            placeholder-默认的提示信息
            required-是否必须

         按钮标签：<button>
         属性：
            type-按钮的类型(submit提交、reset重置、button普通按钮)
    -->
    <form action="#" method="get" autocomplete="off">
        <label for="username">用户名：</label>
        <input type="text" id="username" name="username" value="" placeholder="请在此处输入用户名" required/>
        <button type="submit">提交</button>
        <button type="reset">重置</button>
        <button type="button">按钮</button>
    </form>
</body>
</html>
```

接着我们来学习type属性值

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCEd9326fcece05da823589b46047488e9b.png)

text属性值就是普通的文本框。而password则是密码框，其输入的数据是不会显示出来的。email是邮箱框，其中有简单验证，如果用户输入的数据不含有@符号，则不予通过。radio是单选框，如果我们想要让单选框发挥作用，那么各种不同的选项中必须要有相同的name属性值，而且我们必须在选项中设置value，其代表实际提交的值，checked则代表默认选中的值。checkbox代表多选框，其注意事项和单选框一样，就不再赘述了

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE2fdd98825f6ce11d97aebb156d42294d.png)

选择日期。time属性值则是时间，效果和上面一样。而datetime-local则是时间日期框，用户不但可以选择日期还可以选择时间。number是数字框，用户只能输入数字。range是滚动条数值框，用户可以通过拖动滚动条来选择对应的值。search则是可清除文本框，也是输入文本，与text不同的是，search属性带有一个X，点击这个X可以清除文本框。tel是电话框。url是网址框。file是文件上传框。hidden是隐藏域，可以在value中设置实际要提交的值。

最后我们可以写入演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>type属性演示</title>
</head>
<body>
    <!--
        type属性
        <input type="text"/>        普通文本输入框
        <input type="password"/>    密码输入框
        <input type="email"/>       邮箱输入框
        <input type="radio"/>       单选框。必须有相同的属性值，value代表真实提交的数据，checked属性代表默认选中的值
        <input type="checkbox"/>    单选框。必须有相同的属性值，value代表真实提交的数据，checked属性代表默认选中的值

        <input type="range"/>             滚动条数值框 min-最小值 max-最大值 step-步进值
        <input type="datetime-local"/>    本地日期时间框
        <input type="date"/>              日期框
        <input type="time"/>              时间框
        <input type="number"/>            数字框
        <input type="search"/>            可清除文本框
        <input type="tel"/>               电话框
        <input type="url"/>               网址框
        <input type=" file"/>             文件上传框
        <input type=" hidden"/>           隐藏域 value属性设置实际提交的值
    -->
    <form action="#" method="get" autocomplete="off">
        <label for="username">用户名：</label>
        <input type="text" id="username" name="username"/> <br/>

        <label for="password">密码：</label>
        <input type="password" id="password" name="password"/> <br/>

        <label for="email">邮箱：</label>
        <input type="email" id="email" name="email"/> <br/>

        <label for="gender">性别：</label>
        <input type="radio" id="gender" name="gender" value="man"/>男
        <input type="radio" name="gender" value="women"/>女
        <input type="radio" name="gender" value="other"/>其他 <br/>

        <label for="hobby">爱好：</label>
        <input type="checkbox" id="hobby" name="hobby" value="music"/>音乐
        <input type="checkbox" name="hobby" value="game"/>游戏

        <button type="submit">提交</button>
        <button type="reset">重置</button>

        <label for="birthday">生日：</label>
        <input type="date" id="birthday" name="birthday"/> <br/>

        <label for="time">当前时间：</label>
        <input type="time" id="time" name="time"/> <br/>

        <label for="insert">注册时间：</label>
        <input type="datetime-local" id="insert" name="insert"/> <br/>

        <label for="age">年龄：</label>
        <input type="number" id="age" name="age"/> <br/>

        <label for="range">心情值(1~10):</label>
        <input type="range" id="range" name="range" min="1" max="10" step="1"/> <br/>

        <label for="search">可全部清除文本：</label>
        <input type="search" id="search" name="search"/> <br/>

        <label for="tel">电话：</label>
        <input type="tel" id="tel" name="tel"/> <br/>

        <label for="url">个人网站：</label>
        <input type="url" id="url" name="url"/> <br/>

        <label for="file">文件上传：</label>
        <input type="file" id="file" name="file"/> <br/>

        <label for="hidden">隐藏信息：</label>
        <input type="hidden" id="hidden" name="hidden" value="itheima"/> <br/>

        <button type="submit">提交</button>
        <button type="reset">重置</button>
    </form>
</body>
</html>
```

## 其他常用标签

最后我们来学习下一些其他的常用标签

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE9765abc21d852bc3969ff5ada26b96ef.png)

select表示下拉标签，一般我们将其用于选择对应的城市和地区，而optgroup和option可以给下拉框设置分组和表示下拉列表标签，textarea表示文本域标签，可以手动设置行列，用于给用户输入文字

我们可以写入演示代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>其他常用标签演示</title>
</head>
<body>
    <!--
        下拉列表标签：<select>
        列表项标签：<option>
        列表项分组标签：<optgroup> 属性：label设置分组名称

        文本域标签：<textarea>
        属性：
            rows-行数
            cols-列数
    -->
    <form action="#" method="get" autocomplete="off">
        所在城市：<select name="city">
        <option>---请选择城市---</option>
        <optgroup label="直辖市">
            <option>北京</option>>
            <option>上海</option>>
        </optgroup>
        <optgroup label="省会市">
            <option>杭州</option>>
            <option>武汉</option>>
        </optgroup>
    </select>
        <br/>
        个人介绍：<textarea name="desc" rows="5" cols="20"></textarea>

        <button type="submit">提交</button>
        <button type="reset">重置</button>
    </form>
</body>
</html>
```

学习完了所有内容之后，最后我们可以实现案例界面代码了，先来看看步骤

![](https://rolin-typora.oss-cn-guangzhou.aliyuncs.com/WEBRESOURCE165bc5c1744b184c7f2930f514df5026.png)

最后我们可以按照步骤实现案例界面代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册页面</title>
    <style>
        body{
            background: url("img/bg.png");
        }

        .center{
            /*背景颜色*/
            background: white;
            /*宽度*/
            width: 400px;
            /*文本对齐方式*/
            text-align: center;
            /*外边距*/
            margin: auto;
        }
    </style>
</head>
<body>
    <!--顶部-公司图标-->
    <div>
        <img src="img/logo.png">
    </div>
    <!--中间-注册图标-->
    <div class="center">
        <div>注册详情</div>
        <hr/>

        <!--表单标签-->
        <form action="#" method="get" autocomplete="off">
            <div>
                <label for="username">姓名：</label>
                <input type="text" id="username" name="username" value="" placeholder="请在此输入姓名" required/>
            </div>

            <div>
                <label for="password">密码：</label>
                <input type="password" id="password" name="password" value="" placeholder="请在此输入密码" required/>
            </div>

            <div>
                <label for="email">邮箱：</label>
                <input type="email" id="email" name="email" value="" placeholder="请在此输入邮箱" required/>
            </div>

            <div>
                <label for="tel">手机：</label>
                <input type="tel" id="tel" name="tel" value="" placeholder="请在此输入手机" required/>
            </div>
            <hr/>

            <div>
                <label for="gender">性别：</label>
                <input type="radio" id="gender" name="gender" value="man"/>男 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                <input type="radio" name="gender" value="women"/>女&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
            </div>

            <div>
                <label for="hobby">性别：</label>
                <input type="checkbox" id="hobby" name="hobby" value="music"/>音乐
                <input type="checkbox" name="hobby" value="movie"/>电影
                <input type="checkbox" name="hobby" value="game"/>游戏
            </div>

            <div>
                <label for="birthday">出生日期：</label>
                <input type="date" id="birthday" name="birthday" value=""/>
            </div>

            <div>
                <label for="city">所在城市：</label>
                <select id="city" name="city">
                    <option>---请选择所在城市---</option>
                    <optgroup label="直辖市">
                        <option>北京</option>
                        <option>上海</option>
                        <option>广州</option>
                        <option>深圳</option>
                    </optgroup>
                    <optgroup label="省会市">
                        <option>西安</option>
                        <option>杭州</option>
                        <option>郑州</option>
                        <option>武汉</option>
                    </optgroup>
                </select>
            </div>
            <hr/>

            <div>
                <label for="desc">个性签名：</label>
                <textarea id="desc" name="desc" rows="5" cols="40" placeholder="请写下您的与众不同"></textarea>
            </div>
            <hr/>
            <button type="submit">注册</button>
            <button type="reset">重置</button>
        </form>
    </div>
</body>
</html>
```

